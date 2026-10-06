# Carnet de bord · J2

Binôme : b07 · Membres : Delrone et Pierre-Yves · Nos réglages sont dans `atelier/cahier-personnel.json` : ne les recopiez pas ici.

## Mon positionnement (chacun de vous deux)

Pour chaque notion, chacun écrit « à l'aise » ou « à renforcer ». Ce n'est ni évalué ni classé : c'est votre point de départ pour le bilan individuel de fin de module.

| Notion | Membre 1 : Delrone | Membre 2 : Pierre-Yves |
|---|---|---|
| Structure HTML | | à l'aise |
| CSS et responsive | | à l'aise |
| JavaScript | | à l'aise |
| DOM et événements | | à renforcer |
| Git | | à l'aise |
| Tests | | à l'aise |

Chacun, en une phrase, son objectif personnel pour J2 et J3.

Membre 1 :

Membre 2 : savoir créer et modifier des éléments de la page en JavaScript (createElement, textContent, événements) sans copier d'exemple.

## R1 · Les tests automatisés

Les tests rouges du départ, et ce que vous en avez fait :

Après le commit « J2 : nos réglages » : `node --test tests/contrat/brain.contrat.test.js` → 15 tests, 9 verts, 6 rouges.

| Test rouge | Cause trouvée (une phrase) | Fichier | Message du commit `fix:` |
|---|---|---|---|
| refuse le vide et les espaces seuls | `validateMessage` testait le vide sur le texte brut (`raw === ''`) avant le `trim()`, donc un message d'espaces était accepté. | `public/js/brain.js` | `fix: un message fait d'espaces est refusé comme vide` |
| accepte 200 caractères et refuse 201 | La limite était écrite en dur (`280`) au lieu d'utiliser la constante `LIMITE` ; le message d'erreur, lui, citait bien `LIMITE`. | `public/js/brain.js` | `fix: la limite de longueur utilise LIMITE au lieu de 280 écrit en dur` |
| ignore la casse et les espaces autour | `replyTo` passait le message en minuscules sans retirer les espaces autour, donc «  SALUT  » n'était pas reconnu. | `public/js/brain.js` | `fix: replyTo retire les espaces autour avant de reconnaître un mot (même commit que la ligne suivante)` |
| reconnaît les deux mots du cahier personnel, quelles que soient la casse et les espaces autour | Même cause : sans `trim()`, «  MISSION  » tombait dans le repli. | `public/js/brain.js` | `fix: replyTo retire les espaces autour avant de reconnaître un mot` |
| répond à une phrase inconnue par un repli distinct | Un message inconnu recevait exactement la réponse de « aide » ; ajout d'une réponse `repli` à part. | `public/js/brain.js` | `fix: un message inconnu reçoit un repli distinct de la réponse à aide` |
| view.js affiche du texte et ne décide pas des réponses | `view.js` affichait avec `innerHTML`, donc `<b>gras</b>` aurait été interprété comme du HTML ; remplacé par un `<strong>` créé avec `textContent` et le texte ajouté comme texte. | `public/js/view.js` | `fix: view.js affiche les messages avec textContent, sans innerHTML` |

Résultat : `node --test tests/contrat/brain.contrat.test.js` → 15 tests, 15 verts ; `npm test` → 44 sur 44 ; `git diff --stat depart -- tests cahier-personnel.json` → vide (ni les tests ni le cahier n'ont été modifiés). Dans la page (`npm start`), `<b>gras</b>` s'affiche tel quel, chevrons compris : « Vous : <b>gras</b> », puis le repli de Cap Web. 5 commits `fix:`, un par défaut (le défaut du `trim()` dans `replyTo` faisait rougir deux tests).

Avec l'agent : ce qu'il a proposé et que vous avez refusé, et pourquoi.
Corrections faites sans dsh : chaque test rouge lu (nom = la règle, message = l'écart), la cause cherchée dans `public/js/`, le diff relu, les tests relancés après chaque correction.

Pour aller plus loin : le nom renommé par votre commit `refactor:`, et pourquoi le nouveau est plus clair.

## R2 · Documenter le projet

Vos trois documents sont dans `atelier` : `README.md`, `SPEC.md` et `AGENTS.md`. Rien à recopier ici.

Pour aller plus loin, avec l'agent, les demandes du formateur :

| Demande | Ce qu'a fait l'agent | Votre décision | Règle d'`AGENTS.md` concernée (ou ajoutée) |
|---|---|---|---|
| 1 | | | |
| 2 | | | |
| 3 | | | |

## R3 · Premiers tests unitaires

| À remplir | Votre réponse |
|---|---|
| Fonction tirée | F2, `compterMots(message)` |
| Le rouge vu (message exact) | `SyntaxError: The requested module '../public/js/brain.js' does not provide an export named 'compterMots'` |
| Identifiant du commit `test:` | `502f080` (test: compterMots, critères C1 à C5) |
| Identifiant du commit `feat:` | `6d6bb4f` (feat: compterMots) |
| Casse volontaire : la ligne changée | `return texte.split(/\s+/).length;` remplacée par `return 1;` (puis `git restore`, tout revient au vert : 49 sur 49) |
| Casse volontaire : le test devenu rouge | « C1 : compte les mots séparés par un espace » et « C2 : plusieurs espaces, tabulations et retours à la ligne séparent aussi les mots » |
| Pour aller plus loin : la deuxième fonction | |

Les critères C1 à C5 de votre fonction, recopiés de la fiche :

- C1 : `'salut'` donne 1, `'où est le refuge'` donne 4.
- C2 : `'un   deux'` donne 2, `'un\tdeux\ntrois'` donne 3.
- C3 : `'   salut   '` donne 1.
- C4 : `''` et les espaces seuls donnent 0.
- C5 : ce qui n'est pas du texte donne 0, sans erreur.

Chaque critère a au moins une assertion dans `tests/compterMots.test.js` (un `it` par critère, nommé C1 à C5).

## R4 · La revue de code

| Patch | Accepté ou refusé | Fichier et ligne | Raison |
|---|---|---|---|
| 1 | | | |
| 2 | | | |
| 3 | | | |

Pour aller plus loin : le patch que vous avez corrigé, et ce que vous avez changé.

## Fin de journée

Chacun, une phrase : ce que vous savez faire ce soir et que vous ne saviez pas faire ce matin. Relisez votre positionnement : une notion est-elle passée de « à renforcer » à « à l'aise » ?
