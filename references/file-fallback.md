---
status: APPROVED
role: PRIMARY_SOURCE
---

# Mode sans MCP

**Charger cette référence seulement après avoir constaté qu’un outil `kiao-units` nécessaire
est absent ou ne fonctionne pas.** Lors de l’usage normal du MCP, les outils fournissent
le schéma. Cette référence permet de réaliser les opérations équivalentes sur les fichiers
pour créer ou modifier les unités, leurs contenus et les sommaires. Continuer d’utiliser les
outils d’unité fonctionnels pour les opérations encore disponibles.

## Accès et reprise après échec

Utiliser les outils `files_*` disponibles ou l’accès direct au store cible. Résoudre les chemins
du store par rapport à sa véritable racine dans le système de fichiers ; un lien stocké
`/units/…` n’est pas un chemin absolu du système. Le format ci-dessous est autonome et ne
nécessite pas de disposer du code source de l’application.

Après l’échec d’une écriture, inspecter la destination et le sommaire concerné avant de réessayer.
Une expiration de délai peut survenir après des écritures réussies ou partielles. Réutiliser
le docId, les fichiers de contenu et les appartenances déjà créés ; terminer les étapes
manquantes au lieu de créer une deuxième unité. Corriger les saisies invalides d’après le schéma
de l’outil avant de traiter une erreur de saisie comme une panne.

Lire et analyser le JSON existant avant de le modifier. Préserver les champs sans rapport avec
la demande, les identifiants, l’ordre des contenus, les horodatages et les réglages annexes.
Sérialiser avec une bibliothèque JSON et vérifier les fichiers obtenus ; ne pas effectuer
de remplacement textuel global dans le store.

## Document d’unité

Un nouveau `<docId>.mlunit.json` utilise cette enveloppe et ce contenu, ici avec un article :

```json
{
  "docType": "Unit",
  "docTypeVersion": 21,
  "docId": "08tp38iwk4dhazd0t7jefh",
  "createdAt": "2026-11-02T10:00:00.000Z",
  "content": {
    "meta": {
      "contentUpdatedAt": "2026-11-02T10:00:00.000Z",
      "quality": "DRAFT"
    },
    "lang": "EN",
    "title": "Cell division",
    "tooltips": {},
    "contents": [
      {
        "id": "cadre",
        "type": "article",
        "label": "Introduction",
        "path": "/units/Cell division/cadre.md"
      }
    ]
  }
}
```

- Générer un **docId de 22 caractères alphanumériques minuscules** pour chaque nouveau document ;
  ne jamais réutiliser celui de l’exemple. `nanoid` avec l’alphabet
  `1234567890abcdefghijklmnopqrstuvwxyz` correspond à l’application. En Python,
  `secrets.choice` avec cet alphabet, répété 22 fois, convient aussi.
- À la création, utiliser l’instant UTC actuel au format ISO-8601 pour `createdAt` et
  `meta.contentUpdatedAt`. Conserver `createdAt` et actualiser `meta.contentUpdatedAt` à chaque
  modification d’unité, y compris de ses fichiers de contenu, de leur ordre ou de l’appartenance
  à un sommaire. Conserver `docId` lors des renommages et déplacements.
- `lang` est un code de langue en majuscules, par exemple `EN`, `FR`, `DE` ou `ZH`. Articles et
  podcasts utilisent cette langue ; celles des exercices sont indépendantes. La modifier
  ne traduit pas les contenus existants.
- `quality` vaut `DRAFT` par défaut, ou `NORMAL` ou `HIGH`. Ce champ décrit la finition éditoriale
  et classe les résultats de recherche ; même un brouillon est accessible si un sommaire le liste.
- `meta.domain` est facultatif et doit être un nom de domaine KIAO canonique, tel que
  `IT_NETWORKS`. Conserver la valeur existante ; pour une nouvelle valeur, utiliser un catalogue
  de domaines disponible et omettre le champ si aucune valeur valide n’est connue. Ne pas inventer
  de catégorie ni stocker son libellé localisé.
- Les champs textuels facultatifs sont `title`, `description` et `privateComment`. `tooltips`
  associe des termes à leurs explications ; fusionner les entrées demandées en conservant
  les autres clés.
- `contents` est requis, peut être vide et ordonne ensemble tous les types de contenu. Il n’y a
  ni article ni podcast par défaut. `members` est facultatif et contient les entrées du sommaire
  décrites plus bas.
- `updatedAt` est un horodatage facultatif de sauvegarde dans l’enveloppe, distinct de
  `meta.contentUpdatedAt`. Ne pas fabriquer `lastPublishedAt` : la véritable publication le renseigne.

