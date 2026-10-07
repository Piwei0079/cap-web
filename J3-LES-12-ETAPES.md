# Jour 3 · Terminer Cap Web en 12 étapes

Hier, vous avez réparé et testé Cap Web. Aujourd'hui, vous le terminez : vous lui ajoutez ce que demande le livrable, un compteur, une version mobile, une route serveur en JSON, et le projet sur GitHub, à deux. Faites les étapes dans l'ordre, à votre rythme. Chaque étape se termine par une vérification, et presque toujours par un commit : c'est votre preuve.

Vous travaillez dans le dossier d'hier, `cap-web-j2\atelier`. Il n'y a pas de nouveau ZIP. Ouvrez deux terminaux dans ce dossier : le premier lance le serveur avec `npm start` et reste ouvert ; le second sert à Git et aux tests. Cap Web s'ouvre sur http://127.0.0.1:3000.

Bloqué ? Relisez l'étape, demandez au binôme voisin, puis levez la main. Les problèmes fréquents et leur solution sont à la fin de ce document.

## Avant de commencer

Dans le second terminal, vérifiez que tout est enregistré et que les tests passent. `git status` ne doit lister aucun fichier modifié ; sinon, faites d'abord un commit. Si `npm test` affiche `fail 0`, passez à l'étape 1. Sinon, faites d'abord le rattrapage juste en dessous.

```powershell
git status
npm test
```

## Rattrapage (seulement si des tests sont encore rouges depuis hier)

Dans `public/js/brain.js`, faites quatre corrections. Dans `REPONSES`, ajoutez une réponse de repli, sans oublier la virgule à la fin de la ligne `test` : `repli: 'Je ne connais pas encore cette phrase. Écrivez « aide » pour voir les mots que je connais.'`. Dans `validateMessage`, déplacez la ligne `const value = raw.trim();` juste au-dessus de `if (raw === '')`, changez ce test en `if (value === '')`, et remplacez `280` par `LIMITE`. Dans `replyTo`, la première ligne devient `const texte = String(message).trim().toLowerCase();` et la dernière `return REPONSES.repli;`.

Dans `public/js/view.js`, remplacez les deux lignes `const nom = …` et `li.innerHTML = …` par ces trois lignes :

```js
const nom = document.createElement('strong');
nom.textContent = msg.role === 'user' ? 'Vous' : 'Cap Web';
li.append(nom, ` : ${msg.text}`);
```

C'est fini quand `npm test` affiche `fail 0`. Puis :

```powershell
git add -- public/js/brain.js public/js/view.js
git commit -m "fix: rattrapage du round 1"
```

## Étape 1 · Le troisième mot

Cap Web connaît deux mots à vous, rangés dans l'objet `MOTS`, en haut de `public/js/brain.js`. Avant de toucher au code, prédisez : si vous ajoutez un troisième mot, que répondra Cap Web à « aide » ? Écrivez votre prédiction dans le carnet.

Ajoutez un mot en minuscules, sans espace ni tiret, avec sa phrase. Rechargez la page et envoyez « aide » : la réponse dit encore « deux mots », parce que ce nombre est écrit à la main. Dans la phrase `aide` de `REPONSES`, remplacez « deux » par `${Object.keys(MOTS).length}` : le nombre est maintenant calculé.

C'est fini quand « aide » annonce trois mots et que `npm test` affiche `fail 0`.

```powershell
npm run lint
npm test
git add -- public/js/brain.js ../carnet-j2.md
git commit -m "feat: un troisième mot"
```

## Étape 2 · Le compteur de caractères

Sous le champ, un compteur « 0 / 240 » (avec votre limite) suit chaque frappe et revient à 0 après l'envoi.

Dans `public/index.html`, juste sous la ligne du `textarea`, ajoutez `<p id="compteur"></p>`, et dans la balise `textarea`, ajoutez l'attribut `aria-describedby="compteur"`. Dans `public/js/app.js`, trouvez cet élément avec `document.querySelector('#compteur')`, écoutez l'événement `input` du champ, et écrivez dans son `textContent` la longueur du texte, « / », puis `LIMITE`. Après l'envoi, quand le champ est vidé, remettez aussi le compteur à jour.

C'est fini quand le compteur suit la frappe, revient à 0 après l'envoi, et que la console du navigateur (F12) n'affiche rien en rouge.

```powershell
npm run lint
npm test
git add -- public/index.html public/js/app.js
git commit -m "feat: compteur de caractères"
```

## Étape 3 · L'accessibilité avec Lighthouse

Dans Chrome ou Edge, ouvrez F12, puis l'onglet Lighthouse (dans Edge, il est parfois caché derrière `>>`). Cochez seulement Accessibilité, puis lancez l'analyse. Notez le score dans le carnet.

Dans `public/index.html`, retirez la balise `label` du champ, enregistrez, relancez Lighthouse : notez le nouveau score et l'alerte. Une erreur rouge dans la console est normale pendant cet essai. Remettez le label avec Ctrl+Z, puis enregistrez. Enfin, essayez Cap Web au clavier seul : Tab jusqu'au champ, tapez un message, Entrée.

