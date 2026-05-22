# SOP : Utilisation Opérationnelle du Gestionnaire de Litiges Logistiques IA
*Guide destiné aux équipes non techniques (Thomas - Fondateur & Julie - Responsable Customer Support)*

Ce document est le guide d'utilisation opérationnel pour suivre la détection des retards de livraison et la récupération automatique des indemnités transporteurs.

---

## 🎯 Objectif du Système
Ce système surveille automatiquement l'état des livraisons de colis Shopify auprès des transporteurs (Mondial Relay, Colissimo, Chronopost). En cas de retard de livraison dépassant les CGV du transporteur, il rédige et envoie une réclamation officielle pour récupérer les frais d'expédition (indemnisation), tout en générant un code promo d'excuse personnalisé envoyé par e-mail au client final.

---

## 📋 Processus Étape par Étape

### 1. Suivi Quotidien des Colis sur Notion
L'espace Notion **D2C Operations OS** est mis à jour toutes les nuits.
1. Allez dans l'onglet `Bases de données` ➔ `Suivi des Livraisons`.
2. Le tableau affiche trois indicateurs majeurs :
   * `En cours de livraison` : Colis en transit.
   * `Livré hors délais` : Colis reçus par les clients après la date limite contractuelle.
   * `Litige Ouvert` : Les réclamations envoyées aux transporteurs en cours de traitement.

### 2. Validation d'un Geste Commercial pour le Client
Dès qu'un retard est détecté, le système pré-génère un e-mail d'excuse contenant un code de réduction unique de 15% (créé dynamiquement sur Shopify).
* **Si la propriété `Mode Envoi Client` est sur "Semi-Auto" :**
  1. Julie reçoit une alerte dans le canal Slack `#sav-shipping-delays`.
  2. Allez sur la fiche du client dans Notion, relisez le brouillon de l'e-mail d'excuse généré par l'IA.
  3. Si le texte vous convient, cliquez sur le bouton `Envoyer l'E-mail d'Excuse`. Le mail part instantanément et le code promo est activé sur Shopify.
* **Si la propriété `Mode Envoi Client` est sur "Auto" :**
  L'e-mail part immédiatement sans validation humaine préalable (paramètre recommandé pour les retards simples de 1 à 2 jours).

### 3. Traitement des Réclamations Transporteurs
Le système gère la rédaction des courriers de litiges avec les transporteurs pour réclamer le remboursement des frais de port.
1. Dans la base Notion, ouvrez la vue `Réclamations à Envoyer`.
2. Le système a déjà rédigé une lettre de réclamation officielle contenant les références juridiques de la commande et le tracking transporteur.
3. Le statut passe automatiquement à `Envoyé par Email` ou `Soumis via API` selon le transporteur.
4. Lorsque vous recevez le remboursement ou l'avoir du transporteur, modifiez le statut à `Remboursé` et indiquez le montant récupéré dans la case `Montant Récupéré`.

---

## 🚨 Résolution de Problèmes
* **Un client refuse le bon d'achat et exige un remboursement direct :** Sur la fiche client dans Notion, cochez la case `Remboursement Partiel Stripe`. Le système annulera automatiquement les frais de port de la commande sur Stripe et Shopify en tâche de fond.
* **Un transporteur rejette la réclamation :** Passez le statut du litige à `Rejeté` et indiquez le motif dans la zone de texte libre. L'IA apprendra de ce motif pour adapter la formulation juridique des prochaines réclamations.
