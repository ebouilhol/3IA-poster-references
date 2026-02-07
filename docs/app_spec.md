# Spécification fonctionnelle — Application musculation Android

## Objectif
Créer une application Android de musculation permettant de charger des exercices depuis un JSON, d'éditer les paramètres d'entraînement, puis de calculer une progression hebdomadaire (ex. +2 %, +5 %, +10 %).

## Modèle de données (JSON)

### Schéma conceptuel
- **WorkoutPlan**
  - `name`: nom du programme
  - `progression`: paramètres de progression
  - `exercises`: liste d'exercices

- **Progression**
  - `mode`: `percentage`
  - `weeklyIncrease`: valeur en pourcentage (ex. 2, 5, 10)

- **Exercise**
  - `id`: identifiant unique
  - `name`: nom de l'exercice
  - `sets`: nombre de séries
  - `reps`: nombre de répétitions
  - `weightKg`: poids en kg
  - `restBetweenSetsSec`: repos entre séries (secondes)
  - `restBetweenExercisesSec`: repos entre exercices (secondes)

### Exemple JSON
Voir `data/sample_workout.json`.

## Fonctionnalités principales

### 1) Import JSON
- Charger un fichier JSON local.
- Valider le schéma et afficher une erreur claire si une clé est manquante.

### 2) Liste des exercices
- Afficher tous les exercices avec : nom, séries, répétitions, poids, repos.

### 3) Édition des exercices
- Permettre à l'utilisateur de modifier :
  - `sets`
  - `reps`
  - `weightKg`
- Conserver les temps de repos (facultatif si besoin d'édition).
- Sauvegarder les modifications localement (cache ou base locale).

### 4) Progression automatique
- Calculer un nouveau poids en fonction de la progression hebdomadaire.
- Formule (hebdomadaire) :
  - `newWeight = weightKg * (1 + weeklyIncrease/100)`
- Arrondir selon une règle configurable :
  - Exemple : au multiple de 0,5 kg le plus proche.

### 5) Historique
- Garder un historique des poids avant/après progression.
- L'historique par exercice inclut la date et l'augmentation appliquée.

## UI proposée (écrans)

1. **Accueil**
   - Bouton « Importer un plan JSON »
   - Liste des plans récents

2. **Plan d'entraînement**
   - Liste des exercices
   - Accès à l'édition de chaque exercice

3. **Édition d'exercice**
   - Champs : séries, répétitions, poids
   - Bouton « Sauvegarder »

4. **Progression**
   - Sélecteur de pourcentage (2/5/10 % ou valeur libre)
   - Bouton « Appliquer la progression »
   - Résumé des nouveaux poids

## Stockage
- Utiliser Room (SQLite) pour :
  - Plans d'entraînement
  - Exercices
  - Historique de progression

## Règles de validation
- `sets` >= 1
- `reps` >= 1
- `weightKg` >= 0
- `restBetweenSetsSec` >= 0
- `restBetweenExercisesSec` >= 0

## Notes techniques
- Architecture recommandée : MVVM
- Parsing JSON : Kotlinx Serialization ou Moshi
- UI : Jetpack Compose
