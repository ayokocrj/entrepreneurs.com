# Contexte Stratégique & Alignement ROI - Cas 3 : Gestionnaire de Litiges Logistiques
*Fiche de cadrage business pour Marques E-Commerce, D2C et Grossistes*

Ce document sert de pont entre l'architecture technique (Shopify API, Tracking APIs, OpenAI, Notion) et les enjeux de marge nette (P&L, LTV, réduction de l'attrition SAV) du client type d'Entrepreneurs.com.

---

## 👥 1. Le Persona Client & Business Model
* **Nom Fictif du Client :** **Thomas**, Fondateur de *AlpSport*, boutique e-commerce D2C (Direct-to-Consumer) expédiant du matériel outdoor.
* **Volume d'Activité :** **5 000 commandes expédiées par mois**.
* **Panier Moyen :** 75,00 € TTC.
* **Équipe Support :** Julie, responsable du service client et SAV.

---

## 🛑 2. Les Goulots d'Étranglement Opérationnels (Avant Automatisation)
1. **La Perte Sèche des Remboursements Transporteurs :** Environ **5%** des colis subissent des retards de livraison (250 colis/mois). Contractuellement (Chronopost, Mondial Relay, Colissimo), les frais de port (d'une valeur moyenne de 8,00 €) sont remboursables. Faire 250 demandes de réclamation manuelles sur les portails transporteurs prendrait **25 heures par mois**. Thomas abandonne donc ces frais, perdant **2 000 € HT de marge nette par mois**.
2. **La Surcharge du SAV & Taux d'Attrition Client (Churn) :** Les clients mécontents de leur retard appellent le SAV en colère. Julie passe ses journées à calmer des clients et à traiter des demandes de remboursement.
3. **Le Risque de Chargebacks (Litiges Stripe) :** Certains clients s'impatientent et ouvrent des litiges de paiement, augmentant les frais de pénalité Stripe et le taux d'attrition de la boutique.

---

## 📈 3. Stratégie ROIste & Alignement KPIs

Le déploiement du Gestionnaire de Litiges IA résout en parallèle le problème de trésorerie (marge brute) et la rétention client (LTV) :

### A. Récupération Directe de Trésorerie (Marge Net)
* L'IA détecte chaque retard de manière chirurgicale en interrogeant les APIs transporteurs chaque nuit.
* Les lettres de réclamation contenant toutes les références juridiques sont générées et prêtes à être envoyées automatiquement.
* Taux de récupération visé : **90%** (soit **1 800 € HT / mois de cash réinjecté directement dans la marge brute de l'entreprise**).

### B. Proactivité SAV & Augmentation de la LTV
* Au lieu d'attendre que le client se plaigne, le système lui envoie un e-mail d'excuse proactive contenant un code de réduction unique de 15% pour sa prochaine commande dès le retard constaté.
* Les tickets SAV liés aux retards baissent de **60%**.
* Le taux de réachat des clients ayant subi un retard passe de 10% (déçus) à **25%** (impressionnés par le geste proactif), augmentant la LTV client.

### C. Suivi des KPIs Financiers
* **Taux de tickets SAV retards :** Cible à **< 2%** (contre 8% auparavant).
* **Frais de port récupérés (Net) :** Cible à **1 800 € HT / mois** (contre 0 €).
* **ROI Mensuel du Système :** 
  * Gain direct P&L : +1 800 € de frais remboursés.
  * Gain de temps Julie : 25h/mois (valeur ~500 €).
  * Coût de l'automatisation : ~18 €/mois.

---

## 🗺️ 4. Intégration dans l'Entonnoir de Vente & SAV

```
[Commande Shopify Expédiée]
       │
       ▼
[Webhook n8n enregistre dans Notion OS]
       │
       ▼ (Chaque nuit à 2h00)
[n8n interroge les APIs transporteurs]
       │
       ├─────────────────────────────────────────┐
       ▼ (Retard détecté > CGV)                  ▼ (Livraison OK)
[Création Code Promo -15% Shopify]        [Fiche archivée sur Notion]
       │
       ├─────────────────────────────────────────┐
       ▼ (Proactif Client)                       ▼ (Récupération Transporteur)
[Envoi Email Excuse + Code Promo]        [Génération & Envoi Lettre de Litige]
       │                                         │
       ▼                                         ▼
[Réduction Tickets SAV / Hausse LTV]       [Remboursement versé au P&L]
```
