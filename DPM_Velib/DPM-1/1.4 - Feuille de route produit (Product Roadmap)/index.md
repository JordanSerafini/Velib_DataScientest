---
project: "Projet DPM – Vélib' Paris"
team: "Jordan S., Abdelmalek B."
formation: "RNCP 7 – Machine Learning & IA – DataScientest"
period: "Sept.–Nov. 2025"
role: "Data Product Manager (binôme)"
---

# 1.4 — Product Roadmap (Feuille de route produit)

## Contexte & Objectifs

**Situation** : On a tout ce qu'il faut :
- ✅ Problèmes identifiés (Discovery 1.2)
- ✅ Améliorations priorisées (1.3.1) → **A1** en tête
- ✅ KPIs définis (1.3.2)
- ✅ Product Vision Board (1.3.3)

**Manque** : **Quand** livrer tout ça ? Dans quel ordre ? Sur quel horizon ?

**Objectif** : Créer une Product Roadmap sur 12 mois pour positionner les améliorations dans le temps.

**Pourquoi ?**
- ✅ Vision claire de ce qui sera livré et quand
- ✅ Alignement des équipes
- ✅ Outil de communication
- ✅ Organisation du travail technique

---

## Product Roadmap Vélib' (12 mois)

### Vue d'ensemble

```
┌────────────────────────────────────────────────────────────────┐
│              PRODUCT ROADMAP - VÉLIB' (12 mois)                │
└────────────────────────────────────────────────────────────────┘

Timeline  │ T0 (MVP)    │ T+1 (0-3m)  │ T+2 (3-6m)  │ T+3-T+4 (6-12m)
──────────┼─────────────┼─────────────┼─────────────┼────────────────
          │             │             │             │
Goal 1    │ Infra GBFS  │ Dashboard   │             │ Reco app
Satisfaction│ KPIs      │ BI Criticité│             │ mobile
usager    │ 🟩 Released │ 🟨 En cours │             │ 🟦 Planifié
          │             │             │             │
──────────┼─────────────┼─────────────┼─────────────┼────────────────
          │             │             │             │
Goal 2    │             │             │ Prévision   │ Optim
Efficience│             │             │ demande     │ tournées
opéra.    │             │             │ 🟦 Planifié │ 🟦 Planifié
          │             │             │             │
──────────┼─────────────┼─────────────┼─────────────┼────────────────
          │             │             │             │
Goal 3    │             │             │ Messages    │
Gestion   │             │             │ prescriptifs│
anomalies │             │             │ 🟦 Planifié │
          │             │             │             │
└─────────┴─────────────┴─────────────┴─────────────┴────────────────┘

Légende : 🟩 Released │ 🟨 En cours │ 🟦 Planifié
```

---

## Détail des Features (Epics)

### T0 (MVP) : Fondations data 🟩 Released

**Feature 0 : Infrastructure data GBFS**
- Ingestion automatique flux GBFS toutes les 60s
- Stockage PostgreSQL (table `fct_station_snapshot`)
- Pipeline d'historisation (conservation 6 mois rolling)

**Feature 1 : KPIs baseline**
- Calcul SOR/DOR sur Top 100 stations
- Dashboard référence (baseline avant améliorations)
- Export CSV pour analyses

**Pourquoi commencer par ça ?** Parce qu'on ne peut rien mesurer sans infra data. C'est le prérequis.

**Durée** : 3-4 semaines (fait avant T+1)

---

### T+1 (0-3 mois) : Epic A1 - Dashboard BI 🟨 En cours

**Feature 2 : Dashboard BI opérationnel**

**Goal** : Satisfaction usager (Goal 1) + Efficience (Goal 2)

**Description** :
- Vue "Top stations critiques" (SOR/DOR temps réel)
- Alerting automatique Slack/Email (seuil SOR > 40%)
- Cartes de chaleur par créneau horaire
- Historique tendances 7/14/30 jours

**Techno** : Metabase ou Tableau (à valider)

**Utilisateurs** : Équipes régulation Smovengo + Management

**Critères succès** :
- **-20% SOR/DOR** sur Top 100 stations
- Dashboard consulté **5 fois/semaine** par équipes ops

**Durée** : 6-8 semaines

**Dépendances** : Feature 0 et 1 (infra + baseline)

---

### T+2 (3-6 mois) : Epic A3 - Gestion anomalies 🟦 Planifié

**Feature 3 : Messages prescriptifs app mobile**

**Goal** : Gestion anomalies (Goal 3)

**Description** :
- Diagnostic guidé dans l'app ("Vélo bloqué ? Suivez ces 3 étapes...")
- FAQ contextuelle selon type de problème
- Suggestions stations alternatives si pleine/vide

**Techno** : Modification app mobile Vélib' (iOS/Android)

**Utilisateurs** : Usagers finaux (Alice, Marco)

**Critères succès** :
- **-15% tickets support** liés aux blocages
- **-10% MTTR** (Mean Time To Repair)

**Durée** : 4-5 semaines

**Dépendances** : Coordination équipe dev app mobile

---

### T+3 (6-9 mois) : Epic A2 Phase 1 - Prévision demande 🟦 Planifié

**Feature 4 : Modèle prédictif demande**

**Goal** : Efficience opérationnelle (Goal 2)

**Description** :
- Modèle prévision demande par station (baseline : moyenne mobile)
- Scoring criticité prévue (probabilité stock-out/dock-out à H+2/H+4)
- Recommandations stations à rééquilibrer (règles heuristiques)

