---
project: "Projet DPM – Vélib' Paris"
team: "Jordan S., Abdelmalek B."
formation: "RNCP 7 – Machine Learning & IA – DataScientest"
period: "Sept.–Nov. 2025"
role: "Data Product Manager (binôme)"
rewritten_for: "Soutenance orale – version étudiante plausible"
---

# 2.4 — Évaluation coût financier architecture data

## Contexte & Objectifs

**Situation** : On a défini les solutions ML et BI. **Combien ça coûte de les mettre en œuvre ?**

**Objectif** : Estimer les coûts d'infrastructure data (cloud + équipes).

**Pourquoi ?**
- ❌ Sans estimation : sous-estimer budget → projet bloqué, explosion factures cloud après 6 mois
- ✅ Avec estimation : budget réaliste, arbitrage éclairé, ROI crédible

---

## Les 5 axes de coûts

### 1. Sources de données
- API externes → Coûts abonnement ou paiement à l'usage
- **Vélib'** : API GBFS gratuite (publique) → **0€/mois**

### 2. Types de données
- Structurées (SQL) → Stockage relationnel optimisé
- **Vélib'** : Données structurées (JSON → PostgreSQL) → économique

### 3. Volume & croissance
- **Vélib'** :
  - Volume initial : ~80M lignes (2 ans historique) = ~5 Go
  - Croissance : ~80k lignes/jour × 365 = ~2 Go/an
  - Total après 3 ans : ~11 Go

### 4. Stockage & traitement
- Cloud vs on-premise
- **Vélib'** : PostgreSQL cloud (AWS RDS ou équivalent)

### 5. Équipes & outils
- Salaires (40-100k€/an/personne)
- Outils (0-5000€/mois)

---

## Estimation coûts Vélib' (Epic A1 + A2)

### Infrastructure cloud (AWS)

| Service | Usage | Coût mensuel |
|---------|-------|--------------|
| **RDS PostgreSQL** (db.t3.medium) | Stockage 20 Go + compute | **~50€/mois** |
| **Lambda** (ingestion GBFS) | 43k invocations/mois | **~1€/mois** |
| **S3** (archivage) | 100 Go données brutes | **~2€/mois** |
| **CloudWatch** (monitoring) | Logs + alerting | **~5€/mois** |
| **Metabase** (dashboard BI) | Hébergé sur EC2 t3.small | **~15€/mois** |
| **TOTAL Infrastructure** | | **~73€/mois** |

**Coût annuel** : 73€ × 12 = **~876€/an**

### Équipes (estimation)

**Pour projet étudiant** : Binôme (0€ salaire)

**Pour projet réel Smovengo** :
| Profil | Charge | Salaire annuel | Coût projet (6 mois) |
|--------|--------|----------------|----------------------|
| **Data Engineer** | 50% (pipeline, infra) | 50k€ | 25k€ |
| **Data Scientist** (pour Epic A2) | 50% (modèle ML) | 55k€ | 27,5k€ |
| **Product Manager** | 25% (coordination, validation) | 60k€ | 15k€ |
| **TOTAL Équipes** | | | **67,5k€ (6 mois)** |

### Outils & licences

| Outil | Usage | Coût |
|-------|-------|------|
| **Metabase** | Dashboard BI (open-source) | 0€/mois |
| **Notion** | Backlog (≤10 users) | 0€/mois |
| **GitHub** | Code + versioning | 0€/mois |
| **Slack** | Communication | 0€/mois (gratuit) |
| **TOTAL Outils** | | **0€/mois** |

### Coût total projet (6 mois)

| Poste | Coût 6 mois |
|-------|-------------|
| Infrastructure cloud | 438€ (73€ × 6) |
| Équipes | 67 500€ |
| Outils | 0€ |
| **TOTAL** | **~67 940€** |

**Arrondi** : **~68k€ pour 6 mois** (MVP Epic A1 + A2 Phase 1)

---

## ROI (Return On Investment)

**Gains métier estimés** (cf. Product Vision Board 1.3.3) :
- Économies logistiques : 15-25k€/an
- Revenus supplémentaires (JAR ↓) : 80-150k€/an
- Rétention abonnés (churn ↓) : 150-300k€/an
- **TOTAL Gains** : 245-475k€/an

**ROI** :
```
ROI = (Gains annuels - Coûts annuels) / Coûts annuels

Optimiste : (475k - 68k) / 68k = 598% ROI
Conservateur : (245k - 68k) / 68k = 260% ROI
```

**Conclusion** : ROI positif dès année 1, entre 260% et 598%.

**Payback period** : **< 2 mois** (68k€ / (245-475k€/12 mois))

---

## Périmètre & Hors-périmètre

### Dans le périmètre
✅ Estimation infrastructure cloud (AWS)
✅ Estimation équipes (salaires)
✅ Calcul ROI

### Hors périmètre
❌ Chiffrage détaillé avec devis fournisseurs
❌ Comparatif AWS vs GCP vs Azure
❌ Optimisation coûts (right-sizing, reserved instances)

---

## Limites & Risques

### Limites identifiées
1. **Estimations approximatives** : Coûts basés sur tarifs publics AWS (peuvent varier selon région, volume).
2. **Hypothèses non validées** : Gains métier (245-475k€) basés sur hypothèses (cf. 1.3.3), pas de données réelles Smovengo.
3. **Pas de coûts cachés** : Formation utilisateurs, conduite du changement, maintenance non comptabilisés.

### Risques
- **Sous-estimation** : Si volumes de données explosent, coûts cloud peuvent doubler.
- **Suroptimisme gains** : Si hypothèses ROI sont fausses, le projet peut ne pas être rentable.
- **Coûts récurrents** : 876€/an infra + maintenance continue (non comptabilisée).

