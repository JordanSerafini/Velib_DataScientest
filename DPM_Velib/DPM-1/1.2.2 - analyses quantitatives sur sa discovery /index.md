---
project: "Projet DPM – Vélib' Paris"
team: "Jordan S., Abdelmalek B."
formation: "RNCP 7 – Machine Learning & IA – DataScientest"
period: "Sept.–Nov. 2025"
role: "Data Product Manager (binôme)"
---

# 1.2.2 — Analyses quantitatives de la Discovery

## Contexte & Objectifs

**Objectif** : Mesurer l'ampleur des problèmes identifiés en qualitative (section 1.2.1).

**Pourquoi ?** Parce qu'on ne peut pas améliorer ce qu'on ne mesure pas. Et surtout, il faut des chiffres pour :
- Prioriser les problèmes (Impact/Effort)
- Définir des KPIs avant/après
- Convaincre les parties prenantes qu'il y a un vrai problème

**Notre approche** : On va définir des **métriques opérationnelles** (SOR, DOR, RE, etc.) et des **requêtes SQL** pour les calculer.

**Limite** : On n'a pas encore historisé les données GBFS. Cette section est **méthodologique** (comment on ferait si on avait les données). Pour le projet étudiant, on n'a pas eu le temps d'implémenter le pipeline de logging.

---

## A) Backlog d'analyses : questions et hypothèses

Pour chaque problème identifié en qualitative, on pose une **question précise** et on formule une **hypothèse à tester**.

### Axe A — Disponibilité (stations vides/pleines)

**Q1. Taux de criticité par station et créneau**

**Question** : Quelle part du temps chaque station est en **stock-out** (0 vélo) et **dock-out** (0 place) par créneau (matin/soir/week-end) ?

**Hypothèse H1** : Les pics 8h-10h et 18h-20h génèrent une hausse significative de stock-out/dock-out sur les zones bureau (stock-out le soir) et résidentiel (stock-out le matin).

**Q2. Clusters de stations**

**Question** : Quels **clusters de stations** (quartiers) partagent la même dynamique de criticité ?

**Hypothèse H2** : Les patterns de criticité sont répétables (ex. stations près des gares ont toutes un pic stock-out le matin).

### Axe B — Régulation & opérations

**Q3. Efficacité du rééquilibrage**

**Question** : Après une tournée de rééquilibrage, quelle proportion de stations ciblées sortent durablement de l'état critique ?

**Hypothèse H3** : L'efficacité est meilleure la nuit (moins de contraintes logistiques, demande plus stable).