**Techno** : Python (scikit-learn ou statsmodels) + intégration dashboard BI

**Utilisateurs** : Équipes régulation (Julien)

**Critères succès** :
- **+10 points RE** (Rebalancing Efficiency)
- Prévisions utilisées **3 fois/semaine** par équipes ops

**Durée** : 6-8 semaines

**Dépendances** : Feature 2 (dashboard BI)

---

### T+4 (9-12 mois) : Epic A2 Phase 2 - Optimisation tournées 🟦 Planifié

**Feature 5 : Algorithme optimisation tournées**

**Goal** : Efficience opérationnelle (Goal 2)

**Description** :
- Algorithme heuristique optimisation tournées (phase 1)
- (Optionnel Phase 2 : solveur VRP - Vehicle Routing Problem)
- Calcul circuits optimaux multi-arrêts

**Techno** : Python (OR-Tools ou PuLP) + intégration dashboard BI

**Critères succès** :
- **+15 points RE** (cumulative avec Feature 4)
- **-10% km parcourus** par camions

**Durée** : 8-10 semaines

**Dépendances** : Feature 4 (modèle prédictif)

---

### T+4 (9-12 mois) : Epic A4 - Recommandations app mobile 🟦 Planifié

**Feature 6 : API prédictive app mobile**

**Goal** : Satisfaction usager (Goal 1)

**Description** :
- API exposant probabilités dispo H+15/H+30
- Reco stations alternatives (rayon 400m)
- Intégration parcours utilisateur app Vélib'

**Techno** : API REST (FastAPI ou Flask) + modèle ML

**Critères succès** :
- **-10% JAR** (Journey Abandon Rate)
- **-15% temps détour**

**Durée** : 6-8 semaines

**Dépendances** : Feature 4 (modèle prédictif) + coordination équipe app mobile

---

## Adaptation au contexte : projet d'école

**Important** : Pour le projet DataScientest, on se concentre sur **T0 + T+1** (MVP + Epic A1).

**Ce qui est attendu pour la soutenance** :
- ✅ Roadmap complète (12 mois) → vision stratégique
- ✅ Développement MVP (T0) → infra data + baseline KPIs
- ✅ Développement Epic A1 (T+1) → Dashboard BI criticité stations
- ✅ Mesure résultats → validation KPIs (-20% SOR/DOR sur 6 semaines)

**Les Epics T+2, T+3, T+4** restent "Planifiés" mais ne sont pas développés dans le projet.

**Pourquoi quand même les inclure ?** Pour démontrer :
1. Capacité à **prioriser** (pourquoi A1 avant A2/A3/A4)
2. **Vision** à moyen/long terme
3. **Alignement** avec Goals stratégiques

---

## Périmètre & Hors-périmètre

### Dans le périmètre
✅ Roadmap complète (12 mois)
✅ Développement T0 + T+1 (pour projet étudiant)
✅ Positionnement Features selon priorisation (1.3.1)

### Hors périmètre
❌ Développement T+2, T+3, T+4 (planifiés mais non réalisés)
❌ Transformation roadmap → backlog → sprints (pas fait pour projet)
❌ Roadmap publique (pour usagers finaux)

---

## Limites & Risques

### Limites identifiées
1. **Durées estimées** : 6-8 semaines pour Feature 2 (dashboard BI) est optimiste. Peut être plus long si problèmes de qualité données ou bugs API GBFS.
2. **Dépendances externes** : Features 3 et 6 nécessitent coordination avec équipe app mobile Vélib' (pas de contact établi → hypothétique).
3. **Pas de backlog détaillé** : On n'a pas décomposé les Epics en User Stories (tickets) → ferait partie de la phase Développement.
4. **Pas de contingence** : On n'a pas prévu de marge pour imprévus (bugs, retards, changements priorités).

### Risques
- **Suroptimisme durées** : Les estimations peuvent être trop optimistes. En réalité, Feature 2 peut prendre 10-12 semaines au lieu de 6-8.
- **Changement priorités** : Si Smovengo change de priorité (ex. problème urgent sur A3), la roadmap devra être ajustée.
- **Blocage technique** : Si API GBFS est instable ou a des bugs, tout le projet est bloqué.

---

## Impact & Next Steps

### Impact attendu
Si on suit cette roadmap :
- **Court terme (T0-T+1)** : Infra data + dashboard BI criticité → base pour amélioration continue
- **Moyen terme (T+2-T+3)** : Gestion anomalies + prévision demande → efficacité opérationnelle
- **Long terme (T+4)** : Optimisation tournées + reco app → ROI maximal (245-475k€/an)

### Next Steps
1. **DPM-2** : Conception détaillée (ML methodology, BI methodology)
2. **Développement** : Implémenter T0 + T+1
3. **Mesure impact** : Validation KPIs

---

## Hypothèses à valider

**H1** : T0 + T+1 réalisable en 12 semaines (binôme étudiant)
**Validation** : Planning détaillé avec décomposition en tâches

**H2** : Dashboard BI sera adopté par équipes ops
**Validation** : Interviews avec Julien et équipes régulation (non fait)

**H3** : Dépendances techniques (infra GBFS) ne bloquent pas
**Validation** : Tests qualité API GBFS (disponibilité, fiabilité)

---

## Sources & Références

**Méthodologie** :
- Cours DataScientest DPM : Product Roadmap, backlog, sprints
- Article : "Building a product roadmap" (Product School)

**Outils** :
- Monday, ProductBoard, Notion (roadmaps internes)
- Jira, GitHub Projects (roadmaps techniques)

