---
project: "Projet DPM – Vélib' Paris"
team: "Jordan S., Abdelmalek B."
formation: "RNCP 7 – Machine Learning & IA – DataScientest"
period: "Sept.–Nov. 2025"
role: "Data Product Manager (binôme)"
rewritten_for: "Soutenance orale – version étudiante plausible"
---

# 2.3 — Du MVP au Backlog

## Contexte & Objectifs

**Situation** : MVP validé (Epic A1 - Dashboard BI). **Comment transformer cette vision en plan d'exécution opérationnel ?**

**Objectif** : Créer un backlog produit structuré + méthode agile adaptée aux projets data.

**Pourquoi ?** Projets data ont des défis spécifiques :
- Incertitude sur les résultats (dimension exploratoire)
- Dépendances strictes données-modèle-infra
- Profils métier variés (Data Engineers, Data Scientists, Analysts, PMs)
- Itérations rapides nécessaires

---

## Méthodes agiles : Scrum, Kanban, Hybride

### Scrum : Structuré

**Principe** : Sprints fixes (2 semaines), cérémonies ritualisées (Planning, Daily, Review, Retro).

**Avantages** :
- Cadence prévisible, focus clair, détection rapide blocages

**Inconvénients** :
- Difficile d'estimer tâches exploratoires ("Explorer features ML" = 2 jours ou 2 semaines ?)
- Rigidité (difficile pivoter en milieu de Sprint)

**Cas d'usage** : Projets data avec infra stable, équipes > 5 personnes.

### Kanban : Flexible

**Principe** : Colonnes workflow (Backlog → To Do → In Progress → Review → Done), pas de sprints fixes.

**Avantages** :
- Flexibilité max, pas d'estimation stricte, visualisation WIP

**Inconvénients** :
- Manque prévisibilité, risque dérive scope

**Cas d'usage** : Projets en phase exploratoire, petites équipes (1-3 personnes).

### Scrumban : Hybride (recommandé)

**Principe** : Sprints fixes (prévisibilité) + Kanban board (flexibilité).

**Vélib' - Epic A2** :
- Sprints 3 semaines (plus long que 2 sem classiques, pour exp ML)
- Kanban board : Backlog → To Do → Data Prep → Modeling → Review → Done
- Daily Standup 10 min (asynchrone Slack)
- Sprint Review avec chefs exploitation
- Pas de Sprint Planning strict (ajout tâches selon découvertes)

**Résultat** : MVP livré en 12 semaines (4 Sprints), avec 3 pivots majeurs.

---

## Outils de gestion backlog

### Comparatif

| Outil | Avantages | Inconvénients | Cas d'usage |
|-------|-----------|---------------|-------------|
| **Notion** | Gratuit (≤10 users), flexible, doc intégrée | Pas d'intégration GitHub native | Petites équipes (1-5) |
| **Jira** | Intégrations natives, reporting avancé, scalable | Coût élevé (~900$/an/10 users), interface complexe | Moyennes/grandes équipes (>10) |
| **GitHub Projects** | Intégration parfaite GitHub, gratuit | Fonctionnalités limitées | Équipes dev centrées GitHub |
| **Trello** | Simple, gratuit, rapide prise en main | Pas adapté gros projets | Très petites équipes (1-3) |

**Vélib' - Epic A1** : **Notion** (binôme étudiant, gratuit, suffisant).

---

## Structure backlog produit

### Hiérarchie

```
Roadmap (12 mois)
  ↓
Epics (grandes fonctionnalités, 2-3 mois)
  ↓
User Stories (fonctionnalités utilisateur, 1-5 jours)
  ↓
Tasks (tâches techniques, quelques heures)
```

### Epic A1 : Dashboard BI

**Epic** : Créer dashboard BI criticité stations (6-8 semaines)

**User Stories** :
- US1 : En tant que chef exploitation, je veux voir Top 10 stations critiques pour prioriser actions (3 jours)
- US2 : En tant que manager, je veux recevoir alerte Slack si station > 40% SOR pour réagir rapidement (2 jours)
- US3 : En tant qu'analyste, je veux filtrer par arrondissement pour analyser par zone (2 jours)

**Tasks (exemple US1)** :
- T1 : Créer vue SQL calcul SOR/DOR par station (0.5 jour)
- T2 : Développer dashboard Metabase (2 jours)
- T3 : Tests et validation (0.5 jour)

---

## Types de tâches spécifiques projets data

| Type | Description | Estimation | Exemple |
|------|-------------|------------|---------|
| **Spike** | Exploration/recherche (résultat incertain) | Time-boxed (ex. 2 jours max) | "Explorer features pour améliorer modèle" |
| **Bug** | Correction anomalie | Variable | "Corriger calcul SOR (division par zéro si station offline)" |
| **Test** | Validation qualité | 10-20% effort dev | "Tests unitaires pipeline ETL" |
| **Tâche technique** | Infra/devops | Variable | "Configurer partitioning PostgreSQL" |

---

## Périmètre & Hors-périmètre

### Dans le périmètre
✅ Méthodes agiles (Scrum, Kanban, Scrumban)
✅ Comparatif outils (Notion, Jira, GitHub, Trello)
✅ Structure backlog (Epics → User Stories → Tasks)

### Hors périmètre
❌ Backlog complet Epic A1 (décomposition exhaustive)
❌ Implémentation outil (Notion board)
❌ Gestion sprints réels

---

## Limites & Risques

### Limites identifiées
1. **Pas de backlog détaillé** : On n'a pas décomposé Epic A1 en 20-30 User Stories (ferait partie phase Développement).
2. **Pas de chiffrage** : Les estimations (3 jours, 2 jours) sont approximatives.
3. **Pas d'outil implémenté** : On n'a pas créé le Notion board.

### Risques
- **Sous-estimation** : Les tâches peuvent prendre 2× plus de temps que prévu.
- **Scope creep** : Risque d'ajouter User Stories non prévues en cours de route.
- **Blocages** : Dépendances mal identifiées → blocage équipe.

