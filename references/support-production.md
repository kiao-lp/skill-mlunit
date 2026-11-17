# Réalisation des supports

Lire cette référence pour organiser les dossiers d’épisode, collecter les sources, préparer
les références d’épisode ou produire les supports. La skill `learning-module` définit la pédagogie,
les passes et la matière à rassembler ; cette référence couvre le stockage, les formats KIAO.

## Dossiers d’épisode

Nommer les dossiers d’épisode **`E01-titre-de-l-episode`** : `E`, un numéro sur deux chiffres,
un tiret, puis le titre en minuscules séparées par des tirets. Cette règle vaut pour la référence
et la livraison, dont les structures internes peuvent différer. Un épisode est une étape
d’apprentissage et peut utiliser plusieurs unités.

Si l’unité occupe directement le dossier d’épisode, son titre doit produire ce nom
(`E01-cell-division`). Les unités à l’intérieur gardent des titres courts, sans numéro.
Ne pas ajouter un dossier d’épisode imbriqué pour satisfaire cette convention.

```text
units/Cell division/                     # unité autonome
  08tp38iwk4dhazd0t7jefh.mlunit.json     # le document conserve son docId
  cadre.md
  introduction.ttst.md
units/Human biology/                     # série
  <indexDocId>.mlunit.json               # sommaire
  E01-cell-division/                     # livraison d’un épisode
    Course/                             # une unité de cet épisode
      09tp38iwk4dhazd0t7jefh.mlunit.json
      P1-Course.md
      P2-Course.md
```

Les préfixes d’épisode indiquent la séquence du module ; l’ordre de lecture reste celui des
`members` du sommaire. Maintenir leur cohérence lors d’une réorganisation. Pour la structure
ci-dessus, définir `folder` explicitement ; créer une unité avec `index` seul utilise par défaut
`<dossier du sommaire>/episodes/<titre>/`.

## Recherche, référence et livraison

| Élément | Emplacement |
|---|---|
| Spécification de recherche | `/corpora/<module>/Search.md` |
| Passages collectés | `/corpora/<module>/passages/` |
| Plan du module | `/course/<module>/Plan.md` |
| Référence d’un épisode | `/course/<module>/E01-titre-de-l-episode/` |
| Livraison d’un épisode | `<destination-du-module>/E01-titre-de-l-episode/`, selon la tâche |

Conserver les passages en Markdown avec titre, URL source et langue, éventuellement en tête
de fichier. Dater les études citées, notamment leur année de publication, et préserver
la traçabilité lors des copies vers les références d’épisode.

La référence contient toute la matière nécessaire à l’épisode, organisée **par sujet,
sans classement par passe ni calendrier**. Les passes d’entrée indiquent seulement le volume
et la nature à collecter. La référence est **immuable pendant la production des supports**
et ne prescrit pas la structure livrée : une autre méthode pédagogique doit pouvoir la réutiliser.

À destination, réutiliser les unités et sommaires existants ou les créer selon la procédure
courante de `SKILL.md` : préférer `kiao-units` et utiliser le mode sans MCP prévu sous condition
si un outil nécessaire est indisponible. Choisir la structure finale en fichiers et unités selon
la méthode et les supports demandés, sans devoir reproduire la référence. Les deux formats
évoluent indépendamment.

Dans la livraison de l’épisode, préfixer chaque fichier propre à une passe par **`P1-`, `P2-`,
`P3-`, …**. Rattacher chaque support à son épisode et à sa passe ; conserver les passes
antérieures lorsqu’on en ajoute une. Ces préfixes ne s’appliquent pas à la référence.
Lors d’une reprise, mettre les noms concernés en conformité et actualiser leurs liens.

## Repères et formats par support

Ces noms indicatifs illustrent la passe 1 ; adapter le préfixe à la passe demandée.
Ils ne définissent ni les titres d’unité ni les positions dans un sommaire.

Les durées sont indiquées pour la **passe 3** dans la colonne **Durée passe 3** : c’est la
durée de référence, à 100 %. La durée effective d’un support s’en déduit selon la passe :
**50 % en passe 1, 66 % en passe 2 et 100 % à partir de la passe 3**.

