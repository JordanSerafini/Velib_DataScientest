---
project: "Projet DPM – Vélib' Paris"
team: "Jordan S., Abdelmalek B."
formation: "RNCP 7 – Machine Learning & IA – DataScientest"
period: "Sept.–Nov. 2025"
role: "Data Product Manager (binôme)"
---

# 1.3.3 — Product Vision Board

## Contexte & Objectifs

**Situation** : On a identifié les problèmes (Discovery), priorisé les améliorations (1.3.1), défini les KPIs (1.3.2). Maintenant, **synthétiser la vision sur une page**.

**Objectif** : Créer un Product Vision Board pour l'amélioration **A1 - Réduire la criticité des stations**.

**Pourquoi ?** Pour :
- ✅ Aligner toutes les parties prenantes
- ✅ Éviter de partir dans tous les sens
- ✅ Communiquer clairement l'intérêt du projet
- ✅ Valider que la solution répond à un besoin réel

**Format** : Un canvas d'une page avec 5 sections (inspiré du cours DataScientest).

---

## Les 5 composantes du Product Vision Board

### 1. Vision globale (Elevator Pitch)
En 30 secondes, quel est l'argument clé ?

### 2. Groupe cible (Target Group)
Qui sont les utilisateurs de la solution ?

### 3. Besoins (Needs)
À quels problèmes répond la solution ?

### 4. Produit (Product)
Quelle est la solution concrète ?

### 5. Valeur business (Value)
Quelle valeur cette solution crée-t-elle ?

---

## Product Vision Board pour A1 (Vélib')

### 🎯 Vision globale

**Notre vision** :

*"Garantir aux usagers Vélib' une disponibilité fiable aux heures de pointe grâce à des alertes prédictives et un rééquilibrage optimisé par les données."*

**Pourquoi cette formulation ?**
- **"Garantir"** → objectif ambitieux mais clair
- **"Disponibilité fiable"** → répond au pain point #1
- **"Heures de pointe"** → périmètre précis
- **"Par les données"** → approche data-driven

---

### 👥 Groupe cible

**Groupe 1 : Usagers finaux** (app mobile)

**Persona** : **Alice** (commuter quotidienne)
- Utilise Vélib' tous les jours (8h-10h, 18h-20h)
- Zones : axes domicile-travail (résidentiel → bureaux)
- **Pourcentage** : ~60-70% de l'usage semaine

**Groupe 2 : Équipes opérationnelles** (régulation Smovengo)

**Persona** : **Julien** (logisticien de régulation)
- Planifie tournées de rééquilibrage
- Travail de nuit (contraintes trafic)
- Besoins : priorisation, optimisation, prédiction
- **Pourcentage** : ~30-50 personnes (équipes ops)

**Pourquoi différencier ?** Parce qu'ils n'interagissent pas pareil avec la solution :
- **Alice** → interface mobile, messages simples
- **Julien** → dashboard ops, données détaillées

---

### 💡 Besoins

**Besoins usagers finaux (Alice)** :

**Besoin #1 : Fiabilité**
→ *"Pouvoir compter sur un vélo à ma station habituelle."*

**Verbatim** : *"Trois fois cette semaine, aucun vélo. J'ai fini en retard."* — Alice, 32 ans

**Mesure** : Top 100 stations passent **15-25% du temps** en stock-out aux heures de pointe.

**Besoin #2 : Prédictivité**
→ *"Savoir s'il y aura un vélo dans 15-20 min."*

**Pourquoi ?** Info temps réel ne suffit pas. Alice consulte à 8h00, prend à 8h15 → situation peut changer.

**Besoin #3 : Alternatives prescriptives**
→ *"Si ma station est vide, qu'on me dise où aller."*

**Exemple** : *"République risque d'être vide. Essayez Oberkampf à 300m, 85% de chances d'avoir un vélo."*

**Besoins équipes ops (Julien)** :

**Besoin #4 : Priorisation**
→ *"Savoir quelle station traiter en priorité."*

**Verbatim** : *"On fait au feeling, ce n'est pas optimal."* — Julien

**Besoin #5 : Optimisation tournées**
→ *"Réduire les km parcourus et les coûts."*

**Mesure** : Rebalancing Efficiency (RE) actuellement variable, pas de mesure systématique.

