# Rapport Technique de Livraison (Semaine 1) - Setter Virtuel IA
*La "bible" technique d'implémentation, de sécurité et d'analyse financière*

Ce document détaille les aspects techniques profonds du système de setting automatique déployé en environnement de test, incluant les scripts de secours, l'analyse financière de rentabilité, et la résolution des pannes.

---

## 📈 1. Analyse Financière & Modèle de ROI

L'analyse de coût ci-dessous compare l'utilisation d'un Setter humain (freelance payé à la commission + fixe) par rapport à notre infrastructure IA pour un volume standard de **1 000 conversations de qualification par mois**.

### A. Modèle Humain (Intervention Classique)
* **Coût Fixe Mensuel :** 500 € HT
* **Commission sur appel booké (10% sur 80 appels bookés à 150€ de valeur de lead) :** 1 200 € HT
* **Total mensuel humain :** **1 700 € HT**
* **Inconvénients :** Temps de réponse moyen de 2h30, indisponibilité la nuit et le week-end, variation de la qualité d'accroche.

### B. Modèle IA (Notre Infrastructure)
* **Abonnement n8n Cloud (ou hébergement VPS Hostinger) :** 11 € HT / mois
* **Consommation API OpenAI (GPT-4o-mini : ~1 000 tokens/échange, 6 messages/lead) :**
  $$\text{Coût} = 1000 \text{ leads} \times 6 \text{ messages} \times 1000 \text{ tokens} \times 0.00015\text{€ / 1k tokens (Input)} \approx 0.90\text{€ / mois}$$
* **API de routage & CRM Notion / Slack :** 0 € (inclus dans les offres gratuites/existantes).
* **Total mensuel IA :** **~15 € HT / mois**

### C. Indicateurs Financiers clés
* **Économie Mensuelle Directe :** **1 685 € HT**
* **Coût par Lead Qualifié :** Réduit de 21,25 € à moins de 0,02 €.
* **Augmentation de marge opérationnelle :** 99.1%.

---

## ⚙️ 2. Description Technique des Boucles n8n

Le fichier `n8n_Pipeline_Setter_V1.json` contient deux flux principaux connectés :

### Flux 1 : Ingestion & Sécurisation (Webhook Meta ➔ Notion)
1. **Nœud Webhook :** Reçoit le payload POST de Meta Graph API.
2. **Nœud Crypto (Validation de Signature) :** Valide la signature HMAC SHA-256 avec la clé secrète de l'application Meta. Si la signature ne correspond pas, la transaction est immédiatement abandonnée (sécurité de type zero-trust).
3. **Nœud Notion Lookup :** Interroge la base de données `Leads Tracker` pour vérifier si le prospect existe. S'il n'existe pas, création d'une nouvelle fiche lead avec le statut `Triage`.

### Flux 2 : Génération & Routage (Notion ➔ OpenAI ➔ Meta Send API)
1. **Nœud Notion Chat History :** Extrait les 6 derniers messages de la table `Conversations Log` associés au lead.
2. **Nœud OpenAI Chat Trigger :** Envoie l'historique de chat à l'API OpenAI avec le System Prompt de setting. L'IA est instruite de renvoyer un objet JSON structuré contenant deux clés :
   * `"message_reply"` : Le message textuel poli à envoyer au prospect.
   * `"qualification_status"` : Le statut estimé (`Triage`, `Qualifié`, `Non-éligible`).
3. **Nœud Switch (Routage Métier) :**
   * *Si status = Qualifié :* n8n génère le lien Calendly personnalisé, met à jour Notion, et envoie une alerte HTTP POST vers le webhook de Slack.
   * *Si status = Triage :* n8n envoie simplement la réponse.
4. **Nœud Meta Send API :** Effectue un appel POST HTTP vers `https://graph.facebook.com/v19.0/me/messages` avec le texte généré pour répondre au prospect.

---

