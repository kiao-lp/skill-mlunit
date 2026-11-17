---
status: APPROVED
role: PRIMARY_SOURCE
---

# Renommage et chemins

Lire cette référence pour renommer ou déplacer une unité, résoudre une collision de dossiers
ou examiner la conversion d’un titre en nom de dossier.

## Changement de titre

`unit_update` avec `title` renomme le dossier de l’unité **dans son parent actuel**. Le document
conserve son docId et son nom de fichier. Une unité classée sous un sujet ou dans un épisode
reste dans ce parent. Renommer une série déplace aussi ses épisodes imbriqués.
Un document directement à la racine du store n’a pas de dossier contenant à renommer.

Le worker répare dans la même opération les références vers tout ce qui se trouve dans le dossier :
`members` de chaque sommaire, liens `unit::` des articles et `contents[].path` de l’unité.
Seules les références écrites comme des liens sont suivies : chemins depuis la racine du store,
avec une barre oblique initiale.

Utiliser le **nouveau chemin renvoyé par `unit_update`** ; un ancien chemin devient invalide
quand son dossier est déplacé. `unit_list` associe docIds et chemins actuels. Le titre n’est pas
une identité : un dossier calculé déjà occupé devient `<name> (docId)`, à la création comme au
changement de titre. Un `folder` explicitement fourni à la création est utilisé tel quel ;
l’inspecter avant d’y ajouter des fichiers.

## Déplacement

Pour changer le parent d’une unité, déplacer son dossier avec l’opération de renommage de
fichiers du worker (`files_rename` si disponible). Un changement de titre modifie seulement
le nom du dossier. Définir `folder` à la création pour choisir son emplacement initial.

L’opération de renommage du worker maintient les liens. Un déplacement direct dans le système
de fichiers ne déclenche pas son correcteur de liens. Si l’opération MCP d’unité est indisponible,
suivre le mode sans MCP chargé sous condition pour les écritures équivalentes et les réparations
nécessaires ; modifier le JSON seul ne réalise pas le renommage.

## Conversion en nom de dossier

`toStoreName` de `kiao-worker/utils` conserve l’Unicode lisible, les accents, la casse, les
parenthèses et les tirets cadratins. Utiliser cette fonction si elle est disponible ; sinon,
reproduire les règles suivantes dans l’ordre :

1. Normaliser le titre en NFC.
2. Remplacer `/` et `\` par `-`.
3. Supprimer `: * ? " < > | #`, U+0000–U+001F et U+007F.
4. Réduire les suites de blancs à une espace ; retirer les points et blancs aux extrémités.
5. Limiter le résultat à **180 octets UTF-8** sans couper de caractère, puis nettoyer à nouveau
   ses extrémités.
6. Utiliser le docId si le résultat est vide ou si sa partie avant le premier point est un nom
   réservé par Windows, sans distinction de casse : `CON`, `PRN`, `AUX`, `NUL`, `COM1`–`COM9`,
   `LPT1`–`LPT9`.

Les espaces et signes de ponctuation qui survivent à ces règles restent dans le nom du dossier.
La troncature est silencieuse ; garder des titres courts pour éviter les noms tronqués ou opaques.
La conversion ordinaire ne transforme pas les mots en minuscules séparées par des tirets ;
la convention des épisodes de module est définie dans la référence de réalisation des supports.

L’implémentation est maintenue dans
[les utilitaires partagés du worker](/pub/reference/kiao-worker/src/utils.ts) ; le placement
et le renommage des unités sont implémentés dans
[les fonctions de chemins d’unité](/perso/reference/npm/kiao-perso-mcp/src/unit/paths.ts).
