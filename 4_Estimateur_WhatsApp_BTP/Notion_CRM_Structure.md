# Structure Notion CRM - BatiSuivi OS
*Schéma d'ingénierie et dictionnaire de données pour la gestion de chantiers BTP*

Ce document définit la structure des bases de données de l'espace Notion **BatiSuivi OS** permettant d'alimenter l'automatisation de devis et la communication WhatsApp.

---

## 🗄️ Architecture des Bases de Données (Modèle Entité-Association)

Le CRM est composé de **4 bases de données interconnectées** :

```
[Clients & Prospects] ──(1:N)── [Projets & Estimations] ──(N:1)── [Grille Tarifaire]
         │
       (1:N)
         │
[Logs & Vocaux]
```

---

## 1. Table : `Clients & Prospects`
*Stocke les coordonnées des prospects et le statut général de la relation.*

| Nom de la Propriété | Type | Rôle / Description | Exemple |
| :--- | :--- | :--- | :--- |
| `Nom Client` | Title | Nom et prénom du prospect (clé principale) | Jean Dupuis |
| `Numéro WhatsApp` | Phone | Numéro de téléphone formaté international | +33612345678 |
| `Email` | Email | Adresse de messagerie pour devis officiel | jean.dupuis@gmail.com |
| `Adresse de Facturation` | Text | Adresse complète pour le devis PDF | 12 Rue des Alpes, 38000 Grenoble |
| `Projets` | Relation | Lien vers la table `Projets & Estimations` | Projet Rénovation Salon |
| `Logs de Communication`| Relation | Historique des messages textuels et vocaux | Log #2492 |

---

## 2. Table : `Projets & Estimations`
*Détaille chaque projet, le calcul des coûts estimatifs et le statut du devis.*

| Nom de la Propriété | Type | Rôle / Description | Exemple |
| :--- | :--- | :--- | :--- |
| `Code Projet` | Title | Code unique généré par l'automatisation | PROJ-2026-004 |
| `Client` | Relation | Lien vers le client associé | Jean Dupuis |
| `Type de Travaux` | Select | Nature principale des travaux | Peinture / Carrelage / Cloison |
| `Analyse Visuelle IA` | Rich Text | Description technique des photos par GPT-4o Vision | "Mur placo avec fissures légères..." |
| `Surface (m²)` | Number | Surface estimée ou saisie manuellement | 45.0 |
| `Prestation Grille` | Relation | Lien vers le tarif unitaire applicable dans la grille | Peinture bicouche mur (m²) |
| `Forfaits Extra` | Multi-Select| Suppléments éventuels | Dépose papier peint / Lessivage |
| `Montant Estimé HT` | Formula | Calcul automatique (`Surface` * `Tarif Unitaire`) + `Forfaits` | 1 575,00 € |
| `TVA (%)` | Select | Taux applicable (20% standard, 10% rénovation) | 10% |
| `Montant TTC` | Formula | Calcul final TTC (`HT` * (1 + `TVA`)) | 1 732,50 € |
| `Statut Devis` | Status | Cycle de vie du devis | `Brouillon` ➔ `Approuvé` ➔ `Envoyé` ➔ `Signé` |
| `Lien Devis PDF` | URL | Lien vers le PDF généré stocké sur le cloud | S3 / Google Drive Link |
| `Version Devis` | Number | Index de révision du devis (incrémentation auto) | 1 |

---

## 3. Table : `Grille Tarifaire`
*Catalogue de référence pour les prix unitaires de chaque type de prestation.*

| Nom de la Propriété | Type | Rôle / Description | Exemple |
| :--- | :--- | :--- | :--- |
| `Prestation` | Title | Nom de la prestation de base | Peinture bicouche mur |
| `Unité` | Select | Unité d'œuvre pour le calcul | m² / ml / Forfait |
| `Tarif Unitaire HT` | Number | Prix unitaire facturé | 35.00 € |
| `Type Support` | Select | Catégorie de travaux | Peinture / Plâtrerie / Revêtement sol |

---

## 4. Table : `Logs & Vocaux`
*Stocke tous les messages WhatsApp entrants, sortants et la transcription des notes vocales.*

| Nom de la Propriété | Type | Rôle / Description | Exemple |
| :--- | :--- | :--- | :--- |
| `ID Log` | Title | ID unique du message ou de l'appel | MSG-930492 |
| `Client` | Relation | Client associé au message | Jean Dupuis |
| `Type Message` | Select | Type de média reçu | Texte / Vocal / Photo / PDF / Appel VoIP |
| `Contenu Message` | Rich Text | Texte du message ou transcription Whisper | "Bonjour, je voudrais peindre ma cuisine..." |
| `Fichier Vocal` | Files & Media| Fichier audio mp3/ogg du vocal pour écoute | vocal_382.ogg |
| `Confidence Score` | Number | Score de confiance de la transcription Whisper | 0.96 |
| `Date d'Enregistrement`| Date | Date et heure de réception | 22/05/2026 15:30 |

---

## 🔗 Instructions pour la Synchronisation & Relations
1. **Liaison Client-Projet :** La relation entre `Clients & Prospects` et `Projets & Estimations` doit être bidirectionnelle. Cela permet de voir l'ensemble des devis passés directement depuis la fiche d'un client.
2. **Calcul Dynamique du Devis :** La formule du champ `Montant Estimé HT` doit récupérer automatiquement le `Tarif Unitaire HT` de la `Grille Tarifaire` sélectionnée via une relation Rollup et le multiplier par la `Surface (m²)` saisie. 
3. **Mise à Jour Automatique du Statut :** Lorsque le webhook n8n détecte la signature du devis (via Yousign ou signature électronique intégrée au PDF), le statut de `Projets & Estimations` doit automatiquement passer de `Envoyé - En Attente de Signature` à `Signé - En cours de planification`.