Ces versions correspondent au modèle d’unité actuel. Si le code source est disponible, comparer
avec [le modèle d’unité canonique](/perso/exchange/dashboard-plugins/unit-edit/models/unit.ts)
et [le modèle MCP](/perso/reference/npm/kiao-perso-mcp/src/unit/model.ts) lorsque le format évolue ;
ne pas attribuer une version plus récente à un ancien contenu sans convertir sa structure.

## Créer ou modifier une unité

1. Réutiliser une unité existante adaptée ou générer son docId et choisir sa destination.
   Par défaut, utiliser `units/<titre>/` ; pour une création avec un sommaire, utiliser
   `<dossier du sommaire>/episodes/<titre>/` et ajouter le nouveau document à ce sommaire après
   sa sauvegarde. Un `folder` explicite remplace ces emplacements, mais ajouter l’appartenance
   lorsqu’elle est demandée. Pour la conversion du titre, les collisions et les déplacements,
   lire [Renommage et chemins](retitling-and-paths.md). Pour un épisode de module, appliquer
   la convention de dossier de la référence de réalisation déjà sélectionnée pour cette tâche.
2. Pour un élément existant, cibler son **identifiant de contenu** et suivre son `path` stocké.
   Pour un nouvel élément, choisir un identifiant utilisable comme nom de fichier et unique parmi
   tous les types de l’unité, puis ajouter à `contents` un élément avec `id`, `type`, `label`
   et `path`. Le type est `article`, `podcast` ou `exercises` ; le libellé prend par défaut
   la valeur du nouvel identifiant. Réutiliser l’identifiant existant pour modifier l’élément ;
   ne pas cibler implicitement le premier élément d’un type.
3. Écrire d’abord les fichiers de contenu et leurs réglages, puis le document d’unité complet,
   puis l’appartenance au sommaire. Ainsi, un sommaire ne référence pas un document qui n’est
   pas encore écrit. Modifier uniquement les valeurs demandées et l’horodatage du contenu de l’unité.
4. Pour réordonner les contenus, déplacer l’élément existant dans `contents` (les positions MCP
   commencent à 1). Modifier son `label` conserve son identifiant et son chemin. Un titre
   d’article ou de podcast destiné au lecteur qui remplace ce libellé se stocke dans son annexe.

## Fichiers de contenu et réglages

| Type | Fichier d’un nouvel élément | Réglages |
|---|---|---|
| `article` | `<id>.md` | `<id>.md.config.json` si nécessaire |
| `podcast` | `<id>.ttst.md` | `<id>.ttst.md.config.json` |
| `exercises` | `<id>.mlex.json` | Dans ce document |

`contents[].path` contient le chemin réel du fichier depuis la racine du store **avec une barre
oblique initiale**. Stocker le texte dans le fichier, pas dans le JSON d’unité. Lors d’une
modification, suivre les chemins déjà stockés ; ne pas les recalculer à partir d’un titre,
d’un docId ou d’un libellé. Pour rédiger des supports pédagogiques, lire
[Réalisation des supports](support-production.md) seulement si la tâche le nécessite.

Les annexes d’article sont des objets JSON simples avec les champs facultatifs `lang`, `title`
(titre destiné au lecteur) et `bibliographyUrls` (libellé → URL). Fusionner les bibliographies
sans perdre les entrées existantes. Les annexes de podcast sont des objets simples avec les
champs facultatifs `lang`, `title` (titre destiné au lecteur), `ttsVoice` et `ttsOverrides`
(terme → forme prononcée). Préfixer les voix par leur fournisseur, par exemple
`google:en-US-Chirp3-HD-Fenrir` ou `azure:en-US-JennyNeural`. Renseigner `lang` d’après l’unité
pour une nouvelle annexe de podcast. Lors d’un changement de voix, supprimer l’ancien `hash`
audio pour permettre sa régénération. Le `hash` audio et les intervalles de questions `questions`
sont des résultats de publication ; ne jamais les fabriquer. L’audio généré d’un script est
`<id>.mp3`, à côté de celui-ci, et ne constitue pas un élément de `contents`.

### Ensembles d’exercices, seulement si nécessaire

Un fichier d’exercices possède sa propre identité de document, indépendante de l’unité et de
l’identifiant de contenu :

```json
{
  "docType": "UnitExercises",
  "docTypeVersion": 4,
  "docId": "09tp38iwk4dhazd0t7jefh",
  "createdAt": "2026-11-02T10:00:00.000Z",
  "content": {
    "title": "Everyday conversation",
    "source": { "lang": "FR", "exercises": "" },
    "target": { "lang": "EN", "exercises": "" }
  }
}
```

