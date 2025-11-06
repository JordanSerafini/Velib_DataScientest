---
project: "Projet DPM – Vélib' Paris"
team: "Jordan S., Abdelmalek B."
formation: "RNCP 7 – Machine Learning & IA – DataScientest"
period: "Sept.–Nov. 2025"
role: "Data Product Manager (binôme)"
rewritten_for: "Soutenance orale – version étudiante plausible"
---

# 1.2.1 — Démarche qualitative de Discovery

## Contexte & Objectifs

**Objectif** : Comprendre finement les **pain points** des utilisateurs (usagers Vélib' + équipes internes) AVANT de parler solution.

**Pourquoi ?** Pour éviter de construire un dashboard ou un modèle que personne n'utilisera.

**Notre approche** :
1. Capitaliser sur les retours existants (avis, forums, presse)
2. Mener des entretiens utilisateurs (si possible)
3. Créer des Experience Maps pour visualiser les parcours

**Contrainte** : Temps limité (~2 semaines pour cette phase) + difficulté à recruter des usagers Vélib' pour interviews.

---

## A) Capitaliser sur les retours existants (Semaine 1)

Avant de faire des entretiens, on commence par **lire ce qui existe déjà**. Les gens parlent déjà de leurs problèmes, il suffit d'écouter.

### Sources utilisées

**Avis publics** :
- **Google Maps** des stations "phares" : Gare du Nord, République, Bastille
- **Réseaux sociaux** : Twitter/X, Reddit, groupes Facebook vélotaf
- **Forums** : Discussions mobilité urbaine (Libération, 20 Minutes)

**Remontées organisées** :
- **Presse & comités d'usagers** : Articles sur pannes, stations pleines/vides
  - Source : agemob.org (association usagers)

**Sources officielles** :
- **Pages Smovengo/Vélib'** : Chiffres clés, organisation régulation
  - Source : velib-metropole.fr

### Méthode : Compiler dans un tableur

On a structuré les retours dans un tableur avec ces colonnes :

| Date | Source | Station | Persona | Thème | Sous-thème | Verbatim | Sévérité (1-5) | Fréquence |
|------|--------|---------|---------|-------|------------|----------|----------------|-----------|

**Exemple de ligne** :
```
2024-10-15 | Google Maps | République | Commuter | Disponibilité | stock-out | "Jamais de vélo le matin, j'ai abandonné mon abonnement" | 5 | Haute
```

### Système de tags (Codebook)

Pour analyser, on a taggé chaque verbatim :

**Disponibilité** :
- `stock-out` : 0 vélo disponible
- `dock-out` : 0 place libre

**Qualité matérielle** :
- `panne_VAE` : vélo électrique défectueux
- `blocage_borne` : borne inutilisable

**Expérience app** :
- `infos_insuffisantes` : messages pas clairs
- `réclamation` : problème facturation/service

**Opérations** :
- `régulation_priorisation` : difficulté priorisation interventions
- `tournee_non_optimale` : inefficacité tournées

### Analyse

1. **Regrouper par tag** : Combien de fois revient `stock-out` ? `panne_VAE` ?
2. **Calculer Fréquence & Sévérité moyenne**
3. **Établir le TOP 5 des irritants**

**Résultat** : Cette analyse donne des **hypothèses** à valider ensuite.

**Limite** : Biais de sélection (les gens mécontents sont plus vocaux sur Google Maps). On ne capte pas les usagers satisfaits.

---

## B) Entretiens utilisateurs (Semaine 1-2)

### 1) Combien d'entretiens ?

**Objectif initial** : 6-8 usagers
- 4 **commuters** (trajets domicile-travail quotidiens)
- 2 **occasionnels/touristes**

**Objectif internes** : 3-4 opérationnels (régulation, maintenance, chef de secteur)

**Réalité** : On n'a pas réussi à organiser d'entretiens avec équipes Smovengo (pas de contact établi). Pour les usagers finaux, on a pu faire 3-4 entretiens informels dans notre réseau (amis, collègues qui utilisent Vélib').

**Limite** : Échantillon petit et non représentatif → On compense avec l'analyse des avis publics.

### 2) Guide d'entretien semi-directif (30-40 min)

Voici le guide qu'on a préparé (inspiré des cours DataScientest sur les entretiens utilisateurs) :

#### Warm-up (2-3 min)
- Présentation : "Bonjour, je suis [nom], étudiant à DataScientest. Je travaille sur l'amélioration du service Vélib'."
- **Consentement** : "Accepteriez-vous que j'enregistre pour ne rien oublier ? Uniquement pour ce projet."
- **Cadre** : "Le but est de comprendre l'expérience, pas de tester une solution. Pas de bonne ou mauvaise réponse."

#### Contexte d'usage (5-7 min)
- "Racontez-moi votre dernier trajet Vélib', de A à Z."
- "À quelle fréquence utilisez-vous Vélib' ? À quels horaires ?"
- "Quelles sont vos 2-3 stations habituelles ?"

#### Pain points (10-12 min)
- "À quels moments ça se passe le moins bien ? Pourquoi ?"
- "Quand la station est vide ou pleine, que faites-vous ?"
- "Qu'est-ce qui vous frustre le plus : disponibilité, pannes, app, facturation ?"

**Astuce** : Laisser des silences. Les gens ont besoin de réfléchir.

#### Profondeur — Technique des 5 pourquoi (8-10 min)
- "Pourquoi est-ce un problème pour vous ?"
- "Et pourquoi cela vous impacte autant ?"
- *(Répéter jusqu'à la cause racine, max 5 fois)*

**Exemple** :
- "Je n'ai pas de vélo le matin" → Pourquoi c'est un problème ?
- "Parce que j'arrive en retard au bureau" → Et pourquoi c'est grave ?
- "Parce que j'ai une réunion importante" → Pourquoi pas le métro ?
- "Parce que le métro est bondé et stressant" → Ah ! Vélib' = solution anti-stress.

#### Idées & attentes (5-7 min)
- "Si vous aviez une baguette magique, que changeriez-vous ?"
- "Quelles infos en amont vous aideraient à éviter la galère ?"

#### Clôture (2 min)
- "Ai-je oublié quelque chose d'important ?"

### 3) Bonnes pratiques

✓ **Questions ouvertes**, pas de solutioning
✓ **Laisser des silences**, reformuler, creuser les contradictions
✓ **Notes + enregistrement** (avec autorisation), pseudo-anonymisation
✓ **À chaud → tagger** avec le codebook

---

## C) Experience Maps (après entretiens)

On a créé des Experience Maps pour visualiser les parcours. Format simple :

| Élément | Description |
|---------|-------------|
| **Qui** | Persona (Alice, Marco, Julien) |
| **Étapes** | Planif → Prise → Trajet → Dépôt |
| **Objectif** | Par étape |
| **Touchpoints** | Appli, borne, vélo |
| **Émotions** | 😊 → 😐 → 😡 |
| **Pain points** | Points de friction |
| **Workarounds** | Contournements |
| **Opportunités** | Pistes d'amélioration |

### Exemple : Alice (commute matin)

**Étape 1 : Planif (8h10)** → Consulte app
- **Problème** : Info temps réel mais pas prédictive
- **Émotion** : 😰 Stress

**Étape 2 : Prise (8h20)** → File si 1 seul vélo
- **Problème** : Pas de choix (VAE indispo)
- **Émotion** : 😤 Frustration

**Étape 3 : Dépôt (8h40)** → Station pleine → détour 7 min
- **Problème** : Perd du temps, risque retard
- **Émotion** : 😡 Colère

**Opportunités** :
- Probabilité dispo à H+15 ("85% de chances d'avoir un vélo dans 15 min")
- Reco station alternative + bonus
- Message app prescriptif

**Lien avec les données** : Le flux GBFS permet d'objectiver stock-out/dock-out (si on historise).

---

## D) Restitution et synthèse

### Livrables

**1. Tableur de verbatims taggés**
→ TOP irritants : **disponibilité**, **pannes/blocages**, **info/app**

**2. 3 Experience Maps** (Alice, Marco, Julien)
→ Format PDF/PNG, à présenter en soutenance

**3. Problèmes formulés selon framework Discovery**
- **What** / **Who** / **Workaround** / **Why** / **How much** / **Value**

### Documents produits

- Guide d'entretien
- Formulaires de consentement (RGPD)
- Notes détaillées + verbatims pseudo-anonymisés
- Tableau de tags (Fréquence × Sévérité)
- 3 Experience Maps (PDF/PNG)
- Synthèse Discovery (1 page max)

---

## Périmètre & Hors-périmètre

### Dans le périmètre
✅ Analyse avis publics (Google Maps, forums, presse)
✅ Entretiens informels (3-4 usagers dans notre réseau)
✅ Création de personas et experience maps

### Hors périmètre
❌ Entretiens équipes Smovengo (pas de contact)
❌ Échantillon représentatif d'usagers (contrainte temps/recrutement)
❌ Observations terrain (shadowing des équipes régulation)

---

## Limites & Risques

### Limites identifiées
1. **Échantillon petit** : 3-4 entretiens seulement → pas statistiquement représentatif
2. **Pas d'accès aux opérationnels** : Personas internes (Julien) basées sur sources secondaires (articles, rapports) → pas validées avec vrais régulateurs
3. **Biais réseau** : Les personnes interviewées sont dans notre réseau → potentiellement similaires (CSP+, tech-savvy)
4. **Temps limité** : 2 semaines pour cette phase → pas pu creuser tous les segments (seniors, personnes à mobilité réduite)

### Risques
- **Confirmation bias** : On cherche peut-être à confirmer nos hypothèses initiales au lieu de les challenger.
- **Désirabilité** : Les personnes interviewées peuvent dire ce qu'elles pensent qu'on veut entendre.
- **Non-représentativité** : Les pain points identifiés peuvent ne pas être généralisables à toute la population Vélib'.

---

## Impact & Next Steps

### Impact attendu
Si on réussit notre Discovery :
- **Problèmes priorisés** : On saura sur quoi se concentrer (disponibilité > pannes > info)
- **Hypothèses à tester** : On aura des hypothèses claires à valider avec du quantitatif (section 1.2.2)
- **Personas crédibles** : On pourra présenter des personas qui sonnent vrais (même si hypothétiques)

### Next Steps
1. **Section 1.2.2** : Valider quantitativement (calcul SOR/DOR avec GBFS)
2. **Section 1.3** : Prioriser les améliorations (Impact/Effort)

---

## Hypothèses à valider (transition vers quantitatif)

**H1** : Les stock-out/dock-out sont fréquents aux heures de pointe
**Validation** : Calcul SOR/DOR par créneau avec données GBFS (1.2.2)

**H2** : Les commuters sont plus impactés que les occasionnels
**Validation** : Analyse par zone (résidentiel vs bureaux)

**H3** : Les VAE sont sur-sollicités et manquent plus souvent
**Validation** : Analyse du mix VAE/mécaniques par station et créneau

---

## Sources & Références

**Méthodologie** :
- Cours DataScientest DPM : Entretiens utilisateurs, Experience Maps
- Framework Discovery (What/Who/Why/How much)

**Données terrain** :
- agemob.org (association usagers)
- velib-metropole.fr (chiffres officiels)
- travail-et-securite.fr (conditions de travail régulation)
- Google Maps (avis stations)