C'est fini quand les deux scores et l'alerte sont dans le carnet, et que `git status` ne liste que le carnet comme modifié.

```powershell
git status
git add -- ../carnet-j2.md
git commit -m "docs: scores Lighthouse"
```

## Étape 4 · La version mobile

Ouvrez F12, puis le mode appareil (Ctrl+Maj+M), largeur 375. À la fin de `public/styles.css`, écrivez une media query : sous 600 px de large, le bouton Envoyer prend toute la largeur.

C'est fini quand, à 375 px, le bouton Envoyer occupe toute la largeur, et que rien ne change sur grand écran.

```powershell
git add -- public/styles.css
git commit -m "feat: version mobile"
```

En avance ? Ajoutez un thème sombre avec `@media (prefers-color-scheme: dark)`, en changeant seulement les variables de `:root`.

## Étape 5 · Plan B : la version, même en cas de panne

Tout en bas de `public/js/app.js`, un `fetch` lit `/version.json` avec `.then`. Réécrivez-le dans une fonction `async function afficherVersion()`, avec `await`, `try` et `catch` : vérifiez `reponse.ok`, puis affichez la version dans `versionElt`. En cas d'erreur, le pied de page affiche « version indisponible ». N'oubliez pas d'appeler `afficherVersion()` juste après.

Pour vérifier le cas d'erreur, remplacez un instant `'/version.json'` par `'/version2.json'` et rechargez la page : le pied de page affiche « version indisponible ». Remettez ensuite le bon chemin.

C'est fini quand le pied de page affiche la version, et « version indisponible » avec un mauvais chemin.

```powershell
npm run lint
npm test
git add -- public/js/app.js
git commit -m "refactor: version avec async/await"
```

## Étape 6 · Votre route /api/conseil

Dans `server/app.js`, juste au-dessus du bloc `if (chemin === '/version.json')`, ajoutez une route `/api/conseil`. Elle renvoie un objet JSON `{ conseil: '...' }`, tiré au hasard dans un tableau de trois conseils. Prenez modèle sur le bloc de `/version.json` : `JSON.stringify`, `res.writeHead(200, ...)`, puis `res.end`.

Le serveur ne relit pas son code tout seul : arrêtez-le (Ctrl+C dans le premier terminal), puis relancez `npm start`. Ouvrez http://127.0.0.1:3000/api/conseil : vous voyez votre JSON.

Pour le test, copiez le fichier des tests du serveur avec `Copy-Item tests\server.test.js tests\conseil.test.js`. Dans la copie, gardez tout jusqu'à la fin du bloc `after(...)`, supprimez les tests qui suivent, et écrivez un seul test, sur le modèle du premier test du fichier : `fetch` de `${baseUrl}/api/conseil`, puis `assert.equal` pour vérifier le statut 200, et `assert.match` pour vérifier que l'en-tête `content-type` contient `application/json`.

```powershell
npm test
git add -- server/app.js tests/conseil.test.js
git commit -m "feat: route /api/conseil"
```

C'est fini quand l'adresse `/api/conseil` affiche du JSON et que `npm test` affiche `fail 0`, avec votre test en plus.

## Étape 7 · Cap Web donne un conseil

Dans `public/js/app.js`, écrivez une fonction `async function demanderConseil()` qui appelle `/api/conseil` et renvoie le conseil reçu. En cas d'erreur, elle renvoie un message clair, par exemple « Le serveur ne répond pas : conseil indisponible. »

Dans l'écouteur `submit`, ajoutez `async` devant `(event) =>`. Si le message envoyé est « conseil », la réponse de Cap Web devient `await demanderConseil()` ; sinon, c'est `replyTo` comme avant. Vérifiez aussi le cas d'erreur : arrêtez le serveur (Ctrl+C), envoyez « conseil » dans la page déjà ouverte, puis relancez `npm start`.

C'est fini quand « conseil » affiche un conseil, et votre message d'erreur quand le serveur est arrêté, sans écran blanc.

```powershell
npm run lint
npm test
git add -- public/js/app.js
git commit -m "feat: Cap Web donne un conseil"
```

En avance ? Avec l'API publique `geo.api.gouv.fr`, faites répondre à « commune 69001 » le nom de la commune.

## Étape 8 · Le projet sur GitHub, à deux

Une seule personne du binôme, appelée A ici, crée le dépôt sur github.com : New repository, nom `cap-web`, Private, sans README. Puis, dans son terminal, dans `atelier`, elle tape ces trois lignes, en remplaçant `COMPTE` par son nom de compte GitHub. Si une fenêtre de connexion s'ouvre, connectez-vous.

```powershell
git branch -M main
git remote add origin https://github.com/COMPTE/cap-web.git
git push -u origin main
```

Sur GitHub, A ouvre Settings, puis Collaborators, puis Add people, et invite le compte de B. B accepte l'invitation sur `github.com/COMPTE/cap-web/invitations`, puis récupère le projet sur son poste, dans ses Documents, en dehors du dossier `cap-web-j2` :

