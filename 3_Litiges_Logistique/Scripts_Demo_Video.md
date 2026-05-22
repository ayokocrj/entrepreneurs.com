# Scripts des Vidéos de Démonstration (V1) - Gestionnaire de Litiges Logistiques IA
*Support narratif et visuel pour l'enregistrement des démos clients et techniques*

Ce document contient les scripts complets pour deux formats de vidéos explicatives : le format technique (destiné aux développeurs/décideurs techniques) et le format d'usage client (destiné aux équipes support client et aux dirigeants).

---

## 🎬 Vidéo 1 : Présentation Technique & Infrastructure (Durée : ~3 min)
**Public ciblé :** Thomas (Fondateur) & Roland-Caryl (Enregistrement en direct de l'écran)

### Acte 1 : Architecture Système & Ingestion Shopify (0:00 - 1:00)
* **Visuel à l'écran :** Tableau de bord n8n avec le workflow d'ingestion.
* **Discours :**
  > "Salut Thomas, je te présente l'envers du décor de notre Gestionnaire de Litiges Logistiques. Le système commence ici, sur n8n, avec un webhook branché sur Shopify qui écoute l'événement 'Commande Expédiée'. 
  > Dès qu'un colis quitte l'entrepôt, on enregistre les données de suivi dans notre base de données Notion miroir et on initialise le statut de livraison. Tout est hébergé de manière sécurisée sous Docker avec une base Postgres isolée pour garantir qu'aucune donnée de tracking ne soit perdue."

### Acte 2 : La Boucle de Tracking & Gestion du Rate Limiting (1:00 - 2:00)
* **Visuel à l'écran :** Zoom sur le nœud cron nocturne et le nœud de temporisation (Wait/Queue Node) dans n8n.
* **Discours :**
  > "Chaque nuit à 2h00, n8n lance une boucle de vérification. Il interroge les APIs des transporteurs comme Colissimo ou Mondial Relay pour récupérer le statut de livraison en temps réel. 
  > Pour éviter que les serveurs des transporteurs ne bloquent notre adresse IP en pensant qu'il s'agit d'une attaque, j'ai configuré un nœud de file d'attente avec un délai de 500 ms entre chaque colis. Si l'API renvoie un statut vide ou incohérent, notre script Python de secours intercepte l'erreur, nettoie la donnée et reprogramme la requête 12 heures plus tard."

### Acte 3 : Génération de Code Promo & Génération de Courriers par l'IA (2:00 - 3:00)
* **Visuel à l'écran :** Fichier de prompt OpenAI dans n8n, puis démonstration de la création d'un code promo via l'API Shopify.
* **Discours :**
  > "Dès qu'un retard est formellement détecté au-delà des CGV, le système appelle l'API Shopify pour créer un code promotionnel unique de 15%. 
  > En parallèle, on fait appel à GPT-4o-mini pour deux tâches : rédiger l'e-mail d'excuse personnalisé destiné au client final, et rédiger la mise en demeure officielle pour le transporteur avec toutes les bases juridiques et les numéros de suivi. Le workflow est entièrement sauvegardé sur notre dépôt Git pour garantir un déploiement fluide."

---

## 🎬 Vidéo 2 : Guide d'Utilisation Client (Durée : ~3 min)
**Public ciblé :** Julie (Responsable Customer Support) & Équipe Support

### Acte 1 : Le Tableau de Bord Notion et la Détection Automatique (0:00 - 1:00)
* **Visuel à l'écran :** Espace Notion 'D2C Operations OS', onglet 'Suivi des Livraisons' avec les différents statuts.
* **Discours :**
  > "Bonjour Julie, je te montre comment le système va te faire gagner des heures et sauver la relation avec nos clients au quotidien. 
  > Plus besoin de vérifier les numéros de suivi un par un. Ton espace Notion 'D2C Operations OS' regroupe tout automatiquement. Les colis en cours de livraison, ceux livrés en retard, et ceux pour lesquels une demande de remboursement a été initiée auprès du transporteur sont triés pour toi."

### Acte 2 : L'Alerte Slack et la Validation Semi-Automatique (1:00 - 2:00)
* **Visuel à l'écran :** Notification reçue sur Slack dans le canal `#sav-shipping-delays`, puis clic vers la fiche client Notion.
* **Discours :**
  > "Dès qu'un colis dépasse la date limite de livraison, le système t'envoie une notification immédiate sur Slack dans `#sav-shipping-delays`. 
  > En cliquant dessus, tu arrives sur sa fiche client Notion. L'IA a déjà généré un brouillon d'e-mail d'excuse personnalisé incluant le code promo de 15% créé spécialement sur Shopify. Si tout est bon pour toi, tu as juste à cliquer sur le bouton 'Envoyer l'E-mail d'Excuse' et le client reçoit son message d'excuse instantanément."

### Acte 3 : Suivi des Remboursements Transporteurs & Traitement des Exceptions (2:00 - 3:00)
* **Visuel à l'écran :** Section 'Réclamations' dans Notion avec les boutons d'action rapide et la gestion des exceptions.
* **Discours :**
  > "Pour la partie financière, le système a également rédigé la réclamation transporteur officielle. Une fois que le transporteur nous a remboursé la livraison, il te suffit de passer le statut à 'Remboursé' et de noter le montant dans la fiche Notion. 
  > Si un client refuse le bon de réduction et exige un remboursement direct de ses frais de port, tu coches simplement la case 'Remboursement Partiel Stripe' sur sa fiche Notion. Le système se charge d'annuler les frais sur Stripe et Shopify en tâche de fond. C'est simple, rapide, et tu gardes le contrôle absolu sur notre relation client !"