Générer un nouveau docId et l’horodatage actuel de création. La source est la langue de consigne,
la cible est la langue apprise. Chaque côté requiert `lang` et la chaîne MLScript `exercises` ;
une chaîne vide constitue un brouillon inachevé. Les champs facultatifs propres à chaque côté
sont `ttsVoice`, `ttsOverrides`, `sttOverrides` (terme → transcription alternative) et
`translationInstructions`. Utiliser des voix préfixées par leur fournisseur pour la langue
de chaque côté ; lorsque celle-ci change, choisir une voix adaptée à la nouvelle langue.
Fusionner les réglages sans effacer les scripts ni les autres clés. Le `lastPublishedAt`
facultatif de l’ensemble relève de sa véritable construction audio.

Préserver l’alignement des lignes MLScript, y compris les lignes vides, les préfixes correspondants
et les lignes manuelles `~`/`~~`. `# ` introduit le titre audio ; `^ ` introduit un palier de rappel
avant le corps ou le titre qu’il prépare. Conserver les attributs finaux de traitement
` // {…}`. Utiliser `unit_mlscript_help` et `unit_validate_mlscript` s’ils sont disponibles lors
de la rédaction des scripts ; ces vérifications n’exigent pas d’utiliser les outils de modification
des unités.

## Opérations sur les sommaires

Un sommaire utilise la **même enveloppe `Unit`**, avec dans `content.members` les chemins ordonnés
des documents enfants `.mlunit.json`, par exemple :

```json
[
  "/units/Cell division/08tp38iwk4dhazd0t7jefh.mlunit.json"
]
```

- **Créer :** utiliser un nouveau docId, un titre de série court, sa langue, `contents: []` et
  `members: []` ou les enfants initiaux demandés, dédoublonnés en préservant leur ordre. Laisser
  les enfants existants à leur emplacement. Un sommaire imbriqué dans un autre s’ajoute par
  appartenance ; il n’a pas à être déplacé dans le dossier d’épisodes du parent.
- **Ajouter :** lire le sommaire actuel ; convertir la référence enfant en chemin de son document
  avec une barre oblique initiale. Si elle est déjà présente, laisser la liste inchangée.
  Sinon, l’ajouter à la fin ou l’insérer avant la position demandée, comptée à partir de 1 ;
  une position au-delà de la liste ajoute à la fin. Préserver toutes les autres appartenances.
  Résoudre un docId seul en recherchant son document réel, sans deviner son dossier à partir
  de cet identifiant.
- **Réordonner :** déplacer les chemins existants dans `members` ; conserver les identités et
  les emplacements des unités. Ajouter à nouveau un membre existant ne le déplace pas.
  Maintenir la cohérence des préfixes d’épisode si la séquence du module elle-même change.
- **Retirer :** retirer l’entrée stockée demandée si elle est présente. Conserver les fichiers
  enfants et les appartenances aux autres sommaires. `///deleted///` et les anciens docIds seuls
  sont des entrées périmées qui peuvent être retirées ; ne jamais les écrire comme nouvelles
  références.
- **Lire :** déduire les libellés des membres des titres actuels des enfants. Le sommaire stocké
  fait autorité ; ne pas maintenir de liste externe parallèle. Vérifier l’existence des nouveaux
  chemins enfants et utiliser la langue du sommaire lorsque c’est approprié.

Sauvegarder le sommaire avec `meta.contentUpdatedAt` actualisé après une modification d’appartenance.

## Renommage et vérification

Un changement de titre doit modifier `content.title` **et renommer le dossier contenant dans
son parent actuel**, selon les règles de conversion et de collision de la référence sur le
renommage. Un document directement à la racine du store n’a pas de dossier contenant à renommer.
Conserver le nom du fichier document. Préférer `files_rename` pour le déplacement afin que
le worker répare les liens, puis relire le document à son nouveau chemin.

Si seul l’accès direct au système de fichiers est disponible, déplacer le dossier et réparer
explicitement les références concernées : `contents[].path`, `members` de chaque sommaire,
liens `unit::` et autres liens du store vers ce dossier, y compris dans les fichiers déplacés.
Respecter les exclusions `[refactor]` de `.kiao-files`. Modifier les véritables cibles de
référence en respectant les limites de dossier, y compris les destinations Markdown encodées ;
ne pas remplacer les simples mentions dans la prose ni les chemins partageant seulement un
préfixe de nom. Garder intacts les champs sans rapport avec l’opération et actualiser les
horodatages de contenu des documents d’unité modifiés par la réparation.

Analyser chaque document JSON modifié, vérifier les champs requis et l’unicité des identifiants
de contenu, puis vérifier que les références `contents` et `members` nouvelles ou déplacées
se résolvent. Confirmer l’ordre de lecture, la préservation des données sans rapport avec
l’opération et des autres appartenances. Indiquer les chemins finaux des unités et sommaires,
ainsi que tout échec restant ; l’écriture des fichiers seule ne synthétise pas l’audio des
podcasts et ne réalise pas la publication.
