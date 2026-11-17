---
name: mlunit
description: "Créer ou modifier des unités KIAO, des sommaires et leurs supports écrits ou audio (.mlunit.json, .ttst.md). Lire avant de créer ou renommer une unité : son titre nomme son dossier et le modifier renomme celui-ci."
---

# Unités KIAO

Une **unité** est un dossier du store contenant `<docId>.mlunit.json` et une chaîne ordonnée
d’articles, podcasts et ensembles d’exercices, chacun dans son fichier. Articles et podcasts
utilisent l’unique langue de l’unité ; les exercices ont leur propre paire de langues.
Un **sommaire** est une unité dont les `members` référencent les documents d’autres unités
par chemin, les rendant accessibles aux lecteurs.

Un **épisode** est une étape d’apprentissage ; il peut utiliser plusieurs unités et fichiers
de support. La skill `learning-module` définit la pédagogie, l’ordre des épisodes et les passes.

Lors de la génération d'une unité, il ne faut pas prendre modèle sur ces voisinnes, car la méthode pédagogique est sujette au changement entre épisodes.

## Procédure courante

1. Rechercher les outils MCP `kiao-units` disponibles. Utiliser en priorité `unit_create`,
   `unit_update` et `unit_index_*` pour créer ou modifier les unités et sommaires : leurs schémas
   décrivent les champs acceptés. Avant une modification, lire l’unité existante et les
   identifiants de ses contenus (`unit_list`, `unit_get_contents` et les lectures ciblées utiles).
2. Corriger les erreurs de saisie d’après le schéma de l’outil. **Seulement après avoir constaté
   qu’un outil nécessaire est absent, ou qu’un appel révèle son défaut de fonctionnement**, lire
   [Mode sans MCP](references/file-fallback.md) et réaliser le même travail avec les accès aux
   fichiers disponibles. Ne pas précharger cette référence ni ses schémas lors de l’usage normal
   du MCP. Continuer d’utiliser les outils d’unité fonctionnels pour les opérations qu’ils couvrent.
3. Réutiliser les unités et sommaires existants lorsque c’est pertinent. L’ordre de lecture est
   celui des `members` du sommaire ; `unit_index_add` insère à la position choisie. Une unité peut
   figurer dans plusieurs sommaires, chacun avec son propre ordre.

## Titres et liens

Le titre nomme le dossier et apparaît dans les chemins, les liens et les listes du player.
Le modifier renomme le dossier ; lire la référence sur le renommage ci-dessous avant de le faire.

- Quelques mots nommant le sujet : `Cell division`, `Passive voice`, `TCP handshake`.
  Le périmètre et le bénéfice pour l’apprenant vont dans `description`, en deux phrases au plus.
  Un contexte tel que « troisième de cinq séances » y figure seulement s’il est utile.
- Unités ordinaires et sommaires **sans numéro** : éviter `S3 - …`, `Sequence 12 — …`, `Part 2: …`.
  Le sommaire porte le sujet de la série. Omettre les annonces de type (« Index de… », « Podcast : »)
  et la répétition du nom de série dans chaque titre d’unité.
- Pour les épisodes d’un module, lire la référence de réalisation ci-dessous **avant de choisir
  le titre ou le dossier** : elle définit la convention des dossiers d’épisode et son exception
  au titre sans numéro.
- Écrire les références depuis la racine du store **avec une barre oblique initiale**, par
  exemple `/units/Cell division/cadre.md`. Cela vaut pour `members`, `contents[].path` et les
  liens vers le store.

## À charger selon la tâche

- **Renommer ou déplacer une unité, résoudre une collision de dossiers ou examiner la conversion
  d’un titre :** lire [Renommage et chemins](references/retitling-and-paths.md).
- **Organiser les dossiers d’épisode, collecter les sources, préparer les références d’épisode,
  produire les supports ou appliquer leurs règles d’écriture :** lire
  [Réalisation des supports](references/support-production.md).
- Charger uniquement les références nécessaires à l’opération courante. Le mode sans MCP reste
  soumis au constat prévu dans la procédure courante.
