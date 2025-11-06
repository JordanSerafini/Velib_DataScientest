---
project: "Projet DPM – Vélib' Paris"
team: "Jordan S., Abdelmalek B."
formation: "RNCP 7 – Machine Learning & IA – DataScientest"
period: "Sept.–Nov. 2025"
role: "Data Product Manager (binôme)"
rewritten_for: "Soutenance orale – version étudiante plausible"
---

# 1.1 — Business Model Canvas — Vélib' Métropole

## Contexte & Objectifs

Dans le cadre de notre formation DPM chez DataScientest, on a choisi d'étudier le système Vélib' de Paris. L'idée : comprendre le business avant de plonger dans la data.

**Pourquoi commencer par le Business Model Canvas ?**
- Éviter de construire des modèles ML "dans le vide"
- Identifier où les données peuvent créer de la valeur concrète
- Comprendre qui sont les utilisateurs et leurs vrais problèmes

**Notre approche** : On a utilisé le framework BMC classique (9 blocs) + analyse de personas pour structurer notre compréhension.

**Contraintes** :
- Temps limité : ~3 semaines pour cette phase
- Pas d'accès aux données internes Smovengo
- Sources : docs publiques, API GBFS, rapports annuels, articles presse

---

## Vélib' en chiffres (ordres de grandeur)

D'après les rapports publics 2024 de Smovengo et les données IDFM :

- **~20 000 vélos** dont environ 40% électriques (VAE)
- **1 400-1 500 stations** sur la métropole parisienne
- **~470 000 abonnés actifs** (2024)
- **~49 millions de trajets/an**

**Limite** : Ces chiffres sont des ordres de grandeur issus de sources publiques. On n'a pas accès aux données détaillées internes.

---

## Segments clients — Pour qui ?

On a identifié 4 segments principaux :

### 1. Franciliens réguliers (commuters)
**Profil** : Trajets domicile-travail quotidiens
**Attentes** : Fiabilité, disponibilité aux heures de pointe
**Importance** : Majorité de l'usage en semaine, source de revenus stable

### 2. Usagers occasionnels & touristes
**Profil** : Visiteurs, usage ponctuel
**Attentes** : Simplicité d'inscription, messages clairs
**Importance** : Image du service, croissance potentielle

### 3. Entreprises & collectivités
**Profil** : Abonnements groupés pour employés
**Attentes** : Reporting, gestion simplifiée, image RSE

### 4. Autorités publiques
**Profil** : Métropole du Grand Paris, IDFM
**Attentes** : Qualité de service, respect des SLA contractuels
**Importance critique** : Vélib' fonctionne en délégation de service public → pénalités si objectifs non atteints

---

## Proposition de valeur — Qu'est-ce qui est apporté ?

**En résumé** : Une solution de mobilité douce 24/7, dense, flexible.

**Les 3 piliers** :
1. **Disponibilité** : Un vélo quand on en a besoin
2. **Flexibilité** : Pas de réservation, pas d'horaire
3. **Accessibilité & écologie** : Alternative crédible à la voiture

**Le problème** : Cette proposition ne tient que si la disponibilité est au rendez-vous. Si Alice arrive le matin et qu'il n'y a jamais de vélo → la proposition s'effondre.

→ **C'est là qu'on veut intervenir avec la data** : améliorer la disponibilité via prédiction + rééquilibrage optimisé.

---

## Canaux — Comment accéder au service ?

1. **Stations physiques** : Point d'accès principal
2. **App mobile & site web** : Consultation disponibilité temps réel, gestion abonnement
3. **Intégrations tierces** : Google Maps, Citymapper
4. **Support** : Téléphone, email, chat

**Opportunité data** : L'app est le canal idéal pour délivrer des prédictions et recommandations.

---

## Flux de revenus — Comment Vélib' gagne de l'argent ?

1. **Abonnements usagers** : Formules courte et longue durée + facturation à l'usage (dépassements)
2. **Subventions publiques** : Contrat d'exploitation avec collectivités (c'est un service public)
3. **Partenariats potentiels** : Monétisation données anonymisées (hypothétique, à vérifier RGPD)

**Limite** : On n'a pas accès au détail du modèle financier (chiffre d'affaires, marges). On travaille avec les infos publiques.

