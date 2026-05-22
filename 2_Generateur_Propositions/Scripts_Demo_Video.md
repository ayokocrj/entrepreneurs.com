# Scripts des Vidéos de Démonstration (V1) - Générateur de Propositions
*Support narratif et visuel pour l'enregistrement des démos clients et techniques*

Ce document contient les scripts complets pour deux formats de vidéos explicatives : le format technique (destiné aux développeurs/décideurs techniques) et le format d'usage client (destiné aux équipes de vente et aux dirigeants).

---

## 🎬 Vidéo 1 : Présentation Technique & Gotenberg API (Durée : ~3 min)
**Public ciblé :** Mathieu (Fondateur) & Roland-Caryl (Enregistrement en direct de l'écran)

### Acte 1 : Introduction & Configuration Docker (0:00 - 1:00)
* **Visuel à l'écran :** n8n Workflow montrant la connexion entre le webhook Notion et l'API de Gotenberg.
* **Discours :**
  > "Salut Mathieu. Je te présente l'architecture technique derrière notre générateur de propositions commerciales. 
  > Pour éviter de payer des solutions d'édition de PDF cloud coûteuses et lentes, j'ai configuré un conteneur Gotenberg qui tourne en local sur notre VPS. C'est une instance Chromium headless super rapide, accessible directement via API locale sur le port 3000."

### Acte 2 : Le Prompting Claude & La Structuration JSON (1:00 - 2:00)
* **Visuel à l'écran :** Le nœud Anthropic Claude dans n8n avec le prompt système et la structure JSON attendue.
* **Discours :**
  > "Ici, tu peux voir comment nous utilisons Claude 3.5 Sonnet pour transformer les notes de call brutes et les rapports de veille en données exploitables. 
  > Nous lui demandons de générer un tableau JSON propre contenant les livrables recommandés, les descriptions et les chiffrages unitaires. Ensuite, n8n boucle sur ce JSON pour insérer chaque ligne directement dans la table 'Lignes Devis' de Notion, créant ainsi le lien relationnel automatiquement."

### Acte 3 : Génération HTML et Upload (2:00 - 3:00)
* **Visuel à l'écran :** Code HTML/CSS du template de proposition dans le nœud de formatage de n8n, puis le résultat PDF dans Notion.
* **Discours :**
  > "Une fois les lignes créées dans Notion, on génère un document HTML dynamique injectant les données de l'offre et les styles CSS de notre charte graphique. 
  > Ce fichier HTML est envoyé en POST à l'API de Gotenberg, qui nous retourne un fichier PDF vectoriel parfait en moins d'une seconde. n8n récupère ce binaire et le téléverse directement dans le champ fichier du dossier prospect Notion. Tout le code source et le fichier d'importation n8n sont prêts sur le dépôt Git."

---

## 🎬 Vidéo 2 : Guide d'Utilisation Client (Durée : ~3 min)
**Public ciblé :** Sophie (Directrice) & Pierre (Directeur Associé)

### Acte 1 : La soumission du Formulaire Cadrage Lead (0:00 - 1:00)
* **Visuel à l'écran :** Remplissage du formulaire Typeform/Tally avec des notes brutes de call, puis clic sur valider.
* **Discours :**
  > "Bonjour Sophie, Bonjour Pierre. Je vous présente votre nouvel outil d'aide à la vente. Fini les heures passées à rédiger des propositions commerciales après vos rendez-vous. 
  > Maintenant, à la fin de votre appel de découverte, il vous suffit de remplir ce formulaire rapide : indiquez le nom du prospect, son site web, et collez vos notes brutes d'appel ou la transcription audio."

### Acte 2 : Analyse de la Fiche Client Notion & Ajustement de l'Offre (1:00 - 2:15)
* **Visuel à l'écran :** Base Notion 'Dossiers Prospects' montrant la fiche client pré-remplie, le rapport de veille Perplexity et le tableau des prix.
* **Discours :**
  > "Une fois le formulaire validé, l'intelligence artificielle se met au travail. En moins de 60 secondes, elle va analyser le site web de votre prospect, identifier ses concurrents et insérer ces données directement sur sa fiche Notion dans la section 'Veille'. 
  > Mieux encore, l'IA va analyser vos notes d'appel pour suggérer les livrables les plus adaptés à son besoin et chiffrer l'offre. Vous pouvez modifier ces tarifs ou ajuster les textes à tout moment directement dans ce tableau Notion pour garder le contrôle total sur vos offres."

### Acte 3 : Génération et Téléchargement du PDF (2:15 - 3:00)
* **Visuel à l'écran :** Passage du statut de proposition à 'Générer PDF', puis apparition du fichier PDF finalisé.
* **Discours :**
  > "Quand vos chiffres sont validés, passez simplement le statut de la proposition à 'Générer PDF'. 
  > En tâche de fond, notre serveur va générer un document PDF parfaitement mis en page avec votre logo, votre présentation et les tarifs convenus. En moins de 30 secondes, le fichier PDF apparaît directement dans la fiche Notion. Vous n'avez plus qu'à le télécharger et à l'envoyer par e-mail à votre client. Bonnes ventes à tous !"
