# SOP : Utilisation Opérationnelle du Setter Virtuel IA
*Guide destiné aux équipes non techniques (Mathieu - Fondateur & Sarah - Head of Sales)*

Ce document est le guide d'utilisation au quotidien pour interagir avec le système de "setting" automatique et gérer le flux de leads qualifiés.

---

## 🎯 Objectif du Système
Le Setter Virtuel qualifie de manière autonome et naturelle 100% des messages privés (Instagram / LinkedIn) entrants. Son but est de libérer le temps de l'équipe commerciale en ne planifiant sur Calendly que les prospects répondant aux critères d'éligibilité High-Ticket, tout en alertant immédiatement les commerciaux en cas de profil hautement stratégique.

---

## 📋 Processus Étape par Étape

### 1. La Réception et Qualification des Messages
* **Fonctionnement de l'IA :** Lorsqu'un prospect envoie un message privé, le Setter Virtuel répond en moins de 2 minutes. La conversation vise à qualifier discrètement trois points clés :
  1. **Activité & Chiffre d'Affaires** (ex: coach, infopreneur, agence).
  2. **Problématique principale** (acquisition, délivrabilité, process).
  3. **Budget d'investissement** disponible.
* **Consigne Équipe :** Ne répondez pas manuellement sur l'application Instagram/LinkedIn pendant cette phase. Laissez l'IA mener le fil de discussion. Toute interaction humaine manuelle désactivera temporairement le bot sur ce prospect pendant 24 heures pour éviter les conflits de réponse.

### 2. Gestion des Alertes Slack (Prendre le relais d'un Lead Chaud)
Dès qu'un prospect est classifié comme **"Chaud (High-Ticket)"** par l'IA, le système envoie une alerte instantanée dans le canal Slack `#leads-priority` sous ce format :

> 🚨 **NOUVEAU LEAD CHAUD QUALIFIÉ**
> * **Nom :** Jean Dupont  
> * **Activité :** Agence Web (15k€/mois)  
> * **Problème :** Manque de clients réguliers  
> * **Budget dispo :** > 2 000€  
> * **Action requise :** Prendre le relais manuellement pour closer.  
> * 🔗 [Lien vers la conversation Notion](https://notion.so/...)

**Action pour Sarah (Head of Sales) :** 
1. Cliquez sur le lien pour lire l'historique complet sur Notion.
2. Ouvrez la messagerie du réseau social concerné et envoyez un message personnalisé de closing (ou proposez un appel direct de vive voix). Le bot détecte ton intervention et n'interviendra plus.

### 3. Gestion du Tableau Kanban Notion (Suivi & Optimisation)
L'espace Notion **Setter AI Lead OS** est le centre de contrôle du système.
* **Bases de données :** Allez dans l'onglet `Bases de données` ➔ `Tableau des Leads`.
* **Les Colonnes du Kanban :**
  * `Triage` : Les leads en cours de conversation avec l'IA.
  * `Qualifiés (Calendly Booked)` : Les prospects qui ont réservé d'eux-mêmes un appel.
  * `À relancer (Lead Chaud)` : Les prospects qualifiés mais qui n'ont pas encore cliqué sur le lien de réservation.
  * `Non-Éligibles (Nurturing)` : Les prospects écartés par l'IA (stockés pour les futures campagnes e-mails).
* **Modifier les Prompts d'entraînement :** Si tu remarques que l'IA est trop agressive ou pas assez chaleureuse, va dans la table `Configuration` et modifie la cellule `Instructions Système`. Le bot appliquera les nouvelles consignes dès le message suivant.

---

## 🚨 En Cas d'Urgence / Bug Technique
* **Si le bot répond à côté de la plaque ou en boucle :**
  1. Allez sur la fiche du prospect dans Notion.
  2. Cochez la case `Désactiver le Bot` (cela bloque instantanément le script n8n pour ce prospect).
  3. Prenez le relais manuellement.
  4. Envoyez un message sur Slack dans `#tech-incident` en taguant @Roland-Caryl avec le lien du lead concerné.
