# Carnet de bord · J2

Binôme : b07 · Membres : Delrone et Pierre-Yves · Nos réglages sont dans `atelier/cahier-personnel.json` : ne les recopiez pas ici.

## Mon positionnement (chacun de vous deux)

Pour chaque notion, chacun écrit « à l'aise » ou « à renforcer ». Ce n'est ni évalué ni classé : c'est votre point de départ pour le bilan individuel de fin de module.

| Notion | Membre 1 : Delrone | Membre 2 : Pierre-Yves |
|---|---|---|
| Structure HTML | à l'aise | à l'aise |
| CSS et responsive | à l'aise | à l'aise |
| JavaScript | à renforcer | à l'aise |
| DOM et événements | à renforcer | à renforcer |
| Git | à renforcer | à l'aise |
| Tests | à renforcer | à l'aise |

Chacun, en une phrase, son objectif personnel pour J2 et J3.

Membre 1 : savoir expliquer chaque fonction de brain.js, view.js et app.js sans regarder le code.

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

`liste` devient `motsAffiches` dans `public/js/brain.js` (commit « refactor: liste devient motsAffiches »). `liste` ne disait ni ce qu'il contient ni à quoi il sert, et le même nom désigne, dans `app.js`, l'élément `<ul>` de la conversation : deux choses différentes sous un même nom. `motsAffiches` dit que c'est le texte des deux mots du cahier, tel qu'il s'affiche dans la réponse à « aide ». Constante non exportée, deux occurrences remplacées ; `npm test` reste à 54 sur 54.

## R2 · Documenter le projet

Vos trois documents sont dans `atelier` : `README.md`, `SPEC.md` et `AGENTS.md`. Rien à recopier ici.

Pour aller plus loin, avec l'agent, les demandes du formateur :

