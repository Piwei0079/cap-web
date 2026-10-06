# Conventions de Cap Web

Ces règles valent pour tout développeur du projet, humain ou agent. Ce qui n'est pas écrit ici n'est pas supposé connu : en cas de doute, demandez avant d'écrire.

## Nommage

- **Fonction** : un verbe qui dit ce qu'elle fait, en camelCase, comme `validateMessage`, `replyTo` ou `renderMessages`. Une fonction de `brain.js` est pure : mêmes entrées, même sortie, sans accès à la page.
- **Constante** : en MAJUSCULES quand c'est un réglage ou une table de réponses partagée, comme `LIMITE`, `MOTS` ou `REPONSES`. Une valeur réglable n'est écrite qu'à un seul endroit : jamais un nombre comme `200` recopié en dur ailleurs.
- **Variable** : un nom en français qui dit ce qu'elle contient (`historique`, `statut`, `champ`), pas `x`, `tmp` ou `data`.
- **Fichier** : en minuscules, un rôle par fichier, dans `public/js/` : `brain.js` (les règles), `view.js` (l'affichage), `app.js` (le câblage). Un test s'appelle `tests/<nom>.test.js`.
- **Message de commit** : un préfixe puis ce qui change, au présent, en français : `fix:` (correction), `feat:` (fonction nouvelle), `test:` (test ajouté), `docs:` (documentation), `refactor:` (renommage sans changer le comportement). Exemple : `fix: un message fait d'espaces est refusé comme vide`. Un commit = un seul changement.

## Interdits

1. Ne modifie jamais `tests/contrat/`, `browser/contrat.spec.js` ni `cahier-personnel.json`. Un test rouge se corrige dans le code, jamais dans le test. Si un test te semble faux, arrête-toi et explique pourquoi.
2. N'utilise jamais `innerHTML`, `outerHTML` ni `insertAdjacentHTML` : un message s'affiche avec `textContent`, pour que `<b>gras</b>` reste du texte.
3. N'ajoute aucune dépendance, aucune bibliothèque ni aucune adresse `https://` : `package.json`, `package-lock.json` et `dependances-autorisees.json` ne changent pas.
4. `brain.js` ne touche jamais à la page : aucun `document`, `window` ni `localStorage`, même dans un commentaire.
5. Ne crée pas de nouveau fichier dans `public/` sans l'ajouter à la liste de `server/app.js` ; ne modifie pas `server/` ni `scripts/` sans que la consigne le demande.
6. Ne lance jamais git (`commit`, `push`, `reset`…) et n'écris jamais de clé ni de `.env` : les commits et les secrets doivent être écrit par des humains.
