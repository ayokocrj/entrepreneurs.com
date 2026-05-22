# Architecture Globale & Design Système - Gestionnaire de Litiges
*Document de cadrage technique initial et de flux de données*

Ce document détaille l'architecture macro, les calculs de retour sur investissement (ROI) financiers et les flux de données (DFD) liés à la gestion des litiges logistiques.

---

## 📈 Analyse de Valeur Business (ROI)
Pour une marque E-commerce (D2C) expédiant **5 000 colis par mois** :
* **Taux moyen de retard (Chronopost/Colissimo) :** ~5%, soit **250 colis en retard par mois**.
* **Montant moyen remboursable des frais de port :** ~8,00 € par colis.
* **Manque à gagner mensuel (si réclamations non faites) :** **2 000 € HT** de frais de port perdus par mois.
* **Temps humain nécessaire pour faire 250 réclamations manuelles :** ~25 heures par mois (à 20 €/h = 500 €).
* **Coût de l'automatisation IA (n8n + API tracking + GPT-4o-mini) :** **~18 € par mois**.
* **Gain financier net récupéré :** **1 982 € HT / mois** (Récupération directe sur le P&L + amélioration de la LTV client grâce à l'envoi immédiat du code de réduction de compensation).

---

## 🏛️ Architecture Macro (Niveau 0)

```mermaid
graph TD
    Shopify[Shopify Engine] -->|Webhook Commande Expédiée| n8n[Orchestrateur n8n]
    n8n <-->|Statut de Livraison en Temps Réel| Carrier[APIs Colissimo / Mondial Relay]
    n8n -->|Création Logs & Suivi| Notion[Notion D2C Operations OS]
    n8n <-->|Rédaction Lettre de Litige & Excuse| LLM[OpenAI GPT-4o-mini]
    n8n -->|Création Code Promo Unique| Shopify
    n8n -->|Envoi E-mail d'Excuse| Mail[Email API - Resend / SendGrid]
```

---

## 🔄 Flux de Données Détaillé

### Niveau 1 : Ingestion des Colis et Suivi

```mermaid
sequenceDiagram
    autonumber
    Shopify->>n8n: Webhook : Commande expédiée (Tracking Code + Email)
    n8n->>Notion: Crée une ligne dans la table "Expéditions Colis" (Statut: En cours)
    loop Chaque nuit à 2h00
        n8n->>Carrier: Requête API de suivi avec le "Numéro de Suivi"
        Carrier-->>n8n: Renvoie l'historique des statuts de livraison
        n8n->>Notion: Met à jour les dates et vérifie si "Retard Détecté" est True
    end
```

### Niveau 2 : Traitement d'un Retard Détecté

```mermaid
graph TD
    A[Retard Détecté dans Notion] --> B[n8n déclenche la boucle Litige]
    B --> C[n8n appelle Shopify API : Crée un code promo unique de 15%]
    C --> D[n8n appelle OpenAI : Rédige le brouillon de l'e-mail d'excuse avec le code]
    D --> E[n8n appelle OpenAI : Rédige le courrier de réclamation pour le transporteur]
    E --> F[Mise à jour de la table 'Litiges Transporteurs' avec la lettre de réclamation]
    F --> G{Le mode Envoi Client est-il automatique ?}
    G -->|Oui| H[Envoi de l'e-mail au client]
    G -->|Non| I[Notification Slack pour validation manuelle de Julie]
```

---

## 🛡️ Règles d'Ingénierie & Robustesse
* **Gestion des Limites de Requêtes APIs (Rate Limiting) transporteurs :** Les serveurs de tracking des transporteurs bloquent les requêtes massives répétées. Le workflow n8n implémente une file d'attente (Queue Node) avec une temporisation de 500 ms entre chaque appel API pour éviter les blocages d'IP.
* **Auto-Correction des Statuts Suspects :** Si un transporteur indique un statut de livraison vide ou inconnu, le système ne crée pas d'erreur mais place le colis en statut "À revérifier" et retente l'appel 12 heures plus tard avec une requête élargie.