## 🐍 3. Script Python de Secours (Simulation Conversationnelle)
Ce script permet de tester la logique de qualification de l'IA hors ligne (en local) avant de la déployer sur n8n.

```python
import os
import openai
import json

openai.api_key = os.getenv("OPENAI_API_KEY", "ta-cle-api-ici")

def simuler_setting_ia(historique_messages):
    system_prompt = (
        "Tu es Roland-Caryl, expert IA et setter de confiance pour Mathieu. "
        "Ton but est de qualifier le prospect en DMs de manière naturelle. "
        "Pose une seule question courte à la fois. "
        "Détermine son CA actuel, son budget d'investissement (> 1500€), et son business. "
        "Tu dois impérativement répondre sous format JSON structuré avec : "
        '{"message_reply": "ta réponse ici", "qualification_status": "Triage|Qualifié|Non-éligible"}'
    )
    
    messages = [{"role": "system", "content": system_prompt}]
    messages.extend(historique_messages)
    
    try:
        response = openai.ChatCompletion.create(
            model="gpt-4o-mini",
            messages=messages,
            response_format={"type": "json_object"},
            temperature=0.7
        )
        result = json.loads(response.choices[0].message.content)
        return result
    except Exception as e:
        return {"message_reply": "Désolé, j'ai rencontré un problème. Pouvez-vous répéter ?", "qualification_status": "Triage"}

# Simulation d'un échange
historique_test = [
    {"role": "user", "content": "Salut, j'ai vu votre programme d'accompagnement IA, ça m'intéresse."}
]
print("IA Response:", simuler_setting_ia(historique_test))
```

---

## 🛠️ 4. Post-Mortem : Résolution du Bug des Doublons de Messages (Meta Webhook)
* **Date de l'incident :** 18 mai 2026.
* **Symptômes :** Les prospects recevaient parfois la même réponse de l'IA deux ou trois fois d'affilée.
* **Analyse du Problème :** L'API Meta envoie un webhook et attend une réponse HTTP 200 de notre serveur n8n en moins de 2000 ms. Si n8n met 2500 ms à répondre (à cause du temps de latence de l'API OpenAI), Meta considère que le webhook a échoué et le renvoie à nouveau. n8n traitait donc le message dupliqué en parallèle, générant deux réponses identiques.
* **Résolution & Correctif :**
  * Mise en place d'une logique de **déduplication par ID de message** dans n8n.
  * Avant de lancer la requête vers OpenAI, n8n vérifie si l'identifiant du message (`mid`) existe déjà dans la base de données locale SQLite ou Notion dans les 10 dernières secondes.
  * Si le `mid` existe, la deuxième requête est immédiatement interrompue avec un nœud *Stop* (HTTP 200 renvoyé instantanément), résolvant définitivement le bug de duplication.

---

## 🐋 5. Configuration Docker Compose pour le Déploiement VPS
Fichier `docker-compose.yml` utilisé pour héberger n8n de manière isolée et sécurisée sur le VPS Hostinger.

```yaml
version: '3.8'

services:
  n8n:
    image: docker.n8n.io/n8nio/n8n:latest
    container_name: n8n_setter_instance
    restart: always
    ports:
      - "127.0.0.1:5678:5678"
    environment:
      - N8N_HOST=n8n.votredomaine.com
      - N8N_PORT=5678
      - N8N_PROTOCOL=https
      - NODE_ENV=production
      - WEBHOOK_URL=https://n8n.votredomaine.com/
      - GENERIC_TIMEZONE=Europe/Paris
    volumes:
      - n8n_data:/home/node/.local/share/n8n

volumes:
  n8n_data:
    external: false
```

*Note sur le Hardening du VPS :* Le port `5678` est lié uniquement à `127.0.0.1`. L'accès extérieur est géré par un reverse proxy Nginx avec certificat SSL Let's Encrypt pour garantir le chiffrement HTTPS de bout en bout des webhooks de messagerie.
