# Architecture Globale & Design Système - Setter Virtuel IA
*Document de cadrage technique initial et de flux de données*

Ce document détaille l'architecture macro, les calculs de rentabilité business (ROI) et les diagrammes de flux de données (DFD) pour l'automatisation du triage et du setting.

---

## 📈 Analyse de Valeur Business (ROI)
L'intégration de ce Setter Virtuel a deux objectifs financiers majeurs :
1. **Élimination du lead-lag (temps de réponse) :** Diviser par 120 le temps de réponse aux DMs (de 4 heures à moins de 2 minutes), ce qui augmente statistiquement le taux de booking de 30%.
2. **Économie de ressources :** Pour une agence ou un infopreneur, employer un setter humain à plein temps représente un coût de 1 500 € à 2 500 € par mois (salaire ou commission). L'infrastructure technique (n8n + API) coûte moins de 50 € par mois pour traiter un volume équivalent, soit une marge opérationnelle augmentée de plus de 90%.

---

## 🏛️ Architecture Macro (Niveau 0)

```mermaid
graph LR
    User[Prospect Instagram/LinkedIn] <-->|DMs Messagerie| Meta[API Gateway Meta/LinkedIn]
    Meta <-->|Webhooks / Messages| n8n[Moteur d'Orchestration n8n]
    n8n <-->|Analyse & RAG| LLM[OpenAI GPT-4o-mini]
    n8n -->|Logs & Contacts| Notion[Notion CRM Leads OS]
    n8n -->|Alertes Leads Chauds| Slack[Slack Commercial]
```

---

## 🔄 Flux de Données Détaillé

### Niveau 1 : Traitement d'un Message Entrant

```mermaid
sequenceDiagram
    autonumber
    Prospect->>Meta API: Envoie un DM privé
    Meta API->>n8n Webhook: Déclenche webhook (JSON Payload)
    n8n Webhook->>Notion CRM: Vérifie si le prospect existe et si la case "Désactiver le Bot" est False
    Notion CRM-->>n8n Webhook: Renvoie l'historique des 5 derniers messages
    n8n Webhook->>LLM (GPT-4o-mini): Envoie l'historique + prompt système (Directives de qualification)
    LLM-->>n8n Webhook: Renvoie la réponse rédigée + JSON de statut de qualification
    n8n Webhook->>Meta API: Envoie la réponse au prospect en DM
    n8n Webhook->>Notion CRM: Enregistre le message et met à jour le statut du Lead
```

### Niveau 2 : Logique de Routage Métier (Triage & Alertes)

```mermaid
graph TD
    A[Message Reçu & Analysé par l'IA] --> B{Le prospect a-t-il partagé ses métriques ?}
    B -->|Non| C[Poursuivre la conversation naturellement pour qualifier]
    B -->|Oui| D{Le prospect est-il éligible ? CA > 5k€/mois}
    
    D -->|Oui : Lead Chaud| E[Générer le lien Calendly dans la réponse]
    E --> F[Envoyer une alerte Slack dans #leads-priority]
    F --> G[Passer le statut Notion à 'À Relancer']
    
    D -->|Non : Lead Froid| H[Répondre poliment en proposant une ressource gratuite]
    H --> I[Ajouter le prospect à la liste de Nurturing Email]
    I --> J[Passer le statut Notion à 'Non-Éligible']
```

---

## 🛡️ Règles d'Ingénierie & Sécurité
* **Vérification de Signature Meta :** Pour sécuriser le webhook n8n, nous validons le header de signature X-Hub-Signature-256 en utilisant une clé secrète partagée avec Meta. Cela empêche toute injection de requêtes factices.
* **Verrou de Concurrence (Anti-Boucle) :** Si un utilisateur envoie 3 messages d'affilée en 10 secondes, le système utilise un cache de transaction (Airtable/Redis ou variable interne n8n) pour ne répondre qu'une seule fois au bloc de messages cumulés.
* **Vérification Humaine Prioritaire :** Toute écriture de message manuel par un commercial dans le chat désactive le bot pendant 24h par l'écriture d'un cookie temporel dans la fiche Notion.
