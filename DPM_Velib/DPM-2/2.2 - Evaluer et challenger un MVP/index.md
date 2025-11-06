---
project: "Projet DPM – Vélib' Paris"
team: "Jordan S., Abdelmalek B."
formation: "RNCP 7 – Machine Learning & IA – DataScientest"
period: "Sept.–Nov. 2025"
role: "Data Product Manager (binôme)"
rewritten_for: "Soutenance orale – version étudiante plausible"
---

# 2.2 — Évaluer et challenger un MVP

## Contexte & Objectifs

**Situation** : On a défini les fonctionnalités d'un MVP (Epic A1 - Dashboard BI). Avant de développer, **il faut valider que c'est pertinent et viable**.

**Objectif** : Challenger le MVP sur 3 axes : faisabilité technique, besoin utilisateur, positionnement.

**Statistiques** :
- Gartner (2019) : 20% seulement des initiatives BI atteignent des résultats
- VentureBeat (2019) : 87% des projets ML n'atteignent jamais la production
- Forrester (2020) : 73% des projets data n'impactent jamais les décisions

**Pourquoi ?** Détecter les risques AVANT d'investir massivement.

---

## Axe 1 : Faisabilité technique

### Principe MVM (Minimum Viable Model)

**Hiérarchie de complexité** :

| Niveau | Approche | Effort | Performance | Décision |
|--------|----------|--------|-------------|----------|
| 0 | Règle métier statique | 1 jour | Baseline | Trop faible |
| 1 | Analyse BI descriptive | 1-2 sem | Insight | ✅ **MVP A1** |
| 2 | Modèle statistique simple | 2-3 sem | Prédiction basique | Pour A2 |
| 3 | Modèle ML classique | 4-6 sem | Prédiction avancée | Pour A2 |
| 4 | Modèle ML avancé | 8-12 sem | SOTA | Trop coûteux |

**Pour Epic A1 (Dashboard BI)** : Niveau 1 suffisant (analyse descriptive).

**Pour Epic A2 (Prévision demande)** : Niveau 3 (LightGBM) meilleur compromis.

### Qualité des données (Framework FACTT)

| Critère | GBFS Vélib' | Évaluation |
|---------|-------------|------------|
| **Facilement accessible** | API publique, doc complète | ✅ |
| **Actuelles** | Mise à jour toutes les 10 min | ✅ |
| **Complètes** | Taux complétude > 95% | ✅ |
| **Traçables** | Historique depuis 2020 | ✅ |
| **Transformables** | Format JSON standard | ✅ |

**Conclusion** : Données GBFS suffisantes pour MVP.

---

## Axe 2 : Besoin utilisateur

### Validation du besoin

**Questions clés** :
- Le problème existe-t-il vraiment ?
- Les utilisateurs sont-ils prêts à changer leurs habitudes ?
- Le MVP apporte-t-il assez de valeur ?

**Méthodes validation** :
1. **Entretiens** (3-5 utilisateurs cibles)
2. **Prototype basse-fidélité** (mockup papier)
3. **Test d'usage** (30 min observation)

**Vélib' - Epic A1** :

**Limite** : On n'a pas pu interviewer de chefs d'exploitation Smovengo (pas de contact).

**Compensation** : Sources secondaires (articles, rapports, avis) + hypothèses raisonnables.

**Hypothèses à valider** :
- H1 : Chefs exploitation consulteraient dashboard quotidiennement
- H2 : Format proposé (KPIs + Top 10 + carte) est adapté
- H3 : Dashboard remplacerait reporting Excel actuel

**Risque** : Hypothèses non validées → adoption incertaine.

---

## Axe 3 : Positionnement concurrentiel

### Analyse concurrence

**Questions** :
- Des solutions similaires existent-elles ?
- Quels sont les avantages/inconvénients ?
- Peut-on se différencier ?

**Vélib' - Epic A1** :

**Solutions existantes** :
1. **Reporting Excel manuel** (utilisé actuellement)
   - ✅ Familier
   - ❌ Fastidieux, pas temps réel, erreurs
2. **Dashboards génériques BI** (Tableau, Power BI)
   - ✅ Puissants
   - ❌ Pas spécifiques Vélib', nécessitent formation

**Notre différenciation** :
- ✅ Spécifique problème criticité stations
- ✅ KPIs métier (SOR, DOR) calculés automatiquement
- ✅ Temps réel (mise à jour quotidienne)
- ✅ Interface simple, pas de formation nécessaire

---

## Grille de validation MVP

| Critère | Évaluation | Commentaire |
|---------|------------|-------------|
| **Faisabilité technique** | ✅ | Données GBFS OK, BI descriptive suffisante |
| **Besoin utilisateur** | ⚠️ | Hypothèse non validée avec vrais users |
| **Positionnement** | ✅ | Se différencie des solutions existantes |
| **Budget** | ✅ | Metabase gratuit, 4-6 sem dev acceptable |
| **Conformité RGPD** | ✅ | Données anonymisées (pas d'infos personnelles) |

**Décision** : **GO** pour développer le MVP, avec risque adoption à surveiller.

---

## Périmètre & Hors-périmètre

### Dans le périmètre
✅ Validation faisabilité technique (données, complexité)
✅ Analyse positionnement concurrentiel
✅ Grille de validation MVP

### Hors périmètre
❌ Entretiens utilisateurs (chefs exploitation)
❌ Prototype testable (wireframes haute-fidélité)
❌ A/B testing

---

## Limites & Risques

### Limites identifiées
1. **Pas de validation users** : On n'a pas pu interviewer de chefs exploitation → hypothèses non testées.
2. **Pas de prototype testable** : On a un mockup basse-fidélité, pas une vraie maquette interactive.
3. **Budget hypothétique** : On estime 4-6 semaines dev, mais pas de chiffrage détaillé.

### Risques
- **Non-adoption** : Même si le dashboard est bien conçu, les chefs exploitation peuvent ne pas l'utiliser (habitudes, résistance au changement).
- **Qualité données** : Si API GBFS instable ou avec bugs, le dashboard sera inutilisable.
- **Sur-scoping** : Risque d'ajouter trop de fonctionnalités après coup (dérive scope).

---

## Impact & Next Steps

### Impact attendu
Si MVP validé et développé :
- **Chefs exploitation** : Gain de temps (exit Excel), décisions data-driven
- **Opérateurs** : -20% SOR/DOR en 6 semaines
- **Direction** : Visibilité KPIs, reporting facilité

### Next Steps
1. **Développement** : Implémenter MVP (Metabase + PostgreSQL)
2. **Test alpha** : 1 semaine avec 2 chefs exploitation volontaires
3. **Itération** : Ajuster selon retours
4. **Déploiement** : Rollout progressif (10 → 50 → 100% users)
5. **Mesure impact** : Suivi KPIs avant/après

