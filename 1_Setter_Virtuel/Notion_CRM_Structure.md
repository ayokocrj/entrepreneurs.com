# Structure de la Base de Données Notion (Dictionnaire de Données)
*Composant de l'espace de démonstration "Setter AI Lead OS"*

Ce document détaille l'ingénierie et le schéma relationnel des tables Notion configurées pour piloter le Setter Virtuel. Cette structure garantit que les scripts n8n lisent et écrivent les données de manière propre et cohérente.

---

## 🗺️ Schéma des Relations

```
[Base Leads] (1) <----> (N) [Base Conversations]
      |                              |
      v (N)                          v (N)
[Logs Webhooks]                [Configuration Globale]
```

---

## 🗄️ Dictionnaire de la Base de Données

### 1. Base Principale : `Leads Tracker` (ID: `db_leads_001`)
C'est la base de données centrale qui répertorie tous les prospects identifiés sur les réseaux sociaux.

| Nom de Propriété | Type de Propriété | Description | Options / Formules |
| :--- | :--- | :--- | :--- |
| **Lead Name** | `Title` (Titre) | Nom complet du prospect (ou ID social). | Clé primaire. |
| **Username** | `Text` | Identifiant Instagram/LinkedIn (ex: `@prospect_handle`). | Requis pour les requêtes API. |
| **Platform** | `Select` | Source du contact. | `Instagram`, `LinkedIn`, `Facebook` |
| **Status** | `Select` | Statut opérationnel dans le tunnel de vente. | `Triage`, `Qualifié`, `À relancer`, `Non-éligible` |
| **Score de Qualification** | `Number` | Score calculé par l'IA de 0 à 100 basé sur les critères. | Format : Pourcentage |
| **Activité** | `Text` | Type de business extrait par l'IA. | ex: "Coach Nutrition", "Agence Web" |
| **CA Estimé** | `Select` | Tranche de chiffre d'affaires du prospect. | `< 5k€`, `5k-15k€`, `> 15k€` |
| **Calendly Booked** | `Checkbox` | Coche automatique via webhook Calendly si RDV pris. | `True` / `False` |
| **Désactiver le Bot** | `Checkbox` | Case de débrayage de secours pour couper l'IA. | Déclenche un filtre n8n. |
| **Dernier Échange** | `Date` | Date et heure du dernier message reçu/envoyé. | Tri décroissant par défaut. |
| **Historique Chat** | `Relation` | Lien vers la table des messages de conversation. | Relation avec `Conversations Log` |

---

### 2. Base de Support : `Conversations Log` (ID: `db_conv_002`)
Cette table stocke l'historique brut de chaque message pour donner du contexte au LLM lors de la génération de la réponse suivante (historique de chat glissant).

| Nom de Propriété | Type de Propriété | Description | Options / Formules |
| :--- | :--- | :--- | :--- |
| **Message ID** | `Title` | Identifiant unique du message issu de l'API Meta. | Clé primaire. |
| **Lead** | `Relation` | Lien vers le prospect concerné dans la base principale. | Relation avec `Leads Tracker` (Requis) |
| **Auteur** | `Select` | Qui a rédigé le message. | `IA-Setter`, `Prospect`, `Commercial-Humain` |
| **Message Content** | `Text` (Long) | Contenu textuel du message. | Texte brut. |
| **Timestamp** | `Date` | Date et heure précise du message. | Format ISO. |

---

### 3. Base Technique : `Configuration Globale` (ID: `db_config_003`)
Contient les instructions et les variables système pour ajuster le comportement du bot sans modifier le code de n8n.

| Nom de Propriété | Type de Propriété | Description | Valeurs Types |
| :--- | :--- | :--- | :--- |
| **Config Name** | `Title` | Nom du profil d'instruction. | `Default_Setter_V1` |
| **System Prompt** | `Text` (Long) | Instructions fondamentales données à l'IA. | *"Tu es Roland-Caryl, setter pour..."* |
| **Calendly Link** | `URL` | Lien de réservation envoyé au prospect. | `https://calendly.com/roland-obam/...` |
| **Max Conversations Actives** | `Number` | Limite de sécurité pour éviter le spam. | `50` |

---

## 🛠️ Instructions de Configuration Notion
1. Créez les trois bases de données dans ton espace Notion.
2. Établissez une relation bidirectionnelle entre `Leads Tracker` et `Conversations Log`. Cochez l'option "Afficher sur Leads Tracker" pour pouvoir lire l'historique complet directement depuis la fiche client.
3. Configurez une vue Kanban sur `Leads Tracker` groupée par la propriété `Status` pour permettre à Sarah et Mathieu de suivre visuellement le pipeline en temps réel.
4. Partagez l'espace avec l'intégration n8n en lui donnant les droits d'accès en lecture/écriture.
