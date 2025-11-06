---
project: "Projet DPM – Vélib' Paris"
team: "Jordan S., Abdelmalek B."
formation: "RNCP 7 – Machine Learning & IA – DataScientest"
period: "Sept.–Nov. 2025"
role: "Data Product Manager (binôme)"
---

# 1.3.1 — Priorisation des améliorations

## Contexte & Objectifs

**Situation** : La Discovery (sections 1.2.1 et 1.2.2) a identifié 3 problèmes principaux. Maintenant, **par quoi commencer ?**

**Objectif** : Utiliser une méthode rigoureuse de priorisation pour justifier nos choix.

**Notre approche** : Méthode Impact × Effort (inspirée du framework RICE enseigné chez DataScientest).

---

## Les 3 problèmes identifiés en Discovery

**P1 - Stations critiques aux heures de pointe**
- **Constat** : Stock-out (0 vélo) et dock-out (0 borne libre) fréquents
- **Conséquences** : Détours, frustration, perte de temps

**P2 - Régulation peu efficace**
- **Constat** : Rééquilibrage réactif, pas prédictif
- **Conséquences** : Coûts logistiques élevés, efficacité variable

**P3 - Gestion des anomalies mal vécue**
- **Constat** : Parcours de réclamation pas optimal
- **Conséquences** : Tickets support, expérience dégradée

---

## Transformation en améliorations

**A1 - Réduire la criticité des stations aux heures de pointe**
- **Objectif** : -20% SOR/DOR sur Top 100 stations
- **Approche** : Alerting + ajustements capacité ciblés

**A2 - Optimiser les tournées de régulation**
- **Objectif** : +10-15 points efficacité (RE)
- **Approche** : Priorisation data-driven, algorithme optimisation

**A3 - Améliorer la gestion des anomalies**
- **Objectif** : -15% tickets support, -10% MTTR
- **Approche** : Diagnostic guidé dans l'app, FAQ contextuelle

**A4 - Recommander des alternatives aux usagers**
- **Objectif** : -10% JAR (Journey Abandon Rate), -15% détours
- **Approche** : Suggestions stations proches + probabilité dispo

---

## Méthode de priorisation : Impact × Effort

### Principe

```
Score = Impact / (Effort + 1)
```

Plus le score est élevé, plus l'amélioration est prioritaire.

### Échelle de notation

**Impact** (0 à 5) :
- Combine impact business ET impact utilisateur
- 5 = impact majeur, 0 = impact nul

**Effort** (0 à 5) :
- Complexité technique + qualité données + contraintes orga
- 5 = très difficile, 0 = trivial

### Critères détaillés

**Impact** (5 critères) :
- I1 - Acquisition/Rétention usagers
- I2 - Satisfaction client (NPS)
- I3 - Efficience opérationnelle (réduction coûts)
- I4 - Réduction risques/Conformité
- I5 - Alignement stratégique

**Effort** (3 critères) :
- F1 - Complexité technique
- F2 - Disponibilité/Qualité données
- F3 - Dépendances organisationnelles

---

## Scoring des 4 améliorations

**Pondérations choisies** (inspirées du cours DPM DataScientest) :

**Impact** :
- I1 = 0.25, I2 = 0.30, I3 = 0.30, I4 = 0.10, I5 = 0.05

**Effort** :
- F1 = 0.5, F2 = 0.3, F3 = 0.2

### Résultats

| Amélioration | Impact pondéré | Effort pondéré | Score final |
|--------------|---------------|---------------|-------------|
| **A1 - Réduire criticité stations** | 3.73 | 1.75 | **1.36** 🥇 |
| **A3 - Fluidifier anomalies** | 2.63 | 1.40 | **1.09** 🥈 |
| **A2 - Optimiser régulation** | 3.33 | 2.50 | **0.95** 🥉 |
| **A4 - Reco station + proba** | 2.98 | 2.15 | **0.95** 🥉 |

### Interprétation

**🥇 A1 en tête** (1.36)
- Fort impact sur satisfaction (4.5/5) ET efficience (4.0/5)
- Effort modéré (données GBFS disponibles)

**🥈 A3 en 2ème** (1.09) : quick win UX
- Peu de complexité technique (amélioration app)
- Impact satisfaisant sur satisfaction client

**🥉 A2 et A4 en 3ème** (0.95) : plus techniques
- Complexité plus élevée (algo optimisation, ML)
- Nécessitent plus de travail data

