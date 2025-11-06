---
project: "Projet DPM – Vélib' Paris"
team: "Jordan S., Abdelmalek B."
formation: "RNCP 7 – Machine Learning & IA – DataScientest"
period: "Sept.–Nov. 2025"
role: "Data Product Manager (binôme)"
rewritten_for: "Soutenance orale – version étudiante plausible"
---

# 1.3.2 — Définir les KPIs en amont de la conception

## Contexte & Objectifs

**Situation** : On a priorisé **A1 - Réduire la criticité des stations** (section 1.3.1). Maintenant, **comment mesurer si ça marche ?**

**Objectif** : Définir des KPIs (Key Performance Indicators) pour mesurer le succès avant/après.

**Pourquoi maintenant ?** Parce qu'il faut définir les KPIs AVANT de commencer à développer. Sinon, on risque de mesurer n'importe quoi.

**Rappel contexte** :
- **Stock-out** : station à 0 vélo
- **Dock-out** : station à 0 borne libre
- **Périmètre** : Top 100 stations les plus fréquentées
- **Actions envisagées** : alerting prédictif, ajustements capacité, régulation optimisée

---

## Pourquoi pas les KPIs "classiques" ?

On aurait pu utiliser des KPIs produits classiques :

**DAU/MAU** (Daily/Monthly Active Users)
- **Problème** : Trop général. Une hausse peut venir de la météo, d'une grève de métro, etc.

**NPS** (Net Promoter Score)
- **Problème** : Reflète la satisfaction globale, pas spécifiquement l'impact sur les stock-out/dock-out.

**CSAT** (Customer Satisfaction)
- **Problème** : Même problème, trop macro.

**Churn / Retention Rate**
- **Problème** : Dépend de plein de facteurs (prix, concurrence, qualité globale).

**Conclusion** : On a besoin de KPIs **spécifiques** qui mesurent directement le problème à résoudre (stations critiques).

---

## Les 5 KPIs retenus

| KPI | Mesure | Unité | Objectif | Limitation |
|-----|--------|-------|----------|------------|
| **KPI-1 : SOR** (Stock-Out Rate) | % temps sans vélo | % par station/créneau | **-20%** vs baseline | Exclure pannes techniques |
| **KPI-2 : DOR** (Dock-Out Rate) | % temps sans borne libre | % par station/créneau | **-15%** vs baseline | Vérifier capacité stable |
| **KPI-3 : Criticality Minutes** | Minutes/jour en état critique | Min/jour | **-25%** vs baseline | Comparer périodes équivalentes |
| **KPI-4 : RHR** (Rebalancing Hit Rate) | Efficacité tournées | % par tournée | **+10 points** | Nécessite logs opérationnels |
| **KPI-5 : JDT** (Journey Detour Time) | Temps de détour moyen | Min/trajet | **-15%** | Nécessite télémétrie app |

### Justification

**KPI 1, 2, 3 (Disponibilité)** : Mesurent directement le pain point #1 (Alice qui n'a pas de vélo le matin).

**KPI 4 (Ops)** : Mesure si les tournées de rééquilibrage sont efficaces (Julien).

**KPI 5 (UX)** : Mesure le temps perdu par les usagers quand ils doivent chercher une autre station.

### Formules de calcul

**Stock-Out Rate (SOR)** :
```
SOR = (nb minutes avec 0 vélo) / (minutes observées totales) × 100
```

**Dock-Out Rate (DOR)** :
```
DOR = (nb minutes avec 0 borne libre) / (minutes observées totales) × 100
```

**Criticality Minutes** :
```
Criticality Minutes = somme des minutes où (bikes_total = 0 OU docks_free = 0)
```

**Rebalancing Hit Rate (RHR)** :
```
RHR = (nb stations sorties de criticité 60 min après) / (nb stations ciblées) × 100
```

**Journey Detour Time (JDT)** :
```
JDT = distance vers station alternative × 12.5 min/km (vitesse marche ~4.8 km/h)
```

---

## Protocole de mesure Avant/Après

