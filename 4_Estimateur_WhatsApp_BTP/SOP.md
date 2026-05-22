# SOP : Utilisation Opérationnelle de l'Estimateur WhatsApp BTP IA
*Guide destiné aux équipes opérationnelles (Jean-Pierre - Fondateur & Amélie - Secrétaire Administrative)*

Ce document est le guide d'utilisation opérationnel pour suivre la qualification automatique des chantiers BTP par WhatsApp et la génération de devis pré-calculés par l'IA.

---

## 🎯 Objectif du Système
Ce système permet à **RénovAlpin** de capturer les demandes de devis 24h/24 par WhatsApp et par messagerie vocale. L'IA analyse les photos de chantiers envoyées par les clients (murs à peindre, sol à carreler, croquis cotés) ainsi que les descriptions textuelles ou vocales. Elle calcule un pré-chiffrage basé sur la grille tarifaire de l'entreprise, crée la fiche client dans Notion, et prépare un devis PDF que la secrétaire (Amélie) peut valider et envoyer en un clic sur WhatsApp.

---

## 📋 Processus Étape par Étape

### 1. Réception des Demandes Clients sur WhatsApp
Les clients envoient leur demande de projet sur le numéro WhatsApp Business de l'entreprise.
1. Le client envoie une description de ses travaux et des photos du chantier ou de ses plans.
2. Si le client envoie un message vocal ou appelle la boîte vocale VoIP, le système convertit le message vocal en texte (via Whisper) et lui envoie un message WhatsApp automatique pour lui demander des photos complémentaires.
3. L'IA analyse les photos pour détecter les dimensions estimées, la complexité du support (ex. fissures, ancienne tapisserie à déposer) et calcule une estimation brute.

### 2. Validation & Ajustement du Chiffrage sur Notion
Toutes les demandes de chantiers qualifiées arrivent dans le Notion **BatiSuivi OS**.
1. Ouvrez l'espace Notion et allez dans l'onglet `Devis en Attente`.
2. Cliquez sur la nouvelle estimation générée. Vous y trouverez :
   * L'historique de la conversation WhatsApp.
   * Les photos du chantier analysées par l'IA.
   * La grille de prix appliquée (ex. préparation de surface, peinture bicouche, dépose).
   * La marge prévisionnelle et le montant total estimé HT et TTC.
3. **Ajustement :** Si Jean-Pierre souhaite modifier le prix au m² ou ajouter un forfait spécifique (ex. échafaudage difficile), il modifie directement les lignes de prix sur Notion. Le devis se met à jour en temps réel.

### 3. Envoi du Devis Officiel au Client
Une fois le chiffrage validé :
1. Passez le statut de la fiche Notion de `Brouillon` à `Approuvé pour Envoi`.
2. Le système génère automatiquement un devis PDF professionnel reprenant la charte graphique de **RénovAlpin**.
3. Le PDF est envoyé automatiquement au client sur WhatsApp sous forme de document, accompagné d'un message poli lui proposant de signer le devis en ligne.
4. Le statut du devis passe automatiquement à `Envoyé - En Attente de Signature`.

---

## 🚨 Résolution de Problèmes

* **L'IA n'a pas réussi à calculer les dimensions sur la photo :** Si la photo est trop floue ou manque de repères, le statut Notion passe à `Infos Manquantes`. Amélie ou Jean-Pierre recevra une alerte. Il suffira de saisir manuellement les dimensions (ex: `Longueur` et `Hauteur`) dans les propriétés de la fiche Notion pour que le calcul automatique reprenne.
* **Le client souhaite modifier la demande après envoi :** Sur la fiche Notion, cliquez sur le bouton `Créer Version 2`. Cela duplique l'estimation pour garder une trace de l'historique. Modifiez les quantités ou prestations, puis passez à nouveau le statut à `Approuvé pour Envoi`.
* **Le numéro WhatsApp du client est invalide ou utilise un fixe :** Le système détecte l'échec de distribution WhatsApp. Il envoie immédiatement une alerte par e-mail à Amélie et tente de transmettre le devis par e-mail si l'adresse a été fournie, en changeant le canal de livraison sur Notion pour `Email`.
