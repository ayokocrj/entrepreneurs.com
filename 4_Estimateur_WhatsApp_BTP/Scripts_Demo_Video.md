# Scripts des Vidéos de Démonstration (V1) - Estimateur WhatsApp BTP IA
*Support narratif et visuel pour l'enregistrement des démos clients et techniques*

Ce document contient les scripts complets pour deux formats de vidéos explicatives : le format technique (destiné aux développeurs/décideurs techniques) et le format d'usage client (destiné aux équipes opérationnelles et de direction).

---

## 🎬 Vidéo 1 : Présentation Technique & Infrastructure (Durée : ~3 min)
**Public ciblé :** Jean-Pierre (Fondateur) & Roland-Caryl (Enregistrement en direct de l'écran)

### Acte 1 : Infrastructure & File d'Attente Asynchrone (0:00 - 1:00)
* **Visuel à l'écran :** Terminal montrant `docker compose ps` avec les 3 services (postgres, n8n, gotenberg), puis bascule sur le webhook n8n de réception.
* **Discours :**
  > "Salut Jean-Pierre, je te présente l'architecture technique de notre Estimateur BTP. Afin de respecter la contrainte de Meta qui exige une réponse HTTP 200 en moins de 3 secondes pour les webhooks WhatsApp, j'ai découplé l'ingestion de la logique d'analyse. 
  > Notre webhook n'effectue aucun calcul lourd à la réception : il enregistre le message et acquitte immédiatement l'appel. C'est ensuite cette file d'attente qui va distribuer le travail de manière asynchrone."

### Acte 2 : Transcription Whisper & Analyse Vision GPT-4o (1:00 - 2:00)
* **Visuel à l'écran :** n8n montrant la branche de transcription audio avec le script Python de validation ffmpeg, puis la branche vision.
* **Discours :**
  > "Si le client envoie un message vocal, le fichier audio Opus est converti à la volée en MP3 mono via ffmpeg pour correspondre aux spécifications de l'API OpenAI Whisper. 
  > Si le client envoie une photo, l'image est redimensionnée en JavaScript pour limiter la charge de tokens et transmise à GPT-4o-vision. L'IA extrait la nature des matériaux et l'état des surfaces pour estimer la difficulté du chantier, ce qui nous permet de pré-qualifier le niveau de préparation requis."

### Acte 3 : Moteur PDF Gotenberg & Intégration Notion (2:00 - 3:00)
* **Visuel à l'écran :** Requête API vers Gotenberg pour générer le devis, puis mise à jour de la base de données Notion.
* **Discours :**
  > "Une fois le calcul validé par les formules Notion, n8n transmet le HTML structuré au conteneur Gotenberg. Gotenberg génère un PDF haute définition avec notre charte graphique. 
  > Le fichier est déposé sur un bucket sécurisé et son lien est injecté dans la fiche Notion correspondante. Les clés d'API et variables d'environnement sont isolées dans un fichier `.env` sécurisé à la racine de notre serveur Docker."

---

## 🎬 Vidéo 2 : Guide d'Utilisation Client (Durée : ~3 min)
**Public ciblé :** Amélie (Secrétaire Administrative) & Jean-Pierre (Fondateur)

### Acte 1 : Ingestion des Leads et Visualisation dans Notion (0:00 - 1:00)
* **Visuel à l'écran :** Interface WhatsApp Web montrant un client envoyant un message vocal et une photo d'un mur endommagé, puis bascule vers Notion 'BatiSuivi OS'.
* **Discours :**
  > "Bonjour Amélie, bonjour Jean-Pierre. Je vous montre comment le système qualifie vos chantiers et prépare vos devis en temps réel. 
  > Un client nous contacte sur WhatsApp et nous envoie un message vocal pour expliquer son projet de peinture, accompagné de photos. Instantanément, le système capture son message, transcrit le vocal grâce à l'IA et analyse la photo. Dans notre outil Notion, la fiche client est automatiquement créée avec les surfaces estimées et le détail des supports."

### Acte 2 : Ajustement Tarifaire Rapide (1:00 - 2:00)
* **Visuel à l'écran :** Fiche projet Notion ouverte, modification manuelle du prix au m² et choix de l'option de préparation de support 'Dépose de tapisserie'.
* **Discours :**
  > "Ici, sur la fiche Notion, vous avez la main. L'IA a pré-calculé un prix HT en se basant sur notre grille tarifaire officielle. 
  > Si Jean-Pierre estime qu'il y a un travail de préparation supplémentaire, comme enlever de l'ancienne tapisserie, il lui suffit de cocher l'option dans le Notion. Le montant total du projet se recalcule automatiquement. C'est simple, visuel, et aucun risque d'erreur de calcul."

### Acte 3 : Envoi Automatique du Devis PDF par WhatsApp (2:00 - 3:00)
* **Visuel à l'écran :** Modification du statut Notion vers 'Approuvé pour Envoi', puis retour sur l'écran WhatsApp Web montrant le PDF qui arrive chez le client.
* **Discours :**
  > "Une fois que le devis vous convient, vous passez simplement le statut à 'Approuvé pour Envoi'. n8n génère alors le devis officiel sous forme de PDF professionnel et l'envoie directement au client sur son compte WhatsApp. 
  > Le client peut le lire et le signer en un clic. Plus besoin d'attendre des jours pour envoyer un chiffrage ! Vous gagnez un temps précieux et convertissez beaucoup plus de chantiers. Bonnes ventes à vous !"
