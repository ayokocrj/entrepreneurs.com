# Structure de la Base de Données Notion (Dictionnaire de Données)
*Composant de l'espace de démonstration "Sales Intelligence Hub"*

Ce document définit la structure des tables et des propriétés à configurer dans Notion pour faire fonctionner le pipeline de génération de propositions commerciales IA.

---

## 🗺️ Schéma Relationnel

```
[Base Dossiers Prospects] (1) <----> (N) [Base Lignes Devis]
           |
           v (1)
[Base Rapports Veille]
```

---

## 🗄️ Dictionnaire de la Base de Données

### 1. Base Principale : `Dossiers Prospects` (ID: `db_prospects_101`)
Stocke les informations générales du prospect, les notes brutes de call et le document PDF finalisé.

| Nom de Propriété | Type de Propriété | Description | Options / Formules |
| :--- | :--- | :--- | :--- |
| **Nom de l'Entreprise**| `Title` | Nom officiel du client potentiel. | Clé primaire. |
| **Site Web** | `URL` | Lien vers le site internet du client. | ex: `https://client.com` |
| **Notes de Call** | `Text` (Long) | Transcription ou notes brutes de l'appel de découverte. | Texte libre. |
| **Statut Proposition**| `Select` | Étape de traitement de la proposition. | `Brouillon`, `Générer PDF`, `Prête`, `Envoyée` |
| **Fichier PDF** | `Files & media` | Document PDF généré par l'automatisation. | Stockage de fichier. |
| **Total Devis HT** | `Rollup` | Somme des montants des lignes de devis associées. | Somme de `Montant HT` de `Lignes Devis` |
| **Rapport de Veille** | `Relation` | Lien vers l'analyse concurrentielle web. | Relation avec `Rapports Veille` (1-to-1) |
| **Forcer la Recherche**| `Checkbox` | Case pour ré-exécuter le script de scraping. | `True` / `False` |

---

### 2. Base de Support : `Lignes Devis` (ID: `db_devis_102`)
Permet de décomposer l'offre en plusieurs livrables avec des tarifs modifiables manuellement avant la génération du PDF.

| Nom de Propriété | Type de Propriété | Description | Options / Formules |
| :--- | :--- | :--- | :--- |
| **Livrable** | `Title` | Nom du service ou de l'étape du projet. | ex: "Audit Stratégique IA" |
| **Dossier Prospect** | `Relation` | Lien vers la fiche client associée. | Relation avec `Dossiers Prospects` |
| **Description** | `Text` | Détail de ce que comprend le livrable. | Texte libre. |
| **Quantité** | `Number` | Nombre d'heures, jours ou unités de service. | Valeur par défaut : `1` |
| **Tarif Unitaire HT** | `Number` | Prix de l'unité de service. | Format : Devise (€) |
| **Montant HT** | `Formula` | Calcul automatique du montant de la ligne. | Formula: `prop("Quantité") * prop("Tarif Unitaire HT")` |

---

### 3. Base Analytique : `Rapports Veille` (ID: `db_veille_103`)
Stocke les données de veille concurrentielle récupérées par l'IA sur internet.

| Nom de Propriété | Type de Propriété | Description | Options / Formules |
| :--- | :--- | :--- | :--- |
| **ID Rapport** | `Title` | Identifiant technique du rapport. | Clé primaire. |
| **Dossier Prospect** | `Relation` | Lien vers l'entreprise associée. | Relation avec `Dossiers Prospects` |
| **Concurrents Directs**| `Text` (Long) | Liste des concurrents trouvés par Perplexity API. | Markdown autorisé. |
| **Points Faibles Site** | `Text` (Long) | Erreurs ou optimisations techniques détectées sur leur site. | ex: Problème SEO, vitesse de chargement. |
| **Opportunités IA** | `Text` (Long) | Recommandations de cas d'usage IA à leur vendre. | Synthèse générée par Claude. |

---

## 🛠️ Instructions de Configuration Notion
1. Créez les trois tables dans ton espace Notion.
2. Créez les relations indiquées en veillant à ce que la suppression d'un dossier prospect nettoie également ses lignes de devis associées.
3. Configurez la formule de calcul du total HT dans la table `Dossiers Prospects` en utilisant le Rollup sur `Lignes Devis`.
4. Intégrez l'accès en lecture/écriture de ton outil d'automatisation sur ces trois bases.