---

## Ressources clés

1. **Flotte de vélos** : ~20k vélos à maintenir en état
2. **Infrastructure** : 1400-1500 stations avec bornes connectées
3. **Systèmes IT** : API GBFS temps réel, plateforme de gestion interne
4. **Capital humain** : Équipes maintenance, régulation (travail de nuit), support, IT
5. **Données** : Flux temps réel GBFS, historiques d'usage (si loggés), tickets support

**Point clé** : Les données sont la ressource qui nous intéresse → elles permettent d'anticiper plutôt que réagir.

---

## Activités clés

1. **Maintenance** : Préventive et corrective (MTTR = Mean Time To Repair)
2. **Rééquilibrage** : Tournées camions jour/nuit pour redistribuer les vélos
3. **Supervision IT** : Monitoring temps réel, alerting
4. **Relation institutionnelle** : Coordination avec Métropole et IDFM
5. **Analyse data** : Exploitation données pour optimiser le service → **C'est notre terrain de jeu**

---

## Partenaires clés

1. **Autorités** : Métropole du Grand Paris, IDFM (donneurs d'ordre)
2. **Fournisseurs industriels** : Constructeurs vélos, bornes
3. **Prestataires IT** : Hébergeurs cloud, processeurs paiement, télécoms
4. **Partenaires mobilité** : Google Maps, Citymapper
5. **Logisticiens** : Rééquilibrage, transport

---

## Structure de coûts

**Principaux postes** :
1. **Maintenance** : Vélos + stations (usure, vandalisme)
2. **Logistique** : Camions, carburant, personnel terrain → **Coût élevé, variable à optimiser**
3. **Masse salariale** : Terrain, IT, support, management
4. **Infrastructure IT** : Cloud, télécoms, licences
5. **Amortissement** : Dépréciation vélos, stations, équipements
6. **Assurances & conformité** : RGPD, sécurité

**Insight** : Les coûts variables (logistique, maintenance) sont des leviers d'optimisation → réduire les km de tournées = économies directes.

---

## Données disponibles — Qu'est-ce qu'on a sous la main ?

### Source 1 : API GBFS Vélib' (temps réel)
**Contenu** : Disponibilité vélos (mécaniques/électriques) + bornes libres par station
**Format** : GBFS (standard international)
**Fréquence** : ~60 secondes
**Action requise** : Logger régulièrement pour créer un historique → **Sans ça, pas d'analyse temporelle possible**

### Source 2 : Données statiques stations
**Contenu** : GPS, capacité, nom, commune
**Sources** : Open Data Paris, IDFM

### Source 3 : Indicateurs de marché
**Sources** : Rapports publics Smovengo, Vélib' Métropole
**Utilité** : Cadrage global, validation ordres de grandeur

### Source 4 : Remontées terrain
**Sources** : Verbatims utilisateurs (entretiens), avis Google Maps
**Utilité** : Justification qualitative

**Limite** : On n'a PAS accès aux :
- Logs internes Smovengo (maintenance, tournées)
- Données de fréquentation détaillées par utilisateur (RGPD)
- Historique complet avant notre projet (on devra logger nous-mêmes)

---

## Personas — Qui sont les utilisateurs ?

On a créé 3 personas archétypes pour garder les vrais utilisateurs en tête.

### Persona 1 : Alice, 32 ans — Navetteuse quotidienne

**Profil** : Habite 11e, travaille 2e, Vélib' tous les jours (8h15 départ, 18h30 retour)

**Pain points** :
- **Matin** : Station vide (stock-out) → retards, stress
- **Soir** : Station pleine (dock-out) → détours, sentiment d'injustice (facturation continue)
- **VAE rarement dispo** aux heures de pointe

**Besoins** :
- Fiabilité
- Prédictions : "Y aura-t-il un vélo dans 15 min ?"
- Alternatives : Suggestions stations proches

**Pourquoi importante ?** : Alice représente les commuters réguliers = segment prioritaire (majorité usage semaine).

---

### Persona 2 : Marco, 27 ans — Touriste occasionnel

**Profil** : Italien, visite Paris pour un week-end, 2-3 trajets

**Pain points** :
- Tarifs et inscription peu clairs
- Messages d'erreur pas explicites (vélo bloqué, que faire ?)
- Station pleine à destination (Tour Eiffel) → stress, surfacturation

**Besoins** :
- Onboarding clair
- Messages prescriptifs ("Voici les 3 étapes...")
- Assistance multilingue

**Pourquoi important ?** : Mauvaise expérience touristique = mauvais avis en ligne → impact image service.

---

### Persona 3 : Julien, 41 ans — Logisticien de régulation (Smovengo)

**Profil** : Responsable régulation, planifie tournées camions (travail de nuit)

**Pain points** :
- Difficile de prioriser les tournées (on fait au feeling)
- Pas de visibilité sur pannes bloquantes (bornes HS découvertes sur place)
- Contraintes trafic et horaires (finir avant 6h)

**Besoins** :
- Score de criticité prédictif par station
- Optimisation de tournées (VRP - Vehicle Routing Problem)
- Prévisions de demande par créneau

**Pourquoi important ?** : Julien est l'utilisateur interne des outils d'optimisation qu'on pourrait concevoir.

---

## Les 3 problèmes identifiés (Discovery)

### Problème 1 : Stations vides/pleines aux heures de pointe
**Phénomènes** : Stock-out (0 vélo) et dock-out (0 borne libre) fréquents (8h-10h, 18h-20h)
**Impact** : Frustration usagers, détours, perte de temps, désabonnements

### Problème 2 : Régulation réactive et coûteuse
**Constat** : Rééquilibrage principalement réactif (pas prédictif)
**Impact** : Coûts logistiques élevés (carburant, personnel nuit), efficacité variable

### Problème 3 : UX dégradée en cas d'anomalie
**Constat** : Messages d'erreur génériques, pas prescriptifs
**Impact** : Hausse tickets support, NPS bas, abandons de session

**Priorité** : On a décidé de se concentrer sur le Problème 1 pour le projet (criticité + faisabilité).

---

## KPIs — Les 5 indicateurs de succès

Pour mesurer l'impact avant/après, on a défini 5 KPIs :

### KPI 1 : Stock-Out Rate (SOR)
**Définition** : % du temps avec 0 vélo disponible
**Formule** : `SOR = (minutes avec 0 vélo / minutes totales) × 100`
**Objectif** : -20% sur Top 100 stations

### KPI 2 : Dock-Out Rate (DOR)
**Définition** : % du temps avec 0 borne libre
**Objectif** : -20% sur Top 100 stations

### KPI 3 : Journey Abandon Rate (JAR)
**Définition** : % sessions où l'utilisateur renonce à prendre un vélo
**Objectif** : -10 à -15%
**Limite** : Nécessite logs applicatifs (on n'y a pas accès → proxy à définir)

### KPI 4 : Rebalancing Efficiency (RE)
**Définition** : Efficacité des tournées de rééquilibrage
**Formule** : `RE = (stations sorties criticité / stations ciblées) × 100`
**Objectif** : +10 à +15 points
**Limite** : Nécessite logs opérationnels tournées (on n'y a pas accès)

### KPI 5 : MTTR (Mean Time To Repair)
**Définition** : Temps moyen de retour en service après panne
**Objectif** : -10 à -15%
**Limite** : Nécessite logs maintenance (on n'y a pas accès)

**Réalisme** : KPI 1 et 2 sont calculables avec GBFS (on peut le faire). KPI 3, 4, 5 nécessitent des données internes qu'on n'a pas → **on se focalisera sur SOR et DOR pour le projet**.

---

## Benchmark — Inspirations (avec prudence)

On a regardé ce qui se fait ailleurs :

### Citi Bike (New York)
- Modèles prédictifs de demande (papiers académiques Cornell/Princeton, KDD'16)
- Gains opérationnels reportés : -20 à -25% coûts logistiques

### Santander Cycles (Londres)
- API temps réel ouverte (TfL Unified API)
- Nombreuses études académiques sur prédiction/rééquilibrage (2024)

### Bicing (Barcelone)
- Articles récents (2024-2025) sur maintenance prédictive, rebalancing data-driven
- Intégration données météo et événementielles

**Enseignement** : Les approches data-driven sont **faisables** et ont un impact mesurable (20-30% réduction criticité, 15-25% réduction coûts).

**Limite** : Ces chiffres viennent d'articles académiques, on ne sait pas toujours si c'est en production réelle ou en conditions contrôlées. À prendre avec recul.

---

## Périmètre & Hors-périmètre

### Dans le périmètre de notre projet étudiant
✅ Analyse du Business Model Canvas
✅ Définition des personas et problèmes
✅ KPIs SOR et DOR (calculables avec GBFS)
✅ Priorisation des améliorations
✅ Conception méthodologique (ML, BI)

### Hors périmètre
❌ Accès aux données internes Smovengo (logs maintenance, tournées, tickets support)
❌ Déploiement en production (on fait de la conception, pas de l'implémentation)
❌ Validation terrain avec équipes opérationnelles
❌ A/B testing avant/après (nécessiterait plusieurs mois + accès prod)

---

## Limites & Risques

### Limites identifiées
1. **Données** : On travaille uniquement avec GBFS (données publiques). Pas d'accès aux logs internes → vision partielle.
2. **Temps** : 8 semaines pour tout le projet DPM (Discovery + Conception) → pas le temps d'implémenter.
3. **Validation** : On n'a pas pu interviewer d'équipes Smovengo → personas et problèmes basés sur sources secondaires (articles, avis, forums).
4. **Métriques** : Certains KPIs (JAR, RE, MTTR) ne sont pas mesurables sans accès interne → on se concentre sur SOR/DOR.

### Risques
- **Biais de désirabilité** : Nos personas sont hypothétiques, pas validés avec vrais utilisateurs.
- **Données incomplètes** : Si on loupe des variables importantes (météo, événements, grèves), les prédictions seront moins bonnes.
- **Adoption** : Même si on conçoit un bon système, il faut que les équipes terrain l'utilisent → enjeu d'UX et de conduite du changement (hors scope projet).

---

## Impact & Next Steps

### Impact attendu (hypothèse)
Si on arrivait à implémenter nos recommandations :
- **Usagers** : Moins de frustration, moins de détours, meilleure expérience
- **Opérateurs** : Coûts logistiques réduits, efficacité améliorée
- **Service public** : Image améliorée, conformité SLA

**ROI estimé** : D'après benchmarks internationaux, gains de 15-25% sur coûts logistiques possibles.
**Limite** : On n'a pas fait de calcul ROI détaillé (nécessiterait coûts réels internes).

### Next Steps (séquentiels)
1. **Section 1.2.1** : Démarche qualitative (experience maps détaillées)
2. **Section 1.2.2** : Analyses quantitatives (calcul SOR/DOR avec GBFS si on a le temps de logger)
3. **Section 1.3** : Priorisation des améliorations (Impact/Effort)
4. **Section 1.4** : Feuille de route produit (roadmap 12 mois)
5. **DPM-2** : Conception ML et BI

---

## Hypothèses à valider

**Hypothèse 1** : Les stock-out/dock-out sont le problème #1 pour les usagers réguliers.
**Validation** : Entretiens qualitatifs (section 1.2.1) + analyse quantitative GBFS (section 1.2.2)

**Hypothèse 2** : Un modèle prédictif de demande peut améliorer le rééquilibrage.
**Validation** : Littérature académique (benchmarks internationaux) + faisabilité technique (section 2.1.1)

**Hypothèse 3** : Les équipes terrain utiliseraient des outils de prédiction/optimisation.
**Validation** : À faire avec interviews équipes Smovengo (pas possible dans notre projet)

---

## Sources & Références

**Docs officielles** :
- Rapports publics Smovengo 2024
- Données IDFM (Île-de-France Mobilités)
- Open Data Paris

**Académique** :
- Articles KDD'16 (Citi Bike NYC)
- Études TfL London (Santander Cycles)
- Publications récentes Bicing Barcelone (2024-2025)

**Cours DataScientest** :
- Module DPM : Business Model Canvas, Discovery, KPIs
- Framework RICE/Impact-Effort

**Web** :
- Avis Google Maps des stations Vélib'
- Articles presse sur conditions de travail (Travail & Sécurité)