```powershell
cd $HOME\Documents
git clone https://github.com/COMPTE/cap-web.git
cd cap-web\atelier
npm ci
```

C'est fini quand le projet de A est sur GitHub et que `npm start` lance Cap Web chez B, depuis son clone.

## Étape 9 · Chacun sa branche

Chacun, sur son poste, dans `atelier`, tape d'abord `git pull`, puis crée une branche pour un petit changement, chacun dans un fichier différent. A ajoute l'arborescence du projet dans `README.md` ; B change une couleur dans `public/styles.css`. Par exemple, pour A :

```powershell
git switch -c docs/arborescence
git add -- README.md
git commit -m "docs: arborescence du projet"
git push -u origin docs/arborescence
```

Pour B :

```powershell
git switch -c feat/couleur
git add -- public/styles.css
git commit -m "feat: nouvelle couleur"
git push -u origin feat/couleur
```

Si Git demande qui vous êtes, tapez une fois `git config --global user.name "Prénom Nom"` et `git config --global user.email "vous@exemple.fr"`, puis refaites le commit.

Sur GitHub, cliquez sur Compare & pull request. Donnez un titre, puis deux lignes : ce qui change, et comment le vérifier. C'est fini quand chacun a sa pull request ouverte.

## Étape 10 · Relire la pull request de l'autre

Chacun ouvre la pull request de l'autre, onglet Files changed, et vérifie dans cet ordre : ce qui est annoncé correspond à ce qui est fait ; rien de dangereux, un texte d'utilisateur reste du texte ; le code est lisible ; les tests et le lint passent. Puis laissez un commentaire qui commence par son type, question, suggestion, problème ou bravo, et qui cite un fait précis.

Ensuite, Review changes, puis Approve. L'auteur fusionne avec Merge pull request. Enfin, chacun sur son poste :

```powershell
git switch main
git pull --no-rebase
git log --oneline --graph
```

C'est fini quand les deux pull requests sont fusionnées et que `git log` montre les deux fusions. Appuyez sur `q` pour sortir de l'affichage.

## Étape 11 · Les quatre attaques, puis le README

Votre Cap Web affronte quatre attaques, et doit tenir debout à chaque fois. Un : serveur arrêté, puis « conseil » ; un message clair s'affiche, pas un écran blanc. Deux : un message plus long que votre limite est refusé, avec une erreur visible. Trois : `<b>test</b>` envoyé dans le champ s'affiche tel quel, chevrons compris. Quatre : à 375 px de large, tout reste lisible. Une attaque passe ? Corrigez-la, avec un commit `fix:`.

Puis A met `README.md` à jour : à quoi sert Cap Web, comment l'installer, le lancer et le tester, l'arborescence commentée, et la route `/api/conseil`. A l'envoie sur GitHub, puis B le récupère et essaie chaque commande du README dans son clone.

```powershell
git add -- README.md
git commit -m "docs: README final"
git push
```

Chez B : `git pull --no-rebase`, puis les commandes du README, une par une.

C'est fini quand Cap Web résiste aux quatre attaques, et que B a lancé le projet en suivant seulement le README.

## Étape 12 · Le bilan, et tout sur GitHub

Chacun écrit son bilan dans `bilan/PRENOM.md`, avec votre prénom à la place de PRENOM : votre niveau de départ (le positionnement de mardi), deux acquis prouvés chacun par l'identifiant d'un commit (`git log --oneline`), deux points à renforcer, et un objectif. Puis chacun, sur son poste :

```powershell
git add -- bilan ../carnet-j2.md
git commit -m "docs: bilan individuel"
git pull --no-rebase --no-edit
git push
```

C'est fini quand `git push` répond « Everything up-to-date » chez vous deux, et que toutes les lignes de la checklist du livrable (onglet Fichiers) sont cochées.

## Si ça bloque

| Ce qui se passe | Ce qu'on fait |
|---|---|
| `npm start` affiche « EADDRINUSE » | un autre terminal lance déjà le serveur : fermez-le ; sinon tapez `$env:PORT=3001`, puis `npm start`, et ouvrez http://127.0.0.1:3001 |
| `/api/conseil` répond 404 | redémarrez le serveur : Ctrl+C, puis `npm start` |
| `npm test` est vert, mais la page ne marche pas | `npm run lint`, et regardez la console F12 : rien ne doit être rouge |
| Lighthouse introuvable dans Edge | dans les outils F12, derrière le bouton `>>` |
| `git push` répond 403 | les identifiants d'un autre compte sont enregistrés sur le poste : levez la main |
| « remote origin already exists » | `git remote remove origin`, puis recommencez l'étape 8 |
| `npm ci` échoue après le clone | vous n'êtes pas dans le bon dossier : `cd cap-web\atelier` |
| « Need to specify how to reconcile divergent branches » | `git pull --no-rebase` |
| Un éditeur s'ouvre pendant un commit | appuyez sur Échap, tapez `:wq`, puis Entrée |
| `git log` ne rend pas la main | appuyez sur `q` |
