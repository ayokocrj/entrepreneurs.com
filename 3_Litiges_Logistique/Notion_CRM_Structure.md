# Structure de la Base de Données Notion (Dictionnaire de Données)
*Composant de l'espace de démonstration "D2C Operations OS"*

Ce document présente la structure des tables et des propriétés configurées dans Notion pour suivre les expéditions de colis et gérer l'envoi automatique de litiges.

---

## 🗺️ Schéma Relationnel

```
[Base Expéditions Colis] (1) <----> (N) [Base Litiges Transporteurs]
           |
           v (1)
[Base Clients & Codes Promo]
```

---

## 🗄️ Dictionnaire de la Base de Données

### 1. Base Principale : `Expéditions Colis` (ID: `db_shipping_201`)
Contient les informations de suivi logistique importées de Shopify et synchronisées avec les APIs de tracking.

| Nom de Propriété | Type de Propriété | Description | Options / Formules |
| :--- | :--- | :--- | :--- |
| **Commande ID** | `Title` | Numéro de commande Shopify. | Clé primaire. |
| **Nom du Client** | `Text` | Nom et prénom du destinataire. | Import Shopify. |
| **Email Client** | `Text` | Adresse e-mail pour l'envoi des excuses. | Format Email. |
| **Transporteur** | `Select` | Société en charge de la livraison. | `Colissimo`, `Mondial Relay`, `Chronopost` |
| **Numéro de Suivi** | `Text` | Code de tracking pour l'appel API. | Code unique. |
| **Date d'Expédition**| `Date` | Date d'envoi du colis depuis l'entrepôt. | Date. |
| **Date de Livraison**| `Date` | Date de réception réelle par le client. | Date. |
| **Délais Contractuel**| `Number` | Nombre de jours maximum garantis. | ex: `2` (pour 48h) |
| **Jours de Transit** | `Formula` | Nombre de jours réel entre envoi et livraison. | Formula: `dateBetween(prop("Date de Livraison"), prop("Date d'Expédition"), "days")` |
| **Retard Détecté** | `Formula` | Détermine si le colis a dépassé la date limite. | Formula: `prop("Jours de Transit") > prop("Délais Contractuel")` |

---

### 2. Base de Support : `Litiges Transporteurs` (ID: `db_litiges_202`)
Permet de piloter la rédaction des réclamations et de suivre l'état de recouvrement des frais de livraison.

| Nom de Propriété | Type de Propriété | Description | Options / Formules |
| :--- | :--- | :--- | :--- |
| **Litige ID** | `Title` | Référence unique de la réclamation. | Clé primaire. |
| **Expédition** | `Relation` | Lien vers la fiche de suivi associée. | Relation avec `Expéditions Colis` |
| **Statut Litige** | `Select` | État d'avancement de la réclamation. | `À rédiger`, `Soumis`, `Remboursé`, `Rejeté` |
| **Lettre de Réclamation**| `Text` (Long) | Contenu du courrier juridique rédigé par l'IA. | Markdown. |
| **Frais de Port HT** | `Number` | Montant des frais de port facturés pour la commande. | Format : Devise (€) |
| **Montant Récupéré** | `Number` | Somme reversée par le transporteur. | Format : Devise (€) |
| **Motif Rejet** | `Text` | Raison donnée par le transporteur si refus. | Texte libre. |

---

### 3. Base Clients : `Clients & Codes Promo` (ID: `db_coupons_203`)
Gère l'historique des codes promo créés sur Shopify et envoyés aux clients mécontents.

| Nom de Propriété | Type de Propriété | Description | Options / Formules |
| :--- | :--- | :--- | :--- |
| **Client ID** | `Title` | Identifiant du client sur Shopify. | Clé primaire. |
| **Code Promo Généré**| `Text` | Code promo unique de compensation. | ex: `EXCUSE-JD938` |
| **Statut Envoi** | `Select` | État d'envoi de l'e-mail d'excuse. | `En attente`, `Envoyé`, `Erreur` |
| **Brouillon E-mail** | `Text` (Long) | Message personnalisé rédigé par l'IA. | Texte brut. |
| **Date d'Envoi** | `Date` | Date et heure de l'envoi de l'e-mail. | Date. |

---

## 🛠️ Instructions de Configuration Notion
1. Créez les trois tables dans Notion.
2. Créez les relations indiquées. 
3. Configurez les formules de calcul des jours de transit et de détection de retard dans la table `Expéditions Colis`.
4. Autorisez l'accès en lecture/écriture de n8n à ton espace Notion.
