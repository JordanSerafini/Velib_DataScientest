---
project: "Projet DPM – Vélib' Paris"
team: "Jordan S., Abdelmalek B."
formation: "RNCP 7 – Machine Learning & IA – DataScientest"
period: "Sept.–Nov. 2025"
role: "Data Product Manager (binôme)"
---

# 2.1.2 — Méthodologie conception en BI

## Contexte & Objectifs

**Situation** : On veut créer un dashboard BI pour Epic A1 (criticité stations). **Comment éviter les dashboards que personne n'utilise ?**

**Objectif** : Fournir une méthodologie pour créer des outils BI efficaces.

**Statistiques** : Gartner (2019) : seulement 20% des initiatives BI atteignent des résultats significatifs.

**Pourquoi ?** Déconnexion besoins métier, surcharge informationnelle, interface complexe, données obsolètes.

---

## Les 5 principes d'un outil BI efficace

### Principe #1 : Partir des besoins métiers

**Anti-pattern** : Partir des données disponibles et créer des graphiques "parce que c'est possible".

**Approche recommandée** :
1. Identifier les décisions à prendre
2. Lister les questions métier
3. Déterminer quelles données répondent
4. Choisir visualisations adaptées

**Exemple Vélib' - Epic A1** :

**Besoin** : Chefs d'exploitation doivent identifier stations critiques pour prioriser actions.

**Questions** :
- Quelles sont les 10 stations avec SOR le plus élevé ?
- À quels moments ces stations sont-elles critiques ?
- Évolution SOR sur 30 derniers jours ?

**Visualisations** : KPI cards, Top 10 tableau, carte géo, heatmap heure × jour.

### Principe #2 : Adapter à l'utilisateur

| Profil | Fréquence | Type outil |
|--------|-----------|-----------|
| Exécutif (C-level) | Hebdo | Dashboard synthétique (1 page, KPIs clés) |
| Manager ops | Quotidien | Dashboard + reportings détaillés |
| Analyste métier | Plusieurs fois/jour | Reporting interactif (drill-down) |
| Opérateur terrain | Occasionnel | Dashboard mobile, simplifié |

**Vélib' - Epic A1** :
- Chefs exploitation (Manager ops) → Dashboard interactif avec drill-down, filtres, export Excel
- Direction (Exécutif) → Vue synthétique (1 page, 3-4 KPIs)

### Principe #3 : Less is more

**Règles** :
- 5-7 visualisations max par page
- Hiérarchie visuelle claire (haut gauche = priorité)
- Éliminer le "chart junk" (effets 3D, ombres, backgrounds)
- Palette cohérente : rouge (critique), orange (attention), vert (OK)

### Principe #4 : Centré sur l'action

**Framework "Insight to Action"** : Pour chaque visualisation, répondre à :
- Quel **insight** ?
- Quelle **action** ?
- **Qui** responsable ?

**Exemple** :
| Visualisation | Insight | Action | Responsable |
|---------------|---------|--------|-------------|
| Top 10 stations SOR | "Bastille SOR = 12%" | Ajouter 5 bornes | Chef exploitation |
| Heatmap heure × jour | "8e arrondissement critique lundi 8h-10h" | Renforcer régulation lundi matin | Responsable régulation |

### Principe #5 : Storytelling

**Structure narrative** :
1. QUOI ? (Vue d'ensemble, KPIs)
2. OÙ ? (Top N, carte géo)
3. QUAND ? (Évolutions, patterns temporels)
4. POURQUOI ? (Corrélations, causes)
5. ET MAINTENANT ? (Actions recommandées)

---

## Dashboard vs Reporting

| Critère | Dashboard | Reporting |
|---------|-----------|-----------|
| **But** | Monitoring, détection anomalies | Exploration, analyse détaillée |
| **Longueur** | 1 page | Multi-pages |
| **Fréquence** | Temps réel / Quotidien | Hebdo / Mensuel |
| **Temps lecture** | 1-3 min (scan rapide) | 10-30 min (analyse) |
| **Interactivité** | Faible | Élevée (filtres, drill-down) |

**Vélib' - Epic A1** : On crée un **Dashboard** (1 page, monitoring quotidien).

---

## Structure Dashboard Vélib' (Epic A1)

```
┌──────────────────────────────────────────────────────┐
│ DASHBOARD CRITICITÉ STATIONS - VUE D'ENSEMBLE       │
└──────────────────────────────────────────────────────┘

[1. QUOI ? - Vue d'ensemble]
┌─────────────┬─────────────┬────────────────────┐
│ SOR ACTUEL  │ DOR ACTUEL  │ NB STATIONS        │
│ 8.5%        │ 6.2%        │ CRITIQUES          │
│ ▲ +1.2      │ ▲ +0.8      │ 47 (3.1%)          │
│ 🔴 Obj: 6%  │ 🔴 Obj: 4.5%│                    │
└─────────────┴─────────────┴────────────────────┘

[2. OÙ ? - Localisation]
┌───────────────────────┬────────────────────────┐
│ TOP 10 STATIONS (SOR) │ CARTE GÉOGRAPHIQUE     │
│ 1. Bastille 12.5% 🔴  │ [Carte avec pins]      │
│ 2. Gare Nord 11.8% 🔴 │                        │
│ ...                   │                        │
└───────────────────────┴────────────────────────┘

[3. QUAND ? - Temporel]
┌──────────────────────────────────────────────────┐
│ ÉVOLUTION SOR/DOR (30 jours)                     │
│ [Courbe temporelle]                              │
└──────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────┐
│ HEATMAP CRITICITÉ (heure × jour)                 │
│     Lu  Ma  Me  Je  Ve  Sa  Di                   │
│ 8h  🔴 🔴 🔴 🔴 🔴 🟢 🟢                      │
│ ...                                              │
└──────────────────────────────────────────────────┘
```

Cette structure guide l'utilisateur logiquement : État global → Stations problématiques → Tendances → Patterns.

---

## Périmètre & Hors-périmètre

### Dans le périmètre
✅ Méthodologie conception BI (5 principes)
✅ Structure dashboard Vélib' (mockup)
✅ Dashboard vs Reporting

### Hors périmètre
❌ Implémentation technique (Metabase/Tableau)
❌ Requêtes SQL détaillées
❌ Wireframes haute-fidélité (mockups Figma)

---

## Limites & Risques

### Limites identifiées
1. **Mockup basse-fidélité** : Structure proposée, mais pas de vraie maquette graphique.
2. **Pas de test utilisateur** : On n'a pas pu valider ce dashboard avec chefs exploitation.
3. **Technologie non choisie** : "Metabase ou Tableau" → à décider en phase Développement.

### Risques
- **Sur-design** : On risque d'ajouter trop de visualisations (dérive scope).
- **Qualité données** : Si API GBFS instable, le dashboard affichera des données incohérentes.
- **Adoption** : Même si le dashboard est bien conçu, il faut que les utilisateurs l'ouvrent tous les jours.

---

## Impact & Next Steps

### Impact attendu
Si dashboard bien conçu et adopté :
- **Chefs exploitation** : Décisions basées sur données (exit le "feeling")
- **Équipes** : -20% SOR/DOR en 6 semaines
- **Direction** : Visibilité sur KPIs, reporting facile

### Next Steps
1. **Choisir techno** : Metabase (gratuit, simple) vs Tableau (payant, avancé)
2. **Wireframes** : Créer mockups haute-fidélité (Figma)
3. **Développement** : Implémenter dashboard avec données réelles
4. **Test utilisateur** : Session 30 min avec 2-3 chefs exploitation
5. **Itération** : Ajuster selon retours

