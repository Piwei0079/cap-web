# Cap Web

## À quoi sert Cap Web

Cap Web est un petit assistant de discussion pour les clients des friperies et ressourceries, qui tourne dans le navigateur.
Il répond par des règles écrites à l'avance (pas une vraie IA) : « salut », « aide », « test », trois mots à lui (« mission », « chemin », « costume ») et « conseil », qui demande un conseil au serveur.
Il refuse les messages vides ou trop longs (200 caractères au maximum, avec un compteur sous le champ), garde la conversation après un rechargement de la page et s'adapte aux écrans de téléphone.

## Installer et lancer

Prérequis : Node.js 24.20 ou plus récent (`node --version` pour vérifier) et Git.

Dans un terminal, une commande à la fois :

```sh
git clone https://github.com/Piwei0079/cap-web.git
cd cap-web/atelier
npm ci
npm start
```

Ouvrez ensuite http://127.0.0.1:3000 dans le navigateur. `Ctrl+C` dans le terminal arrête le serveur. Le dépôt est privé : il faut avoir été invité comme collaborateur pour le cloner.

Pour lancer les tests, depuis le dossier `atelier` :

```sh
npm test
```

Tous les tests doivent être verts (`fail 0`). Sous Windows, si PowerShell refuse `npm`, tapez `npm.cmd` à la place.

## Arborescence

```text
atelier/
├── public/                      ce que le navigateur reçoit
│   ├── index.html               la page : formulaire, liste des messages, compteur, statut
│   ├── styles.css               la mise en forme, avec la version mobile (sous 600 px)
│   └── js/
│       ├── app.js               le câblage : formulaire, compteur, mémoire, version, conseil
│       ├── brain.js             le cerveau : valide un message et choisit la réponse
│       └── view.js              l'affichage de la conversation, en textContent
├── server/
│   ├── app.js                   le serveur : sert les fichiers de public/, /version.json et /api/conseil
│   └── start.js                 lance le serveur sur http://127.0.0.1:3000
├── tests/
│   ├── contrat/                 le contrat du formateur : ne jamais le modifier
│   ├── server.test.js           les tests du serveur
│   ├── conseil.test.js          le test de la route /api/conseil
│   ├── compterMots.test.js      les tests de compterMots (C1 à C5)
│   └── estEnMajuscules.test.js  les tests de estEnMajuscules (C1 à C5)
├── cahier-personnel.json        nos réglages : limite et mots (ne pas modifier)
├── README.md, SPEC.md, AGENTS.md  la documentation : ce fichier, la spécification, les conventions
└── package.json                 les commandes npm (start, test, lint)
```

Les autres fichiers (`browser/`, `scripts/`, `eslint.config.js`, `playwright*.config.js`, `dependances-autorisees.json`) sont les outils de vérification fournis.

## La route `/api/conseil`

Le serveur (`server/app.js`) répond à `GET /api/conseil` par un objet JSON avec un conseil tiré au hasard parmi trois, par exemple :

```json
{ "conseil": "Le lundi, de nouvelles pièces arrivent en rayon." }
```

Pour l'essayer : ouvrez http://127.0.0.1:3000/api/conseil (le conseil change à chaque rechargement), ou écrivez « conseil » dans Cap Web. Si le serveur ne répond pas, Cap Web affiche « Le serveur ne répond pas : conseil indisponible. » au lieu de planter. La route est vérifiée par `tests/conseil.test.js` (statut 200 et `content-type` JSON).

## Les 3 modules de `public/js`

- `brain.js` : le cerveau. Il vérifie un message (`validateMessage` : pas vide, pas plus de `LIMITE` caractères, espaces autour retirés) et choisit la réponse (`replyTo`). Fonctions pures : il ne touche jamais à la page.
- `view.js` : l'affichage. `renderMessages` transforme l'historique en lignes « Vous : … » et « Cap Web : … » dans la liste, avec `textContent` uniquement (un message comme `<b>gras</b>` s'affiche tel quel). Il ne décide d'aucune réponse.
- `app.js` : le câblage. Il écoute le formulaire et le bouton « Effacer la conversation », met à jour le compteur de caractères, demande à `brain.js` de valider et de répondre (ou au serveur pour « conseil », avec `fetch`), enregistre l'historique dans le navigateur (`localStorage`), demande à `view.js` de l'afficher et affiche la version du serveur dans le pied de page.

On ne modifie jamais `tests/contrat/`, `browser/contrat.spec.js` ni `cahier-personnel.json` : ce sont la spécification et les réglages fournis par le formateur.
