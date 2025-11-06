---
project: "Projet DPM – Vélib' Paris"
team: "Jordan S., Abdelmalek B."
formation: "RNCP 7 – Machine Learning & IA – DataScientest"
period: "Sept.–Nov. 2025"
role: "Data Product Manager (binôme)"
rewritten_for: "Soutenance orale – version étudiante plausible"
---

# 1.2 — Discovery — Introduction

## Contexte & Objectifs

**Objectif de la Discovery** : Comprendre finement les besoins réels des utilisateurs AVANT de parler solution (ML, dashboard, etc.).

**Pourquoi ?** Éviter de développer un modèle ou un outil que personne n'utilisera. On part du problème, pas de la techno.

**Notre démarche** :
1. Discovery qualitative (section 1.2.1) → Identifier les problèmes
2. Discovery quantitative (section 1.2.2) → Mesurer l'ampleur
3. Priorisation (section 1.3.1) → Décider par quoi commencer

---

## Vélib' en bref (rappel)

- **~20 000 vélos** dont ~40% VAE (Vélos à Assistance Électrique)
- **1 400-1 500 stations** sur la métropole parisienne
- **~470 000 abonnés** actifs (2024)
- **~49 millions de trajets/an**

**Atout data** : API GBFS (temps réel) → dispo vélos méca/élec et bornes libres par station (~1 min de refresh)

**Sources** : Rapports publics Smovengo, transport.data.gouv.fr, velib-metropole.fr

---

## Ce qu'on sait déjà (sources secondaires)

**Presse & comités d'usagers** : Irritants récurrents remontés
- Vélos bloqués en station
- Pannes sur VAE (batterie vide, assistance HS)
- Parcours de réclamation long et peu clair

**Équipes Smovengo** : Contraintes opérationnelles connues
- Régulation (rééquilibrage) organisée de nuit
- Difficultés de priorisation (quelle station traiter en priorité ?)
- Coûts logistiques élevés (camions, personnel de nuit)

→ **Hypothèse** : Une approche data pourrait aider (prévision demande, optimisation tournées, messages app contextualisés).

---

## Public cible (utilisateurs)

### Externes (usagers finaux)
- **Commuters franciliens** : Trajets domicile-travail (8h-9h, 18h-19h)
- **Occasionnels/touristes** : Usage ponctuel, moins familiers

### Internes (équipes opérationnelles)
- Équipes **régulation** : Planification tournées
- Équipes **maintenance** : Réparation vélos/stations
- Équipes **opérations** : Supervision globale

**Point clé** : Besoins différents entre ces deux publics.
- Usagers finaux → fiabilité, clarté, prédictivité
- Opérationnels → priorisation, productivité, réduction coûts

---

## Problématiques candidates (pain points)

On a identifié 3 grandes catégories de problèmes à creuser :

### 1. Stations critiques aux heures de pointe
**Phénomènes** :
- **Stock-out** : 0 vélo disponible → usager ne peut pas partir
- **Dock-out** : 0 place libre → usager ne peut pas reposer son vélo

**Conséquences** : Détours, stress, perte de temps, frustration

### 2. Qualité perçue & pannes
**Problèmes** :
- Blocages en station (bornes HS)
- VAE défaillants (batterie, assistance)
- Parcours de réclamation long

### 3. Info en situation d'anomalie
**Problème** : Messages dans l'app pas assez prescriptifs. L'usager ne sait pas :
- Quoi faire (réessayer ? changer de station ?)
- Où aller (quelle station alternative ?)
- Quel délai attendre (temporaire ou durable ?)

---

## Tableau de Discovery (synthèse)

| Problème | Qui | Contournement actuel | Pourquoi critique | Mesure possible | Valeur si résolu |
|----------|-----|---------------------|------------------|-----------------|------------------|
| **Stations vides/pleines (stock-out/dock-out)** | Usagers quotidiens ; Équipe régulation | Usagers : essayent plusieurs stations ; Ops : tournées réactives | Perte temps, frustration, surcoûts logistiques | Via GBFS : % temps à 0 vélo/place | +Satisfaction, −détours, −coûts camions |
| **Régulation pas assez prédictive** | Équipe régulation | Heuristiques locales, expérience terrain | Manque d'optimisation globale, coûts & fatigue (nuit) | Indicateurs internes (si dispo) : succès tournée | Gains opératoires, réduction temps critique |
| **Pannes/Blocages & réclamations** | Tous usagers | Relancer trajet, contacter support | Friction forte, sentiment d'injustice (facturation), baisse ré-usage | Taux incidents, temps résolution | −Tickets support, +clarté, +rétention |

**Sources** : velib-metropole.fr, travail-et-securite.fr, agemob.org

---

## Périmètre & Hors-périmètre

### Dans le périmètre
✅ Discovery qualitative : entretiens utilisateurs (limités, voir 1.2.1)
✅ Discovery quantitative : analyses sur données GBFS publiques (voir 1.2.2)
✅ Priorisation des problèmes (Impact/Effort)

### Hors périmètre
❌ Accès aux logs internes Smovengo (tournées, maintenance, tickets)
❌ Entretiens avec équipes Smovengo (pas de contact établi)
❌ Déploiement en production
❌ A/B testing avant/après

---

## Limites & Risques

### Limites identifiées
1. **Accès données** : On travaille uniquement avec GBFS (public). Pas de logs internes → vision partielle.
2. **Entretiens** : Difficile de recruter des usagers Vélib' pour interviews (contrainte temps/réseau). On va compenser avec sources secondaires (avis Google Maps, forums, presse).
3. **Validation interne** : Pas de contacts chez Smovengo → hypothèses sur les ops non validées avec les vrais opérationnels.

### Risques
- **Biais confirmation** : On peut être tenté de confirmer nos hypothèses au lieu de les challenger.
- **Données incomplètes** : Si on loupe des variables importantes (événements, grèves, météo), les analyses seront biaisées.
- **Représentativité** : Si on n'arrive pas à interviewer assez d'usagers, les personas seront hypothétiques.

---

## Démarche : Problems Discovery → Solution Discovery

Dans le cadre du Data Product Management (DPM), la démarche se fait en 2 temps :

1. **Problems Discovery** (phase actuelle) : Identifier et problématiser les vrais problèmes
2. **Solution Discovery** (phase ultérieure - DPM-2) : Cadrer le MVP (Minimum Viable Product)

**L'idée** : Ne pas partir d'une solution ("faire un modèle de prédiction") mais d'un problème ("les usagers n'ont pas de vélo le matin").

---

## Next Steps

Les prochaines étapes :

1. **Section 1.2.1** : Démarche qualitative (entretiens, experience maps)
2. **Section 1.2.2** : Analyses quantitatives (calcul métriques, requêtes SQL)
3. **Section 1.3** : Priorisation des améliorations et définition KPIs

**Objectif final** : Avoir une vision claire du problème à résoudre, validée par des données qualitatives ET quantitatives, avant de se lancer dans la conception d'une solution.

---

## Hypothèses à valider

**H1** : Les stock-out/dock-out sont le problème #1 pour les usagers réguliers
**Validation** : Entretiens qualitatifs (1.2.1) + analyses GBFS (1.2.2)

**H2** : La régulation actuelle n'est pas assez prédictive
**Validation** : Analyses efficacité tournées (1.2.2) + benchmarks internationaux

**H3** : L'UX en cas d'anomalie est insuffisante
**Validation** : Entretiens usagers + analyse avis Google Maps