---

### 🛠️ Produit

**Produit 1 (MVP) : Dashboard BI pour équipes ops**

**Fonctionnalités** :
1. **Vue "Top stations critiques"** : classement par SOR/DOR temps réel
2. **Alerting automatique** : Slack/Email si seuil dépassé (SOR > 40%)
3. **Cartes de chaleur** par créneau (8h-10h / 18h-20h)
4. **Historique** : tendances 7/14/30 jours

**Techno envisagée** : Metabase ou Tableau (à valider en Conception)

**Utilisateurs** : Équipes régulation et management Smovengo

**Produit 2 (Phase 2 - optionnelle) : Intégration app mobile**

**Fonctionnalités** :
1. **Prédiction dispo** : *"85% de chances d'avoir un vélo dans 20 min"*
2. **Reco alternatives** : stations proches avec meilleure dispo
3. **Gamification** : bonus pour inciter à lisser les flux

**Techno envisagée** : API prédictive (ML ou heuristique) + intégration app

**Utilisateurs** : Usagers finaux (Alice)

**Pourquoi deux produits ?** Deux groupes cibles avec besoins différents. **Produit 1 = MVP** (priorité). Produit 2 = évolution ultérieure.

---

### 💰 Valeur business

**Court terme (0-6 mois)**

**Valeur #1 : Réduction criticité**
- **Objectif** : **-20% temps en état critique** (Top 100 stations)
- **Impact** : Amélioration expérience usager, réduction tickets support
- **Mesure** : KPI 1 (SOR) et KPI 2 (DOR)

**Valeur #2 : Visibilité opérationnelle**
- **Objectif** : Dashboard BI en production (mise à jour quotidienne)
- **Impact** : Équipes ops ont vue consolidée de la criticité, décision data-driven

**Moyen terme (6-12 mois)**

**Valeur #3 : Efficacité rééquilibrage**
- **Objectif** : **+10-15 points RE** (Rebalancing Efficiency)
- **Impact** : Moins de passages inefficaces, réduction coûts
- **Estimation** : -10% km parcourus → **15-20k€/an économies** (à valider)
- **Mesure** : KPI 4 (RE)

**Valeur #4 : Réduction abandons**
- **Objectif** : **-10-15% JAR** (Journey Abandon Rate)
- **Impact** : Plus de trajets = plus de revenus, amélioration NPS
- **Estimation** : -10% abandons, 2€/trajet, 50k abandons/mois → **10k€/mois** = **120k€/an**
- **Mesure** : KPI 3 (JAR) — si télémétrie dispo

**Long terme (12-24 mois)**

**Valeur #5 : Rétention abonnés**
- **Objectif** : Réduire churn lié aux problèmes de disponibilité
- **Impact** : 1% churn évité (sur 470k abonnés) = 4 700 abonnés × 50€ = **235k€/an**
- **Mesure** : Corrélation SOR/DOR et taux désabonnement

**Valeur #6 : Image service public**
- **Objectif** : Amélioration NPS et avis en ligne
- **Impact** : Meilleure perception, attractivité nouveaux usagers, respect engagements IDFM

---

## Synthèse financière (estimations)

**Valeur potentielle sur 12 mois** :

| Levier | Estimation basse | Estimation haute |
|--------|------------------|------------------|
| Économies logistiques | 15k€ | 25k€ |
| Revenus supplémentaires (JAR ↓) | 80k€ | 150k€ |
| Rétention abonnés (churn ↓) | 150k€ | 300k€ |
| **TOTAL** | **~245k€** | **~475k€** |

**ROI** : Si coût développement dashboard BI + infra data = 50-80k€ → **ROI positif dès année 1**.

**⚠️ Important** : Ces chiffres sont des **estimations** basées sur des hypothèses. À valider avec données réelles Smovengo et affiner en Conception.

---

## Canvas synthétique (format 1 page)

