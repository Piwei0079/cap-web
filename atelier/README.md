# Cap Web

## À quoi sert Cap Web

Cap Web est un petit assistant de discussion pour les clients des friperies et ressourceries, qui tourne dans le navigateur.
Il répond par des règles écrites à l'avance (pas une vraie IA) : « salut », « aide », « test » et deux mots à lui, « mission » et « chemin ».
Il refuse les messages vides ou trop longs (200 caractères au maximum) et garde la conversation après un rechargement de la page.

## Installer et lancer

Prérequis : Node.js 24.20 ou plus récent (`node --version` pour vérifier) et Git.

Dans un terminal, une commande à la fois :

```sh
git clone https://github.com/delrone91/cap-web-j2.git
cd cap-web-j2/atelier
npm ci
npm start
```

Ouvrez ensuite http://127.0.0.1:3000 dans le navigateur. `Ctrl+C` dans le terminal arrête le serveur.

Pour lancer les tests, depuis le dossier `atelier` :

```sh
npm test
```

Tous les tests doivent être verts (`fail 0`). Sous Windows, si PowerShell refuse `npm`, tapez `npm.cmd` à la place.

## Les 3 modules de `public/js`

- `brain.js` : le cerveau. Il vérifie un message (`validateMessage` : pas vide, pas plus de `LIMITE` caractères, espaces autour retirés) et choisit la réponse (`replyTo`). Fonctions pures : il ne touche jamais à la page.
- `view.js` : l'affichage. `renderMessages` transforme l'historique en lignes « Vous : … » et « Cap Web : … » dans la liste, avec `textContent` uniquement (un message comme `<b>gras</b>` s'affiche tel quel). Il ne décide d'aucune réponse.
- `app.js` : le câblage. Il écoute le formulaire et le bouton « Effacer la conversation », demande à `brain.js` de valider et de répondre, enregistre l'historique dans le navigateur (`localStorage`) et demande à `view.js` de l'afficher.

On ne modifie jamais `tests/contrat/`, `browser/contrat.spec.js` ni `cahier-personnel.json` : ce sont la spécification et les réglages fournis par le formateur.
