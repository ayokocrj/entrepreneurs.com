# SOP : Utilisation Opérationnelle du Générateur de Propositions IA
*Guide destiné aux équipes non techniques (Sophie - Directrice & Pierre - Directeur Associé)*

Ce document est le guide d'utilisation opérationnel pour faire fonctionner le système de recherche d'avant-vente et générer des propositions commerciales personnalisées en moins de 2 minutes.

---

## 🎯 Objectif du Système
Ce système permet de supprimer la corvée de rédaction manuelle des propositions commerciales après un appel de découverte. En croisant les notes brutes de l'appel avec des recherches web en temps réel (via l'IA) sur l'entreprise du client et son marché, il génère automatiquement un dossier de proposition commerciale ultra-personnalisé et chiffré en format PDF.

---

## 📋 Processus Étape par Étape

### 1. La Saisie des Notes de Call de Découverte
À la fin de chaque appel avec un client potentiel, Pierre ou le consultant en charge doit saisir le compte-rendu dans le formulaire de cadrage.
* **Méthode :**
  1. Ouvrez le formulaire interne **Cadrage Lead** (Typeform/Tally ou formulaire Notion).
  2. Renseignez le nom du prospect, son site internet principal.
  3. Collez les notes brutes prises durant l'appel (ou chargez directement la transcription audio issue de Zoom/Meet).
  4. Cliquez sur **Soumettre**.

### 2. Consultation de l'Analyse Concurrentielle sur Notion
Dans les 60 secondes qui suivent la soumission du formulaire, le système effectue une recherche web automatisée.
1. Allez dans ton espace Notion **Sales Intelligence Hub** ➔ `Dossiers Prospects`.
2. Ouvrez la fiche du prospect concerné.
3. Consultez la section **Rapport de Veille IA** qui s'est remplie automatiquement :
   * Concurrents directs identifiés en ligne.
   * Forces et faiblesses techniques détectées sur leur site web.
   * Opportunités clés à mettre en valeur lors de ton closing.

### 3. Validation et Génération de la Proposition PDF
1. Dans la fiche Notion du prospect, vérifiez le tableau **Chiffrage & Offre** pré-rempli par l'IA d'après les besoins exprimés dans tes notes.
2. Si nécessaire, modifiez manuellement les tarifs ou les lignes de livrables dans la table.
3. Une fois les chiffres validés, passez la propriété `Statut Proposition` de **Brouillon** à **Générer PDF**.
4. Patientez 30 secondes : l'icône de chargement disparaît et le fichier **Proposition_Commerciale.pdf** apparaît en pièce jointe sur la fiche Notion.
5. Vous pouvez le télécharger et l'envoyer directement au client.

---

## 🚨 Résolution de Problèmes
* **La recherche web n'affiche rien ou fait une erreur :** Si l'URL du prospect est mal orthographiée ou si son site est protégé contre le scraping, la recherche web échouera. Modifiez l'URL dans Notion et passez le bouton `Forcer la Recherche Web` à **True** pour relancer le processus.
* **Le chiffrage proposé par l'IA ne correspond pas à nos standards :** Vous pouvez à tout moment écraser la proposition automatique en renseignant manuellement la table Notion avant d'activer la génération du PDF.