**Phase 1 : Baseline (2 semaines)**
- Mesurer SOR/DOR/Criticality Minutes sur Top 100 stations
- Focus créneaux critiques : 8h-10h (matin) et 18h-20h (soir)
- Noter conditions externes (météo, événements)

**Pourquoi 2 semaines ?** Pour lisser les variations jour à jour.

**Phase 2 : Déploiement**
- Activer système d'alerting et ajustements
- **Important** : figer les définitions et requêtes SQL (v1.0)
- Pas de modification pendant le test

**Phase 3 : Suivi post-déploiement (6-8 semaines)**
- Mesure hebdomadaire des KPIs
- Surveillance facteurs externes
- Documentation de tout ce qui pourrait biaiser les résultats

**Pourquoi 6-8 semaines ?** Pour avoir assez de recul et s'assurer que c'est durable.

**Phase 4 : Analyse statistique**
- Comparaison Avant vs Après
- Tests statistiques (Wilcoxon par station)
- Analyse par créneau horaire et zone géographique

**Pourquoi tests statistiques ?** Pour éviter de crier victoire si -5% SOR est juste de la variabilité normale.

**Phase 5 : Décision**
- Si objectifs atteints → extension à autres stations
- Si résultats mitigés → analyse causes et ajustements
- Dans tous les cas → documentation apprentissages

---

## Requêtes SQL (exemples)

### Calcul SOR/DOR par station et créneau

```sql
-- Étape 1 : Récupérer snapshots GBFS
WITH snaps AS (
  SELECT station_id, ts,
         (bikes_mech + bikes_elec) AS bikes_total,
         docks_free
  FROM fct_station_snapshot
  WHERE ts >= :date_start AND ts < :date_end
),

-- Étape 2 : Classifier par créneau
bucket AS (
  SELECT station_id,
    CASE
      WHEN EXTRACT(HOUR FROM ts) BETWEEN 8 AND 9 THEN 'AM_peak'
      WHEN EXTRACT(HOUR FROM ts) BETWEEN 18 AND 19 THEN 'PM_peak'
      ELSE 'OFF'
    END AS slot,
    (bikes_total = 0)::int AS is_stockout,
    (docks_free = 0)::int AS is_dockout
  FROM snaps
)

-- Étape 3 : Agréger
SELECT station_id, slot,
       100.0 * AVG(is_stockout) AS sor_pct,
       100.0 * AVG(is_dockout) AS dor_pct
FROM bucket
WHERE slot IN ('AM_peak', 'PM_peak')
GROUP BY station_id, slot;
```

### Calcul RHR (efficacité rééquilibrage)

```sql
-- Étape 1 : Identifier interventions
WITH targets AS (
  SELECT i.intervention_id, i.ended_at, s.station_id
  FROM intervention i
  JOIN intervention_station s USING (intervention_id)
),

-- Étape 2 : Vérifier état 60 min après
crit AS (
  SELECT t.intervention_id, t.station_id,
         AVG((bikes_mech + bikes_elec) = 0 OR docks_free = 0) FILTER (
           WHERE ts BETWEEN t.ended_at AND t.ended_at + interval '60 min'
         ) AS crit_post
  FROM targets t
  JOIN fct_station_snapshot f ON f.station_id = t.station_id
  GROUP BY 1, 2
)

-- Étape 3 : Calculer taux de succès
SELECT 100.0 * AVG((crit_post = 0)::int) AS rhr_pct
FROM crit;
```

---

## Périmètre & Hors-périmètre

### Dans le périmètre
✅ Définition des 5 KPIs (SOR, DOR, Criticality Min, RHR, JDT)
✅ Requêtes SQL d'exemple
✅ Protocole de mesure avant/après

