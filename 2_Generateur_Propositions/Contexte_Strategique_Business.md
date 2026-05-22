# Contexte Stratégique & Alignement ROI - Cas 2 : Générateur de Propositions PDF
*Fiche de cadrage business pour Agences B2B, Cabinets de Conseil et Freelances*

Ce document sert de pont entre l'architecture technique (Perplexity, Claude, Gotenberg, Notion) et les enjeux de conversion commerciale (Taux de closing, TJM, gain de temps associé) du client type d'Entrepreneurs.com.

---

## 👥 1. Le Persona Client & Business Model
* **Nom Fictif du Client :** **Pierre**, Directeur Associé chez *Bento Consulting*, cabinet de conseil et agence marketing B2B.
* **Panier Moyen :** Accompagnements et forfaits à **8 000 € HT** de moyenne.
* **Volume d'Activité :** Reçoit environ 8 demandes de propositions commerciales sur-mesure par semaine après appels de découverte.
* **Équipe Vente :** Sophie (Directrice de Cabinet) et Pierre (qui réalise les audits stratégiques).

---

## 🛑 2. Les Goulots d'Étranglement Opérationnels (Avant Automatisation)
1. **La Lenteur de Rédaction (Speed-to-Proposal) :** Pierre passe en moyenne **4 à 6 heures par proposition** à faire des recherches Google sur le prospect, rédiger le diagnostic SWOT, définir la méthodologie et concevoir le devis sur Word.
2. **Le Délai d'Envoi :** À cause de cette charge de travail, les propositions mettent **5 à 7 jours** à être envoyées après l'appel de découverte. Durant ce temps, les concurrents plus réactifs signent les contrats.
3. **Le Manque de Personnalisation :** Pour gagner du temps, Pierre a tendance à copier-coller des propositions types, ce qui réduit l'impact perçu de son expertise et fait baisser le taux de closing.

---

## 📈 3. Stratégie ROIste & Alignement KPIs

L'automatisation de l'avant-vente et de la génération de PDF vise à augmenter drastiquement la conversion commerciale :

### A. Augmentation du Taux de Closing (Conversion)
* Envoyer une proposition ultra-personnalisée, enrichie de données récentes du marché (via Perplexity), en **moins de 2 heures** après le call produit un effet "wow" immédiat chez le décideur.
* Le taux de closing moyen de l'agence passe de **25% à 40%** uniquement grâce à la vélocité et à la qualité de l'audit initial.

### B. Gain de Temps Facturable (TJM)
* Pierre économise **4 heures de rédaction par proposition** (soit 32 heures par semaine pour 8 propositions).
* À un taux journalier moyen (TJM) estimé à 1 200 € HT (soit 150 €/h), cela représente une économie de valeur brute de **4 800 € HT de temps qualifié récupéré par semaine**.

### C. Suivi des KPIs Financiers
* **Délai d'envoi de la proposition :** Cible à **< 2 heures** (contre 6 jours en manuel).
* **Ventes supplémentaires générées :** 1 à 2 signatures de contrats supplémentaires de 8 000 € HT par mois (= **+8 000 € à +16 000 € HT de CA additionnel / mois**).
* **ROI Mensuel du Système :** 
  * Temps de rédaction économisé pour Pierre : ~120h/mois (valeur ~18 000 €).
  * CA supplémentaire sécurisé.
  * Coût du système : ~45 €/mois.

---

## 🗺️ 4. Intégration dans l'Entonnoir de Vente

```
[Call de Découverte Effectué]
       │
       ▼
[Saisie des Notes de Call dans Notion] ➔ (Déclenchement instantané n8n)
       │
       ├─────────────────────────────────────────┐
       ▼ (Recherche Perplexity API)               ▼ (Traitement Claude 3.5)
[Scraping Positionnement & Concurrents]    [Rdaction du Diagnostic & Solutions]
       │                                         │
       └──────────────────┬──────────────────────┘
                          ▼
             [Gotenberg PDF Generator] ➔ (Génération de la proposition de 3 pages)
                          │
                          ▼
             [Slack Alert pour Sophie] ➔ (Aperçu du prix et validation)
                          │
                          ▼
             [Envoi PDF un clic au prospect]
```
