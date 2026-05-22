# 🏛️ Bento AI Systems - Process Architectures & Case-Pilotes Elite
*Frameworks d'automatisation, architectures d'agents et configurations n8n de production pour PME et Infopreneurs*

Ce dépôt GitHub centralise les architectures techniques, les configurations n8n, les structures Notion CRM et les guides opérationnels (SOP) conçus pour démontrer l'implémentation de systèmes d'IA de bout en bout pour les clients de **Entrepreneurs.com**.

Chaque cas d'usage est modélisé selon le framework **WAT** (Workflows, Agents, Tools), reliant directement l'ingénierie technique au ROI financier et au P&L du dirigeant.

---

## 🗺️ Le Framework WAT (Workflows, Agents, Tools)

Pour chaque implémentation, le système est segmenté en trois couches :
1. **Workflows (Les Flux)** : Cartographie et automatisation des flux opérationnels de base (ingestion, routage, relances).
2. **Agents (L'Intelligence)** : Modèles de raisonnement (prompts système, qualification, vision, transcription) utilisant des LLMs (Claude 3.5 Sonnet, GPT-4o).
3. **Tools (L'Écosystème)** : Outils tiers interconnectés (Shopify, Notion CRM, Stripe, Slack, Gotenberg PDF, API WhatsApp).

---

## 📁 Liste des Cas Pilotes Implémentés

Chaque dossier contient la suite complète des **6 documents clés** nécessaires à l'ingestion dans **NotebookLM** pour créer des environnements d'apprentissage ou de vente personnalisés, ainsi que le fichier de configuration de workflow n8n.

### 1. [🤖 Setter Virtuel - Instagram/LinkedIn DM](file:///c:/Users/caryl/OneDrive/Desktop/ENTREPRENEURS.COM/1_Setter_Virtuel)
*Cible : Coachs, Infopreneurs High-Ticket & Agences.*
* **Fichiers clés :** Contexte Stratégique & Alignement ROI, SOP, structure Notion Leads Tracker, diagramme d'architecture Meta Webhook, livrable technique avec boucle de rate-limiting, scripts vidéo et [workflow n8n](file:///c:/Users/caryl/OneDrive/Desktop/ENTREPRENEURS.COM/1_Setter_Virtuel/n8n_Pipeline_Setter_V1.json).

### 2. [📄 Générateur de Propositions PDF](file:///c:/Users/caryl/OneDrive/Desktop/ENTREPRENEURS.COM/2_Generateur_Propositions)
*Cible : Cabinets de conseil, Freelances & Agences B2B.*
* **Fichiers clés :** Contexte Stratégique & Alignement ROI, SOP d'avant-vente, structure Notion Sales Intelligence, architecture Perplexity + Claude, livrable technique avec Gotenberg PDF sous Docker, et [workflow n8n](file:///c:/Users/caryl/OneDrive/Desktop/ENTREPRENEURS.COM/2_Generateur_Propositions/n8n_Pipeline_Proposal_V1.json).

### 3. [📦 Gestionnaire de Litiges Logistiques](file:///c:/Users/caryl/OneDrive/Desktop/ENTREPRENEURS.COM/3_Litiges_Logistique)
*Cible : Marques E-Commerce, D2C.*
* **Fichiers clés :** Contexte Stratégique & Alignement ROI, SOP logistique pour support client, structure Notion D2C Operations, architecture d'ingestion Shopify et tracking nocturne, livrable technique et [workflow n8n](file:///c:/Users/caryl/OneDrive/Desktop/ENTREPRENEURS.COM/3_Litiges_Logistique/n8n_Pipeline_Disputes_V1.json).

### 4. [📞 Estimateur WhatsApp & Secrétariat BTP](file:///c:/Users/caryl/OneDrive/Desktop/ENTREPRENEURS.COM/4_Estimateur_WhatsApp_BTP)
*Cible : Artisans, PME du bâtiment & Services physiques.*
* **Fichiers clés :** Contexte Stratégique & Alignement ROI, SOP de capture de chantiers sur site, structure Notion BatiSuivi, architecture vocale (Whisper) & vision (GPT-4o Vision), livrable technique et [workflow n8n](file:///c:/Users/caryl/OneDrive/Desktop/ENTREPRENEURS.COM/4_Estimateur_WhatsApp_BTP/n8n_Pipeline_BTP_V1.json).

---

## ⚡ Guide de Démarrage & Import n8n

Pour déployer l'un des workflows n8n sur ton instance de test ou production :

1. **Installe l'infrastructure** via le Docker Compose fourni dans le fichier `Livrable_Semaine_1_Detaille.md` du cas d'usage choisi.
2. **Importe le fichier JSON** du workflow dans ton interface n8n :
   * Va dans *Workflows* ➔ *Add Workflow* ➔ *Import from File*.
   * Sélectionne le fichier `n8n_Pipeline_..._V1.json` correspondant.
3. **Configure tes credentials** :
   * Connecte ton API Key OpenAI/Anthropic.
   * Renseigne l'intégration Notion de test.
   * Ajoute les tokens de webhooks pour Meta (Instagram/WhatsApp) ou Shopify.

---

## 🛠️ Outils de Cadrage Globaux (Dépôt Racine)

En plus des cas pilotes, ce dépôt contient trois outils d'aide à la vente et au diagnostic à utiliser directement en séance de coaching :
* [formulaire_audit_ia.md](file:///c:/Users/caryl/OneDrive/Desktop/ENTREPRENEURS.COM/formulaire_audit_ia.md) : Questionnaire de diagnostic d'onboarding pour qualifier le business model, les goulots d'étranglement et le ROI en 20 minutes.
* [bibliotheque_prompts_notebooklm.md](file:///c:/Users/caryl/OneDrive/Desktop/ENTREPRENEURS.COM/bibliotheque_prompts_notebooklm.md) : Collection de prompts types (stratégiques, opérationnels et techniques) pour interroger et tester tes environnements NotebookLM.
* [matrice_opportunites_ia.md](file:///c:/Users/caryl/OneDrive/Desktop/ENTREPRENEURS.COM/matrice_opportunites_ia.md) : Une matrice sectorielle de **15 cas d'usage IA et d'automatisations** classés par complexité, stack technique et KPIs impactés.

---

## 🧠 Exploitation NotebookLM (Sales Enablement)

Cette suite documentaire est calibrée pour être importée dans Google **NotebookLM**. En déposant les 6 fichiers Markdown d'un cas pilote, tu obtiens :
1. **Un outil de FAQ commercial** : Tu peux demander en langage naturel à l'IA *"Comment Julie traite-t-elle un client qui demande un remboursement Stripe ?"* ou *"Quelles sont les formules SQL à configurer dans mon Notion pour le BTP ?"*.
2. **Un Podcast Vulgarisé automatique** : NotebookLM génèrera une conversation audio expliquant l'architecture, la valeur business et la résolution technique de la panne (ex. corruption de base SQLite ou blocages d'IP transporteurs) avec un naturel impressionnant.
