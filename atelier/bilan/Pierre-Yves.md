# Bilan · Pierre-Yves (binôme b07, Cap Web)

## Niveau de départ (positionnement de mardi)

| Notion | Mardi |
|---|---|
| Structure HTML | à l'aise |
| CSS et responsive | à l'aise |
| JavaScript | à l'aise |
| DOM et événements | à renforcer |
| Git | à l'aise |
| Tests | à l'aise |

## Deux acquis, prouvés par un commit

1. **Un serveur qui répond en JSON, et son test** — commit `765beb6` (feat: route /api/conseil). J'ai ajouté la route `/api/conseil` dans `server/app.js` : un conseil tiré au hasard avec `Math.floor(Math.random() * conseils.length)`, envoyé avec `JSON.stringify` et un `content-type: application/json`. Le test `tests/conseil.test.js` démarre un serveur de test sur un port libre (`baseUrl`), appelle la route avec `fetch` et vérifie le statut 200 et le type JSON ; il devient rouge si on casse le chemin de la route.
2. **Réécrire un `fetch` avec `async`/`await` et gérer les erreurs** — commit `d2a7709` (refactor: version avec async/await). Le `.then(...).catch(() => {})` qui ne disait rien en cas de panne est devenu `afficherVersion()` avec `try`/`catch` : le pied de page affiche « version indisponible ». Je sais distinguer les deux pannes : sans réponse (serveur arrêté), c'est `fetch` qui échoue ; avec une réponse en erreur (404), c'est à moi de vérifier `reponse.ok`.

## Deux points à renforcer

1. `async`/`await` en JavaScript pur, sans typage : bien suivre ce que contient chaque variable à chaque `await` (`reponse` l'enveloppe, `reponse.json()` qui l'ouvre, `donnees.conseil` qui lit le contenu), sans l'aide d'un type qui le dit.
2. React : je ne l'ai pas encore pratiqué ; Cap Web m'a montré ce que fait un framework à ma place (créer les éléments, les mettre à jour à chaque événement), que j'ai fait ici à la main avec le DOM.

## Mon objectif

Me réentraîner à écrire des requêtes vers une API sans framework (`fetch`, `async`/`await`, gestion des erreurs), seul et sans modèle.