```
┌──────────────────────────────────────────────────────────────┐
│           PRODUCT VISION BOARD - A1 Vélib'                   │
└──────────────────────────────────────────────────────────────┘

🎯 VISION
Garantir aux usagers une disponibilité fiable aux heures de
pointe grâce à des alertes prédictives et un rééquilibrage
optimisé par les données.

┌─────────────────────────┬────────────────────────────────────┐
│ 👥 GROUPE CIBLE         │ 💡 BESOINS                         │
├─────────────────────────┼────────────────────────────────────┤
│ 1️⃣ Usagers finaux      │ Usagers :                          │
│   • Alice (commuter)    │ • Fiabilité disponibilité          │
│   • 60-70% usage        │ • Prédictivité H+15/H+30           │
│                         │ • Alternatives prescriptives        │
│ 2️⃣ Équipes ops         │ Équipes ops :                      │
│   • Julien (régulation) │ • Priorisation stations            │
│   • 30-50 personnes     │ • Optimisation tournées            │
└─────────────────────────┴────────────────────────────────────┘

🛠️ PRODUIT
• Produit 1 (MVP) : Dashboard BI pour équipes ops
  - Top stations critiques (SOR/DOR)
  - Alerting automatique
  - Cartes chaleur par créneau
• Produit 2 (Phase 2) : Intégration app mobile
  - Prédictions dispo H+15/H+30
  - Recommandations alternatives

💰 VALEUR BUSINESS
• Court terme : -20% temps critique, Dashboard BI prod
• Moyen terme : +10-15 pts RE, +80-150k€/an revenus
• Long terme : +150-300k€/an rétention abonnés
• ROI estimé : 245-475k€/an pour 50-80k€ investissement

📊 KPIs DE SUCCÈS
• KPI 1 : SOR → -20%
• KPI 2 : DOR → -15%
• KPI 3 : JAR → -10-15%
• KPI 4 : RE → +10-15 points
```

---

## Périmètre & Hors-périmètre

### Dans le périmètre
✅ Product Vision Board pour A1
✅ Définition des 2 produits (dashboard BI + app mobile)
✅ Estimation ROI (avec hypothèses)

### Hors périmètre
❌ Validation avec équipes Smovengo (pas de contact)
❌ Chiffrage précis du développement (nécessite devis techniques)
❌ Business case détaillé avec analyse de sensibilité

---

## Limites & Risques

### Limites identifiées
1. **Estimations ROI** : Basées sur hypothèses (50k abandons/mois, 2€/trajet, etc.). À valider avec données réelles.
2. **Pas de validation users** : Vision Board créé par nous (binôme), pas co-construit avec Smovengo.
3. **Techno non choisie** : "Metabase ou Tableau" → à décider en phase Conception (benchmark outils).
4. **Produit 2 hypothétique** : Intégration app mobile nécessite accord équipe produit Vélib' (pas consulté).

### Risques
- **Suroptimisme** : ROI de 245-475k€ peut être optimiste. Si les hypothèses sont fausses, le ROI peut être plus faible.
- **Adoption** : Même si on construit le dashboard, il faut que Julien et les équipes l'utilisent. Risque de non-adoption si l'outil n'est pas adapté à leur workflow.
- **Coûts cachés** : On a estimé 50-80k€ de dev, mais il peut y avoir des coûts cachés (hébergement, maintenance, formation utilisateurs).

---

## Impact & Next Steps

### Impact attendu
Si on réussit à implémenter cette vision :
- **Usagers** : Moins de frustration, moins de détours
- **Opérateurs** : Coûts logistiques réduits, efficacité améliorée
- **Service public** : Image améliorée, conformité SLA

### Next Steps
1. **Section 1.4** : Feuille de route produit (roadmap 12 mois)
2. **DPM-2** : Conception détaillée (ML methodology, BI methodology)

---

## Hypothèses à valider

**H1** : Dashboard BI sera adopté par les équipes ops
**Validation** : Interviews avec Julien et équipes régulation (non fait pour projet étudiant)

**H2** : ROI de 245-475k€ est atteignable
**Validation** : Business case détaillé avec données réelles Smovengo

**H3** : Produit 1 (dashboard) suffit pour atteindre -20% SOR
**Validation** : Benchmark internationaux (Citi Bike, Santander Cycles)

---

## Sources & Références

**Méthodologie** :
- Cours DataScientest DPM : Product Vision Board
- Article : "Creating a product vision" (Roman Pichler)

**Estimations ROI** :
- Hypothèses : 50k abandons/mois, 2€/trajet, 470k abonnés, 1% churn
- Benchmarks : Citi Bike (économies logistiques -20-25%)

