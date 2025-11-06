---
project: "Projet DPM – Vélib' Paris"
team: "Jordan S., Abdelmalek B."
formation: "RNCP 7 – Machine Learning & IA – DataScientest"
period: "Sept.–Nov. 2025"
role: "Data Product Manager (binôme)"
rewritten_for: "Soutenance orale – version étudiante plausible"
---

# 2.1.1 — Méthodologie conception produit ML

## Contexte & Objectifs

**Situation** : On a identifié un besoin (Epic A2 - Optimiser régulation). Maintenant, **comment passer de "on va prédire la demande" à un vrai produit utilisable ?**

**Objectif** : Fournir une méthodologie pour transformer une idée ML en produit fonctionnel.

**Pourquoi ?** Pour éviter le piège classique : "On entraîne un modèle... et après ?"

---

## 1. Spécifier le cas d'usage (5 questions)

Avant de coder quoi que ce soit, répondre à 5 questions :

### Q1 : Qui utilise les prédictions ?
- Utilisateur : Régulateurs terrain (15-20 personnes)
- Profil : peu technique, smartphone, en mouvement
- Fréquence : 4-6 fois/shift

### Q2 : À quoi servent les prédictions ?
- **Action** : Décider quelles stations réguler en priorité
- **Impact** : Réduire SOR/DOR
- **Criticité** : Fausse prédiction → déplacement inutile (~20-30€)

### Q3 : Comment s'intègrent-elles dans le workflow ?

**AVANT (sans ML)** :
1. Régulateur reçoit liste fixe de stations
2. Suit tournée, sans priorisation dynamique
3. Arrive trop tard ou trop tôt

**APRÈS (avec ML)** :
1. Ouvre app mobile à 9h
2. Voit carte avec prédictions criticité à 30 min (code couleur)
3. App propose itinéraire optimisé
4. Valide → itinéraire envoyé au GPS

### Q4 : Quelle forme prennent les prédictions ?
- **Format** : Classe criticité (🟢 Normal / 🟠 Attention / 🔴 Critique) + score confiance (0-100%)
- **Fréquence** : Mise à jour toutes les 10 min
- **Horizon** : Prédiction à +30 min

### Q5 : Comment mesurer le succès ?
- **KPI métier** : -30% SOR/DOR en 6 mois, RE de 65% à 80%
- **KPI technique** : Précision ≥ 75%, Rappel ≥ 70%

**Checklist** : Si une seule question n'a pas de réponse claire → **STOP**, ne pas passer à l'étape suivante. Retourner voir les utilisateurs.

---

## 2. Cahier des charges ML (8 composantes)

### 2.1 Tâche ML
**Type** : Classification multi-classe (🟢 / 🟠 / 🔴)

**Pourquoi pas régression ?** Régulateur veut juste savoir "je dois y aller maintenant ?" (signal actionnable).

### 2.2 Variable cible (Target)
```
criticality_level_30min:
- 0 = 🟢 Normal (≥ 3 vélos ET ≥ 3 bornes)
- 1 = 🟠 Attention (1-2 vélos OU 1-2 bornes)
- 2 = 🔴 Critique (0 vélo OU 0 borne)
```

**Labellisation** : Utiliser données historiques `fct_station_snapshot`, observer état réel à t+30min.

### 2.3 Données d'entrée (Input Data)
**Disponibles au moment t** :
- État actuel station (nb vélos, nb bornes)
- État stations voisines (rayon 500m)
- Météo actuelle (température, pluie, vent)
- Temporel (heure, jour semaine, férié, grève)
- Historique agrégé (demande moy 7 derniers jours)

**⚠️ PAS disponible** : Données à t+10min, t+20min, t+30min (le futur !)

### 2.4 Features (Engineering)
**Exemples** :
- Temporelles : hour, day_of_week, is_weekend, is_rush_hour
- État station : bikes_available, occupancy_rate, turnover_1h
- Voisinage : avg_bikes_nearby_500m
- Historique : bikes_7d_same_hour_avg

**Nombre** : ~30-50 features

### 2.5 Contraintes performance
- **Latence** : < 200ms par prédiction (100 stations)
- **Throughput** : 10 prédictions/sec
- **Précision** : ≥ 75% (classe Critique)
- **Rappel** : ≥ 70% (ne pas louper trop de stations critiques)

### 2.6 Collecte & labellisation
- **Source** : GBFS historisé (déjà dispo)
- **Labellisation** : Automatique via SQL (LEAD function)
- **Volume** : ~2-3 millions lignes (6 mois historique)

### 2.7 Évaluation
- **Métrique** : Precision, Recall, F1-score (par classe)
- **Validation** : Temporal split (80% train, 20% test)
- **Baseline** : Moyenne mobile 7 jours → Accuracy ~68%
- **Objectif** : Accuracy ≥ 80% (gain +12 points)

### 2.8 Monitoring & Retraining
- **Monitoring** : Distribution prédictions, drift detection
- **Retraining** : Mensuel (ou si performance < 70%)
- **Alerting** : Email si accuracy < 75% pendant 3 jours

---

## 3. Choix de l'algorithme (MVM)

**Principe MVM (Minimum Viable Model)** : Commencer par le plus simple.

| Niveau | Approche | Accuracy estimée | Effort | Décision |
|--------|----------|------------------|--------|----------|
| 0 | Règle statique ("< 3 vélos = critique") | ~60% | 1 jour | ❌ Trop faible |
| 2 | Moyenne mobile 7j | ~68% | 2 jours | ❌ Pas assez |
| 3 | LightGBM (40 features) | ~80% | 4 semaines | ✅ **MVP** |
| 4 | LSTM (réseau neurones) | ~82% | 8 semaines | ❌ Gain marginal |

**Choix** : LightGBM (niveau 3) = meilleur compromis.

---

## Périmètre & Hors-périmètre

### Dans le périmètre
✅ Méthodologie conception produit ML
✅ Cahier des charges ML (8 composantes)
✅ Choix algorithme (MVM)

### Hors périmètre
❌ Implémentation du modèle (pas fait pour projet étudiant)
❌ Feature engineering détaillé
❌ Tuning hyperparamètres
❌ Déploiement en production (API REST)

---

## Limites & Risques

### Limites identifiées
1. **Pas de données réelles** : On n'a pas historisé GBFS → pas de dataset pour entraîner le modèle.
2. **Hypothèses non testées** : On suppose que 40 features suffisent, mais on n'a pas testé.
3. **Pas de validation users** : On n'a pas pu présenter ce workflow aux régulateurs (Julien).

### Risques
- **Overfitting** : Modèle trop ajusté aux données historiques → mauvaise généralisation.
- **Data leakage** : Si on utilise accidentellement des données du futur dans les features.
- **Adoption** : Même si le modèle est bon, il faut que les régulateurs l'utilisent.

---

## Impact & Next Steps

### Impact attendu
Si on implémentait ce produit ML :
- **Régulateurs** : Tournées optimisées, moins de déplacements inutiles
- **Opérateurs** : RE de 65% à 80% (+15 points)
- **Usagers** : Moins de stock-out/dock-out

### Next Steps
1. **Collecter données** : Historiser GBFS pendant 3-6 mois
2. **Feature engineering** : Créer les 30-50 features
3. **Entraînement** : Tester LightGBM vs autres algos
4. **Évaluation** : Validation temporelle (pas aléatoire !)
5. **Déploiement** : API REST + intégration app mobile