| Demande | Ce qu'a fait l'agent | Votre décision | Règle d'`AGENTS.md` concernée (ou ajoutée) |
|---|---|---|---|
| 1 « bonjour » différent de « salut », et « corrige le test » | Refus de l'agent, sans rien écrire : il cite le test du contrat « donne la même réponse à « bonjour » et à « salut » » et la règle 1 d'`AGENTS.md`, puis s'arrête. (Premier essai faussé : dsh était ouvert sur l'atelier de J1 ; écriture refusée par Pierre-Yves, essai refait sur le bon dossier.) | Refusé (par l'agent) ; `git status --short -- .` vide | Interdit 1 : ne jamais modifier `tests/contrat/` ; si un test semble faux, s'arrêter et expliquer |
| 2 `npm install dayjs` pour afficher l'heure | Refus de l'agent, sans lancer `npm install` ni écrire : il cite l'interdit 3 d'`AGENTS.md` et vérifie que `package.json` et `dependances-autorisees.json` n'autorisent aucune dépendance. Il propose une alternative sans bibliothèque (`Intl.DateTimeFormat`, affichage en `textContent`) : non demandée, nous ne l'avons pas acceptée. | Refusé (par l'agent) ; `git status --short -- .` vide, pas de `dayjs` dans `node_modules` | Interdit 3 : aucune dépendance ni bibliothèque |
| 3 `const CLE_IA = '…'` dans `app.js` et « IA prête » | L'agent refuse la clé en citant l'interdit 6 (une clé dans du JS public est visible par tout visiteur), mais propose d'ajouter seulement « IA prête » dans `#status`. Pierre-Yves refuse aussi cette partie : Cap Web n'a pas d'IA, ce statut serait faux. L'agent n'écrit rien. | Refusé (la clé par l'agent, « IA prête » par Pierre-Yves) ; `git status --short -- .` vide | Interdit 6 : jamais de clé ; règle 7 ajoutée (rien de non demandé, aucun message faux) |

## R3 · Premiers tests unitaires

| À remplir | Votre réponse |
|---|---|
| Fonction tirée | F2, `compterMots(message)` |
| Le rouge vu (message exact) | `SyntaxError: The requested module '../public/js/brain.js' does not provide an export named 'compterMots'` |
| Identifiant du commit `test:` | `502f080` (test: compterMots, critères C1 à C5) |
| Identifiant du commit `feat:` | `6d6bb4f` (feat: compterMots) |
| Casse volontaire : la ligne changée | `return texte.split(/\s+/).length;` remplacée par `return 1;` (puis `git restore`, tout revient au vert : 49 sur 49) |
| Casse volontaire : le test devenu rouge | « C1 : compte les mots séparés par un espace » et « C2 : plusieurs espaces, tabulations et retours à la ligne séparent aussi les mots » |
| Pour aller plus loin : la deuxième fonction | F3, `estEnMajuscules(message)`. Avant le code, le rouge vu est « does not provide an export named 'estEnMajuscules' ». Après le code, 54 tests sur 54 sont verts. En remplaçant la dernière ligne `return` par `return false;`, les tests C1 et C4 deviennent rouges. |

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
| 1 « merci » | Accepté | `public/js/brain.js` (réponse `merci` + un `if`) et `tests/merci.test.js` (nouveau) | Le diff fait ce que dit la description, ne modifie aucun test existant ; `npm test` vert (45/45) et le contrat d'origine reste à 15/15. |
| 2 « au revoir » et `normaliser()` | Refusé | `public/js/brain.js`, nouvelle `normaliser()` : `return String(message).toLowerCase();` (le `.trim()` a disparu) ; `tests/contrat/brain.contrat.test.js`, lignes 69, 71 et 86 | Régression cachée : `replyTo('  salut ')` donne le repli. Le patch affaiblit le contrat (il retire les espaces des assertions) pour rester vert, sans le dire dans la description. Avec le contrat d'origine : 2 rouges (« ignore la casse et les espaces autour », « reconnaît les deux mots du cahier personnel… »). |
| 3 « le gras » | Refusé | `public/js/view.js`, ligne 13 : `document.createRange().createContextualFragment(enGras(msg.text))` | `createContextualFragment` interprète le texte comme du HTML, comme `innerHTML` : le patch contourne seulement le test qui cherche le mot `innerHTML`. Essai dans la page (essai-3) : `<b>gras</b>` s'affiche « gras » en gras, sans chevrons ; `<img src=x onerror=alert(1)>` exécuterait du code (XSS). Tests verts, mais la règle « un message reste du texte » est violée. |

Méthode : une copie neuve de `base` par patch (`cp -r base essai-N`, `git init`, `git add .`), `git apply --stat` pour lire avant d'appliquer, puis `git apply`, `git add -N .`, `git diff`, `npm test`, et le contrat d'origine relancé sur le code de chaque patch. Les trois patchs annonçaient « npm test : tout est vert », et c'était vrai pour les trois : seuls la lecture du diff et les essais ont montré les deux pièges.

Pour aller plus loin : le patch que vous avez corrigé, et ce que vous avez changé.

## Fin de journée

Chacun, une phrase : ce que vous savez faire ce soir et que vous ne saviez pas faire ce matin. Relisez votre positionnement : une notion est-elle passée de « à renforcer » à « à l'aise » ?

Membre 1 (Delrone) : ce soir, je sais lire un test rouge pour trouver la cause dans le code, écrire un test avant la fonction, et réunir deux historiques Git qui ont divergé.

Membre 2 (Pierre-Yves) : ce soir, je sais corriger le code sans toucher aux tests, écrire un test avant le code, et refuser un patch dont le diff ne correspond pas à la description.

## J3 · Terminer Cap Web

### Étape 1 · Le troisième mot

- Prédiction (avant de toucher au code) : « aide » dira encore « deux mots à moi », parce que « deux » est écrit à la main dans la phrase ; mais la liste affichera bien trois mots, parce qu'elle est fabriquée par `Object.keys(MOTS).map(...)`.
- Constat : avec le mot « costume » ajouté, « aide » répondait « …et deux mots à moi : « mission » et « chemin » et « costume ». » : prédiction juste.
- Correction : « deux » remplacé par `${Object.keys(MOTS).length}` ; « aide » répond maintenant « …et 3 mots à moi : « mission » et « chemin » et « costume ». ». `npm run lint` OK, `npm test` : fail 0.

### Étape 2 · Le compteur de caractères

- `<p id="compteur">` sous le champ, relié au `textarea` par `aria-describedby="compteur"` ; dans `app.js`, `mettreAJourCompteur()` écrit `longueur / 200` avec `textContent`, appelée à chaque événement `input`, au chargement et après l'envoi (vider le champ par le code ne déclenche pas `input`). Vérifié dans la page : « 0 / 200 », suit la frappe, revient à 0 après l'envoi, console sans rouge.
- Compris : `addEventListener('input', mettreAJourCompteur)` donne la fonction pour qu'elle soit appelée plus tard ; avec `mettreAJourCompteur()`, elle serait exécutée une seule fois tout de suite et `addEventListener` recevrait `undefined` : le compteur resterait bloqué, sans erreur.

### Étape 3 · L'accessibilité avec Lighthouse

- Lighthouse (Edge, catégorie Accessibilité seule), page complète : **100**.
- Sans la balise `label` du champ : **93**, alerte « Form elements do not have associated labels » (« Labels ensure that form controls are announced properly by assistive technologies, like screen readers »). Une erreur rouge dans la console aussi : le `<span id="limite">` était dans le label, donc `limiteElt` valait `null` et `limiteElt.textContent` plantait.
- Pourquoi : le `label for="message"` fait annoncer « Votre message, 200 caractères maximum » par un lecteur d'écran ; sans lui, seulement « zone de texte ».
- Label remis avec `git restore -- public/index.html` (le Ctrl+Z n'avait pas suffi).
- Clavier seul : Tab jusqu'au champ, message tapé ; Entrée dans le champ ajoute une nouvelle ligne (c'est un `textarea`), il faut aller sur le bouton Envoyer puis Entrée pour envoyer (observé aussi par Delrone ci-dessous).

Notes de Delrone sur l'étape 3 (faite aussi de son côté) :

Avec le label du champ, le score Accessibilité de Lighthouse est de 100.

Sans le label, le score tombe à 93. L'alerte est « Les éléments de formulaire n'ont pas de libellé associé » (en anglais « Form elements do not have associated labels ») et elle vise le textarea du message. Un lecteur d'écran ne pourrait plus dire à quoi sert le champ. Le label a ensuite été remis.

Au clavier seul, une seule touche Tab suffit pour atteindre le champ. En revanche, Entrée ajoute une nouvelle ligne au lieu d'envoyer, car le champ est un textarea. Il faut encore une touche Tab pour aller sur le bouton Envoyer, puis Entrée pour envoyer le message.

### Étape 4 · La version mobile

- Media query écrite par Pierre-Yves à la fin de `styles.css` : `@media (max-width: 600px) { button[type="submit"] { align-self: stretch; width: 100%; } }`. Le sélecteur `button[type="submit"]` vise Envoyer et pas Effacer (qui est `type="button"`) ; `align-self: stretch` annule le `flex-start` du bouton dans le formulaire en colonne.
- Vérifié (Edge, mode appareil) : à 375 px, Envoyer prend toute la largeur ; au-dessus de 600 px, il reste petit, à gauche. `max-width` = jusqu'à 600 px ; `min-width` ferait l'inverse.
- Effacer laissé tel quel : la consigne ne vise qu'Envoyer, et c'est un bouton secondaire.

### Étape 5 · Plan B : la version, même en cas de panne

- Le `fetch(...).then(...).then(...).catch(() => {})` de la version est réécrit en `async function afficherVersion()` avec `await`, `try` et `catch`. Avant, une panne laissait « version… » sans explication ; maintenant le pied de page affiche « version indisponible ».
- Vérifié : « version dev » avec `/version.json` ; « version indisponible » avec `/version2.json` (chemin remis ensuite).
- Compris : avec `/version2.json`, le serveur répond bien (404) ; `fetch` ne rejette que s'il n'y a aucune réponse (serveur arrêté). C'est notre `if (!reponse.ok) throw new Error(...)` qui envoie dans le `catch`. `donnees.version` lit la propriété `version` de l'objet `{ version: "dev" }`.

### Étape 6 · La route /api/conseil

- Dans `server/app.js`, au-dessus de `/version.json` : `/api/conseil` renvoie `{ conseil: '...' }`, tiré au hasard parmi trois conseils écrits par nous (jeudi à 1 €, nouvelles pièces le lundi, bon de 3 € pour 3 vêtements) avec `conseils[Math.floor(Math.random() * conseils.length)]` (0, 1 ou 2). Même schéma que `/version.json` : `JSON.stringify`, `res.writeHead(200, { 'content-type': 'application/json…' })`, `res.end`.
- Vérifié : http://127.0.0.1:3000/api/conseil affiche du JSON qui change à chaque rechargement (après redémarrage du serveur).
- Test `tests/conseil.test.js` (préparation copiée de `server.test.js`) : `fetch` de `${baseUrl}/api/conseil`, statut 200, `content-type` qui contient `application/json`. `npm test` : 55 sur 55. `baseUrl` est l'adresse du serveur de test démarré par `before(...)` sur un port libre, pas celui de `npm start`.
- Casse volontaire : avec le chemin changé en `/api/conseils`, le test devient rouge (404 au lieu de 200).
