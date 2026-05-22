# Scripts des Vidéos de Démonstration (V1) - Setter Virtuel IA
*Support narratif et visuel pour l'enregistrement des démos clients et techniques*

Ce document contient les scripts complets pour deux formats de vidéos explicatives : le format technique (destiné aux développeurs/décideurs techniques) et le format d'usage client (destiné aux équipes de vente et aux dirigeants).

---

## 🎬 Vidéo 1 : Présentation Technique & Infrastructure (Durée : ~3 min)
**Public ciblé :** Mathieu (Fondateur) & Roland-Caryl (Enregistrement en direct de l'écran)

### Acte 1 : Introduction & Configuration Docker (0:00 - 1:00)
* **Visuel à l'écran :** Terminal SSH connecté au VPS + Fichier `docker-compose.yml` ouvert.
* **Discours :**
  > "Salut Mathieu, je te présente la partie technique du Setter Virtuel. Comme convenu, le conteneur n8n tourne sur notre propre instance Docker isolée. Tu peux voir ici la configuration d'environnement sécurisée avec HTTPS activé via le reverse proxy Nginx. Les données sont persistées localement, ce qui garantit qu'on ne perd aucune configuration d'API en cas de redémarrage du serveur."

### Acte 2 : Le Webhook & La Déduplication (1:00 - 2:00)
* **Visuel à l'écran :** Interface n8n montrant la boucle d'ingestion et le nœud de vérification de signature.
* **Discours :**
  > "Ici, c'est le point de sécurité critique : le webhook de réception valide la signature HMAC transmise par Meta. Si quelqu'tente d'appeler l'URL directement avec des fausses requêtes, le système rejette l'appel en renvoyant une erreur 401. 
  > Ensuite, on a intégré le nœud de déduplication par Message ID (`mid`). Cela règle définitivement le problème des doublons de réponses générés lorsque Meta renvoie le webhook parce que l'API d'OpenAI met plus de 2 secondes à répondre."

### Acte 3 : La boucle de qualification & Logs (2:00 - 3:00)
* **Visuel à l'écran :** Nœud OpenAI Chat dans n8n montrant le JSON retourné et la mise à jour de la table Notion.
* **Discours :**
  > "Le cœur de la logique est ce nœud d'appel à GPT-4o-mini. Regarde le format de réponse strict qu'on lui impose en JSON. On lui demande de qualifier le lead et de modifier son statut. 
  > n8n parse ce JSON et fait un embranchement. Si le statut passe à 'Qualifié', on met à jour la base Notion et on déclenche immédiatement l'alerte Slack. Le code source est propre, versionné sur le dépôt Git, et le fichier JSON de configuration n8n est disponible dans le dossier de livraison pour une importation directe."

---

## 🎬 Vidéo 2 : Guide d'Utilisation Client (Durée : ~3 min)
**Public ciblé :** Sarah (Head of Sales) & Équipe commerciale

### Acte 1 : La Vision & Le Miro de Processus (0:00 - 1:00)
* **Visuel à l'écran :** Schéma de flux Miro montrant le parcours d'un prospect qualifié par l'IA.
* **Discours :**
  > "Bonjour Sarah, je te montre comment fonctionne le Setter Virtuel au quotidien pour t'aider à maximiser tes ventes sans perdre de temps avec les leads non qualifiés. 
  > Le système répond automatiquement à chaque nouveau contact sur nos réseaux sociaux. L'IA se présente de manière naturelle et va poser des questions discrètes sur l'activité du prospect, son chiffre d'affaires et ses besoins."

### Acte 2 : L'Alerte Slack & La Fiche Client Notion (1:00 - 2:15)
* **Visuel à l'écran :** Interface Slack avec une notification de lead chaud, puis bascule vers la base Notion Leads Tracker.
* **Discours :**
  > "Dès que l'IA détecte que le prospect a du budget et un besoin réel, elle génère un résumé de son profil et t'envoie une alerte prioritaire sur le canal Slack `#leads-priority`. 
  > En cliquant sur le lien Slack, tu arrives directement sur sa fiche dans notre espace Notion. Tu y trouveras l'historique complet des messages échangés par l'IA, le score de qualification estimé et toutes les informations sur son business. Tu peux alors décider de prendre le relais manuellement dans la messagerie LinkedIn ou Instagram pour finaliser le closing."

### Acte 3 : Paramétrage & Débrayage de Sécurité (2:15 - 3:00)
* **Visuel à l'écran :** Fiche prospect dans Notion avec la case à cocher "Désactiver le Bot", puis table de prompt globale.
* **Discours :**
  > "Si à un moment tu souhaites couper le bot pour un prospect spécifique pour lui parler toi-même, il te suffit de cocher la case 'Désactiver le Bot' sur sa fiche Notion. Le bot s'arrêtera instantanément. 
  > Et si vous souhaitez modifier l'approche du bot ou ajuster le script d'accroche global, vous pouvez le faire directement dans la table de Configuration Notion. C'est simple, intuitif, et opérationnel en temps réel. Bons close à tous !"