| Support | Exemple de fichier | Durée passe 3 | Repères de réalisation |
|---|---|---|---|
| Podcast de vocabulaire | `P1-Vocabulary.ttst.md` | — | Brèves définitions des termes et expressions. Pour l’anglais, profil de référence B2, surtout centré sur l’IT ; l’adapter au profil donné. |
| Podcast d’introduction | `P1-Introduction.ttst.md` | Environ 25 minutes, pas plus | Poser des questions ouvertes, puis donner intuitions et exemples, avant la règle générale ou la définition technique. |
| Questions ouvertes d’introduction | `P1-IntroQuestions.ttst.md` | Environ 25 minutes corrections comprises, pas plus | Développement oral bref, puis éléments d’une réponse solide. |
| Cours écrit | `P1-Course.md` | — | Accessible et progressif, exemples et petits exercices réguliers ; terminer par un résumé des acquis essentiels. |
| Cours audio | `P1-Course.ttst.md` | — | Complet et progressif, exemples concrets, petits exercices oraux à réponse courte, corrections et résumé final. |
| Exercices écrits | `P1-Exercises.md` | Environ 45 minutes de travail | Correction raisonnée de chaque exercice. |
| Questions ouvertes d’entraînement | `P1-TrainingQuestions.ttst.md` | Environ 30 minutes, corrections comprises, pas plus | Questions de cours et problèmes courts, suivis d’une correction. Certaines questions incluent des objectifs perlocutoires tels qu'assoir sa crédibilité, rassurer, vulgariser, convaincre, dédramatiser, alerter, faire accepter un compromis, aligner, obtenir une décision, faire adopter. Pour ces questions, corriger la stratégie de communication avant le contenu de la réponse. |
| Tutoriel | `P1-Tutorial.md` | 45 minutes, pas plus | — |

## Règles communes de production

- Pour une langue donnée, les supports d'une même passe d'un épisode sont complémentaires dans le cadre d'un plan d'apprentissage, à l'exception du cours audio et écrit, s’ils sont présents ensemble. Ces derniers sont alors interchangeables et non complémentaires, l’idée étant de pouvoir choisir la modalité selon la situation.
- Ne jamais répéter une question ou un exemple d’un support à l’autre. Produire un énoncé neuf à chaque fois : un énoncé déjà rencontré produit une reconnaissance, pas une seconde restitution.
- Réserver le fil narratif récurrent à l’enseignement. Pour l’entraînement, nommer des cas neufs : le cas du cours explique, des cas inédits mesurent.
- Ne jamais formuler une correction avec les mots exacts du cours. Reformuler force la comparaison sémantique ; recopier laisse croire à une réponse juste. Exception : l’objectif demande un apprentissage mot pour mot.
- Reproduire au moins une fois le format de réponse du contexte visé : examen, situation professionnelle, discussion.
- Tous les supports audio sont adaptés à l’écoute en marchant.
- À l’oral, la relecture étant impossible, respecter scrupuleusement les limites de la mémoire de travail humaine. Eviter de poser plusieures questions en une.
- Lorsqu’un apprenant signale un problème dans un tutoriel, corriger le support et lui fournir aussi de quoi poursuivre immédiatement.
- Limiter les explications générales sur le rôle, le fonctionnement ou l’utilisation du support à **une seule phrase courte**, à l’écrit comme à l’oral.
- Assure-toi que l’apprenant peut reconstruire chaque raisonnement avec les seuls prérequis annoncés et les informations du cours. Explicite toute nouvelle donnée, hypothèse, convention ou transition de contexte avant son utilisation.

## Conventions d’écriture

**Transcriptions audio (`.ttst.md`).** Texte non formaté optimisé pour la synthèse vocale,
sans mise en forme d’article ni prédécoupage nécessaire. Encadrer le **paragraphe entier**
de chaque question, y compris « Question n… », par `<mark>…</mark>` : le player s’arrête
à sa fin. Ne pas ajouter de silence SSML après une question marquée. Entre la correction et
la question suivante, insérer `<break time="2500ms"/>` pour l’assimilation. Ailleurs, exprimer
les silences en SSML, par exemple `<break time="10000ms"/>` pour dix secondes.

**Cours, exercices et tutoriels écrits.** Mettre en gras les expressions essentielles des
paragraphes pour la lecture rapide. Diagrammes PlantUML/Mermaid et blocs de code sont possibles ;
utiliser `<br>` pour les retours à la ligne dans une chaîne Mermaid. Placer chaque correction
**immédiatement après son exercice, dans un bloc commutable**, avec des liens vers les passages
exacts du cours construits selon `markdown-passage-link`.
Dans un QCM, aucune réponse en gras ; donner aux distracteurs une forme et une longueur
comparables pour que la présentation ou une réponse plus longue ne révèle pas la solution.

## Modules Azure

Seulement pour rechercher ou produire des supports Azure, lire
[Modules Azure](azure-modules.md) : actualité de la plateforme, cycle de vie des tutoriels,
valeurs régionales par défaut et exigences AZ-104.