**Limite** : Cette métrique nécessite les logs de tournées (on n'y a pas accès).

**Q4. Temps de retour en service (MTTR)**

**Question** : Quel est le **temps moyen de retour en service** d'une station après blocage/panne ?

**Proxy** : Longue inactivité détectable via GBFS (station avec 0 mouvement pendant >30 min).

**Limite** : Proxy imparfait. Idéalement, on aurait besoin des logs maintenance.

### Axe C — Usage & mix

**Q5. Turnover et criticité**

**Question** : Quel est le **turnover** (prises/dépôts par heure) par station et sa corrélation avec criticité ?

**Hypothèse H5** : Turnover élevé → risques de stock-out/dock-out plus fréquents (volatilité).

**Q6. Impact du mix VAE/mécaniques**

**Question** : Le **mix VAE/mécaniques** influe-t-il sur la criticité ?

**Hypothèse H6** : Pénurie de VAE aux heures de pointe (forte demande pour trajets longs/vallonnés).

### Axe D — Facteurs externes (météo/événements)

**Q7. Impact météo**

**Question** : Quel impact de la **météo** (pluie/température) sur la demande et criticité ?

**Hypothèse H7** : La pluie diminue la demande globale mais accentue les déséquilibres localement.

**Limite** : On n'a pas intégré de données météo. À faire dans une version ultérieure.

**Q8. Impact événements**

**Question** : Quel impact des **événements** (concerts, manifestations) sur les stations alentour ?

**Hypothèse H8** : Pics ponctuels prédictibles (ex. concert Stade de France → stations vides avant, pleines après).

**Limite** : Nécessiterait un calendrier événementiel (pas fait dans le projet).

---

## B) Métriques & formules

Voici les formules exactes pour calculer les indicateurs (directement utilisables en SQL ou Python).

### B1 — Disponibilité

**Stock-out rate (SOR)** — station *s*, période *T*

```
SOR = (Minutes où bikes_total = 0) / (Minutes observées)
```

**Exemple** : Si République est à 0 vélo pendant 120 minutes sur 1440 minutes (1 jour), alors SOR = 120/1440 = 8,3%

**Dock-out rate (DOR)** — station *s*, période *T*

```
DOR = (Minutes où docks_free = 0) / (Minutes observées)
```

**Criticality Index (CI)** — Indice combiné

```
CI = w1 × SOR + w2 × DOR
```

*(poids à calibrer, ex. w1=0,6, w2=0,4 si manquer de vélo est pire que manquer de place)*

### B2 — Opérations

**Rebalancing Efficiency (RE)** — sur une tournée *τ*

```
RE = (Nb stations sorties d'état critique 30-60 min après) / (Nb stations ciblées)
```

**Exemple** : Tournée de 10 stations, 8 sortent de criticité 1h après → RE = 8/10 = 80%

**MTTR** — (proxy)

```
MTTR = avg(t_résolution - t_incident)
```

*Définir un incident par règle (ex. bornes libres = 0 & vélos inchangés >30 min)*

### B3 — Usage & mix

**Turnover/h (TOH)**

```
TOH = Nb prises + Nb dépôts par heure
```

*(déduit des deltas d'inventaire si on historise)*

**VAE share (VAE_%)**

```
VAE_% = (vélos_élec_dispos) / (vélos_totaux_dispos)
```

---

## C) Base de données : d'où viennent les données ?

**Source primaire** : Flux temps réel **GBFS Vélib'**
- Vélos mécaniques/électriques par station
- Bornes libres par station
- **Action requise** : Historiser à fréquence 60s

**Pourquoi 60 secondes ?** Compromis entre précision et volume (1 ligne toutes les 60s pour 1500 stations = ~2,5 millions de lignes/jour).

**Limite** : On n'a pas implémenté le pipeline de logging pour ce projet (contrainte temps). Cette section est méthodologique : on documente comment on ferait si on avait les données.

---

## D) Pipeline & modèle de données

### Schéma PostgreSQL proposé

```sql
-- Table dimensionnelle : stations (données statiques)
CREATE TABLE dim_station (
  station_id   INT PRIMARY KEY,
  name         TEXT,
  capacity     INT,
  lat          DOUBLE PRECISION,
  lon          DOUBLE PRECISION,
  commune      TEXT
);

-- Table de faits : snapshots temps réel
CREATE TABLE fct_station_snapshot (
  ts           TIMESTAMPTZ NOT NULL,
  station_id   INT NOT NULL REFERENCES dim_station(station_id),
  bikes_mech   INT,
  bikes_elec   INT,
  docks_free   INT,
  PRIMARY KEY (ts, station_id)
) PARTITION BY RANGE (ts);
```

**Architecture** : Modèle en étoile (star schema) classique avec 1 dimension (stations) et 1 table de faits (snapshots).

**Pourquoi partitionner ?** Avec 2,5M lignes/jour, une table monolithique devient ingérable. Partitionner par jour permet de requêter efficacement sur une période donnée.

### Requêtes SQL d'exemple

#### Calcul du Stock-out / Dock-out par station & créneau

```sql
-- Étape 1 : Récupérer les snapshots sur la période
WITH snaps AS (
  SELECT station_id,
         date_trunc('hour', ts) AS h,
         (bikes_mech + bikes_elec) AS bikes_total,
         docks_free
  FROM fct_station_snapshot
  WHERE ts >= :date_start AND ts < :date_end
)

-- Étape 2 : Calculer les taux de criticité
SELECT station_id,
       h,
       100.0 * AVG( (bikes_total = 0)::INT ) AS stockout_rate_pct,
       100.0 * AVG( (docks_free = 0)::INT ) AS dockout_rate_pct
FROM snaps
GROUP BY station_id, h
ORDER BY h, station_id;
```

**Explication** :
- `(bikes_total = 0)::INT` → Convertit TRUE/FALSE en 1/0
- `AVG(...)` → Calcule le % de temps en stock-out
- `100.0 *` → Convertit en pourcentage

#### Turnover/h (approché par delta d'inventaire)

```sql
WITH s AS (
  SELECT station_id, ts,
         (bikes_mech + bikes_elec) AS bikes_total
  FROM fct_station_snapshot
  WHERE ts >= :date_start AND ts < :date_end
),
d AS (
  SELECT station_id,
         date_trunc('hour', ts) AS h,
         SUM(ABS(bikes_total - LAG(bikes_total) OVER (PARTITION BY station_id ORDER BY ts))) AS delta_abs
  FROM s
)
SELECT station_id, h, COALESCE(AVG(delta_abs),0) AS turnover_per_hour
FROM d
GROUP BY station_id, h;
```

**Explication** :
- `LAG(bikes_total)` → Valeur précédente (snapshot N-1)
- `ABS(... - LAG(...))` → Delta absolu (prises + dépôts)
- `SUM(...)` → Somme des mouvements sur l'heure

---

## E) Visualisations & tests statistiques

Pour communiquer les analyses, on recommande :

**Heatmaps** :
- SOR/DOR par station & créneau (matin/soir/week-end)
- → Voir d'un coup d'œil quelles stations sont critiques et quand

**Courbes temporelles** :
- SOR/DOR vs heure sur Top 100 stations
- → Confirme visuellement les pics 8h-10h et 18h-20h

**Barres** :
- RE & SR par tournée, par équipe, par créneau
- → Compare l'efficacité des stratégies de régulation

**Scatter plots** :
- TOH vs CI (corrélation)
- → Teste H5 : plus de turnover = plus de criticité ?

**Boxplots** :
- SOR/DOR sous pluie vs sec
- Tests statistiques : t-test (si normalité) / Mann-Whitney (sinon)

**Avant/Après** :
- Évolution après changement process
- → Test d'interruption (causal impact simple)

---

## F) Interprétation & décisions

Comment utiliser ces métriques pour prendre des décisions ?

**Si SOR/DOR > 20%** sur 7 jours → **Cible prioritaire** de rééquilibrage ou ajustement capacité

**Si TOH élevé & CI élevé** → Envisager **capacité** plus grande OU **reco station alternative** dans l'app

**Si RE/SR faibles en journée** → Déplacer actions vers **nuit/early morning**

**Si ΔSOR|pluie significatif** → Adapter planification selon météo

**Exemple** :
- Station République : SOR = 35% aux heures de pointe, TOH = 25 mouvements/h
- → Décision : Priorité 1 pour ajout bornes + alerting régulateurs

---

## Périmètre & Hors-périmètre

### Dans le périmètre
✅ Définition des métriques (SOR, DOR, TOH, RE, MTTR)
✅ Requêtes SQL d'exemple
✅ Schéma de base de données proposé
✅ Hypothèses à tester

### Hors périmètre
❌ Implémentation du pipeline de logging GBFS (pas fait)
❌ Historique de données réel (on n'a pas les données)
❌ Analyses statistiques approfondies (tests de significativité)
❌ Intégration données météo/événements

---

## Limites & Risques

### Limites identifiées
1. **Pas de données historiques** : On n'a pas implémenté le pipeline de logging → section méthodologique uniquement.
2. **Proxies imparfaits** : MTTR et RE nécessiteraient logs internes (tournées, maintenance) qu'on n'a pas.
3. **Facteurs externes** : Météo et événements non intégrés → analyses incomplètes.
4. **Volume de données** : 2,5M lignes/jour est gérable en PostgreSQL mais nécessite partitioning et optimisation.

### Risques
- **Overhead technique** : Mettre en place le pipeline GBFS + PostgreSQL peut prendre plusieurs jours → pas fait dans le temps imparti.
- **Qualité des données** : API GBFS peut avoir des trous (stations offline, erreurs 5xx) → nécessite monitoring et data quality checks.
- **Coût infrastructure** : Héberger PostgreSQL + logs continus peut coûter cher si on le fait sur cloud (AWS RDS, etc.).

---

## Impact & Next Steps

### Impact attendu
Si on implémentait ce pipeline :
- **Baseline KPIs** : On pourrait calculer SOR/DOR actuels (état des lieux)
- **Priorisation** : On saurait quelles stations traiter en priorité
- **Mesure avant/après** : On pourrait mesurer l'impact des améliorations

### Next Steps (si on avait plus de temps)
1. Implémenter le pipeline de logging GBFS (Python + cron job + PostgreSQL)
2. Historiser pendant 2-4 semaines pour avoir une baseline
3. Calculer les métriques (SOR, DOR, TOH) avec les requêtes SQL
4. Créer des dashboards de monitoring (Metabase ou Looker)
5. Analyser et prioriser (section 1.3.1)

**Réalité du projet** : On n'a pas eu le temps de faire tout ça. Mais la méthodologie est documentée pour une implémentation future.

---

## Hypothèses à valider (avec données)

**H1** : SOR/DOR > 20% aux heures de pointe sur Top 100 stations
**Validation** : Requête SQL sur snapshots historisés

**H2** : Stations résidentielles = stock-out matin, stations bureaux = stock-out soir
**Validation** : Analyse par zone (résidentiel vs bureaux)

**H3** : Efficacité rééquilibrage meilleure la nuit
**Validation** : Calcul RE par créneau (nécessite logs tournées)

**H4** : Turnover élevé corrélé avec criticité élevée
**Validation** : Scatter plot TOH vs CI + test corrélation

**H5** : VAE sur-sollicités aux heures de pointe
**Validation** : Analyse VAE_% par créneau

---

## Sources & Références

**Méthodologie** :
- Cours DataScientest DPM : Métriques, KPIs, analyses quantitatives
- SQL pour l'analyse de données (PostgreSQL, window functions)
- Modélisation en étoile (star schema)

**Données** :
- API GBFS Vélib' : transport.data.gouv.fr, prim.iledefrance-mobilites.fr
- Docs GBFS : github.com/MobilityData/gbfs

**Outils** :
- PostgreSQL (partitioning, time-series)
- Python (pandas, requests pour l'ingestion)
- Metabase ou Looker (dashboards BI)