**Limite** : Ce scoring est **subjectif**. Les notes ont été attribuées par nous (binôme), pas validées avec les équipes Smovengo. Dans un vrai projet DPM, il faudrait un atelier de priorisation avec les parties prenantes.

---

## Feuille de route recommandée

Sur la base du scoring :

### Priorité 1 : A1 - Réduire criticité stations (4-6 semaines)

**Livrables** :
- Dashboard BI avec KPIs SOR/DOR
- Système d'alerting (Slack/Email si SOR > 40%)
- Vue Top stations critiques par créneau (8h-10h / 18h-20h)

**MVP** :
- Dashboard avec mise à jour quotidienne
- Alertes pour Top 10 stations critiques

**Critère de succès** : **-20% minutes en état critique** (vs baseline)

### Priorité 2 : A3 - Améliorer gestion anomalies (3-4 semaines)

**Livrables** :
- Diagnostic guidé dans l'app ("Vélo bloqué ? Suivez ces 3 étapes...")
- FAQ contextuelle

**Critère de succès** : **-15% tickets support** liés aux blocages

### Priorité 3 : A2 - Optimiser tournées régulation (8-10 semaines)

**Livrables** :
- Modèle prévision demande (baseline : moyenne mobile)
- Système scoring pour prioriser stations
- Algo heuristique optimisation tournées

**Critère de succès** : **+10 points RE** (Rebalancing Efficiency)

### Priorité 4 : A4 - Recommandations stations alternatives (6-8 semaines)

**Livrables** :
- Calcul probabilité dispo à H+15
- Reco stations alternatives (rayon 400m)
- Intégration app mobile

**Critère de succès** : **-10% JAR** (Journey Abandon Rate)

---

## Périmètre & Hors-périmètre

### Dans le périmètre
✅ Priorisation méthodologique (Impact/Effort)
✅ Définition des améliorations
✅ Feuille de route recommandée

### Hors périmètre
❌ Validation du scoring avec équipes Smovengo
❌ Implémentation des améliorations (on est en phase Conception)
❌ Ajustement dynamique de la priorisation

---

## Limites & Risques

### Limites identifiées
1. **Scoring subjectif** : Notes attribuées par nous (binôme), pas validées avec parties prenantes.
2. **Hypothèses non testées** : Ex. on suppose que l'effort de A1 est modéré (1.75/5), mais on n'a pas fait d'étude de faisabilité technique détaillée.
3. **Pas de validation utilisateurs** : On n'a pas pu présenter ce scoring aux équipes Smovengo pour validation.
4. **Pondérations arbitraires** : Les poids (I1=0.25, etc.) sont inspirés du cours, mais pas adaptés au contexte spécifique Vélib'.

### Risques
- **Biais confirmation** : On peut être tenté de scorer plus haut l'amélioration qu'on préfère.
- **Sur-estimation de l'impact** : Ex. on dit "-20% SOR", mais c'est peut-être optimiste.
- **Sous-estimation de l'effort** : A1 peut être plus complexe qu'on ne le pense (qualité données, bugs API GBFS, etc.).

---

## Impact & Next Steps

### Impact attendu
Si on suit cette priorisation :
- **Focus** sur l'amélioration à fort impact et effort modéré (A1)
- **Quick win** avec A3 (peu d'effort, impact satisfaisant)
- **Patience** pour A2 et A4 (plus techniques, à faire après)

### Next Steps
1. **Section 1.3.2** : Définir les KPIs pour mesurer A1
2. **Section 1.3.3** : Créer le Product Vision Board pour A1
3. **DPM-2** : Concevoir la solution (ML, BI)

---

## Hypothèses à valider

**H1** : A1 a vraiment un effort modéré (1.75/5)
**Validation** : Étude de faisabilité technique (accès GBFS, qualité données, partitioning PostgreSQL)

**H2** : Les pondérations (I1=0.25, etc.) sont adaptées au contexte Vélib'
**Validation** : Atelier avec parties prenantes (direction, équipes ops, produit)

**H3** : -20% SOR est atteignable en 6 semaines
**Validation** : Benchmarks internationaux (Citi Bike, Santander Cycles)

---

## Sources & Références

**Méthodologie** :
- Cours DataScientest DPM : Priorisation, RICE framework
- Article : "How to prioritize features" (Product School)

**Scoring** :
- Inspiré du framework RICE (Reach × Impact × Confidence / Effort)
- Adapté avec critères Impact (I1-I5) et Effort (F1-F3)