### Hors périmètre
❌ Implémentation du pipeline de logging (on n'a pas les données)
❌ Mesure réelle des KPIs (section méthodologique)
❌ Accès aux logs internes (tournées, télémétrie app)

---

## Limites & Risques

### Limites identifiées
1. **KPI 1, 2, 3 calculables** : On peut les calculer avec GBFS (si on historise).
2. **KPI 4 (RHR) difficile** : Nécessite logs opérationnels des tournées (on n'y a pas accès).
3. **KPI 5 (JDT) difficile** : Nécessite télémétrie app (on n'y a pas accès).
4. **Focus KPI 1, 2, 3** : Pour le projet étudiant, on se concentrera sur SOR/DOR/Criticality Minutes.

### Risques
- **Pas de baseline réelle** : On n'a pas historisé GBFS → pas de mesure avant/après possible pour le projet étudiant.
- **Biais temporels** : Si on compare des périodes différentes (ex. septembre vs novembre), la météo peut biaiser les résultats.
- **Manque de puissance statistique** : 2 semaines de baseline peuvent ne pas suffire (idéalement 4-6 semaines).

---

## Objectifs chiffrés

| KPI | Objectif | Réaliste ? |
|-----|----------|-----------|
| **SOR** | **-20%** | Ambitieux mais atteignable (benchmarks internationaux : -20 à -30%) |
| **DOR** | **-15%** | Raisonnable |
| **Criticality Min** | **-25%** | Cohérent avec SOR/DOR |
| **RHR** | **+10 points** | Nécessite validation avec équipes ops |
| **JDT** | **-15%** | Dépend de l'adoption des recommandations |

**Garde-fous** :
- Comparer périodes équivalentes (mêmes jours de semaine)
- Exclure périodes anormales (grèves, événements majeurs, météo extrême)
- Versionner définitions KPIs (v1.0) et ne JAMAIS les modifier pendant le test
- Documenter tous les biais potentiels

---

## Organisation du suivi

### Responsabilités

| Équipe | KPIs | Source données |
|--------|------|----------------|
| **Équipe Data** | SOR, DOR, Criticality Min | GBFS historisé |
| **Équipe Ops** | RHR | Logs interventions |
| **Équipe Produit** | JDT | Télémétrie app |

### Cadence

**Revue hebdomadaire** (équipe ops) :
- Point évolution KPIs semaine par semaine
- Identification rapide des problèmes

**Revue mensuelle** (comité produit) :
- Décisions stratégiques : ajuster ? étendre ?
- Partage apprentissages

### Infrastructure

**Dashboard BI centralisé** :
- Visualisation tous KPIs en temps réel
- Comparaison avant/après sur mêmes graphiques
- Filtres par station, créneau, zone

**Système d'alertes** :
- Notifications Slack/Email si KPI dépasse seuil critique
- Ex. : alerte si SOR d'une station > 50% pendant 3 jours

---

## Impact & Next Steps

### Impact attendu
Si on implémentait ce protocole :
- **Baseline claire** : photo de l'état actuel
- **Mesure objective** : preuves chiffrées de l'amélioration (ou pas)
- **Apprentissages** : comprendre ce qui marche et ce qui ne marche pas

### Next Steps
1. **Section 1.3.3** : Product Vision Board pour A1
2. **DPM-2** : Conception ML et BI

**Réalité du projet** : On n'a pas le temps d'implémenter le pipeline de logging et de mesurer réellement. Mais la méthodologie est documentée.

---

## Hypothèses à valider

**H1** : -20% SOR est atteignable en 6 semaines
**Validation** : Benchmarks internationaux (Citi Bike : -20 à -30% après optimisation)

**H2** : Les tests statistiques montreront une différence significative
**Validation** : Calcul puissance statistique (nécessite estimation variance SOR)

**H3** : KPI 4 (RHR) et KPI 5 (JDT) sont mesurables
**Validation** : Accès aux logs internes (pas possible pour le projet étudiant)

---

## Sources & Références

**Méthodologie** :
- Cours DataScientest DPM : KPIs, mesure avant/après
- Article : "How to measure product success" (Product School)

**Tests statistiques** :
- Test de Wilcoxon (non-paramétrique, adapté aux données temporelles)
- Puissance statistique : nécessite variance SOR estimée

