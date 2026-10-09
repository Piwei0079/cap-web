# Bilan · Delrone (binôme b07, Cap Web)

## Niveau de départ (positionnement de mardi)

| Notion | Mardi |
|---|---|
| Structure HTML | maîtrisé |
| CSS et responsive | maîtrisé |
| JavaScript | à renforcer |
| DOM et événements | à renforcer |
| Git | à renforcer |
| Tests | à renforcer |

## Deux acquis, prouvés par un commit

1. **Écrire un test avant le code**, avec le commit `80c6415` (test: estEnMajuscules, critères C1 à C5) puis le commit `8324740` (feat: estEnMajuscules). Le test a d'abord été vu rouge pour la bonne raison, parce que la fonction n'existait pas encore (« does not provide an export named 'estEnMajuscules' »). La fonction l'a ensuite rendu vert. Elle garde seulement les lettres du message, en exige au moins deux et vérifie qu'elles sont toutes en majuscules. En remplaçant son dernier `return` par `return false`, les tests C1 et C4 deviennent rouges, ce qui prouve que le test vérifie vraiment le code.

2. **Appeler le serveur avec `fetch`, `async` et `await`, et gérer la panne**, avec le commit `599e175` (feat: Cap Web donne un conseil). La fonction `demanderConseil()` appelle `/api/conseil` avec `await fetch`, vérifie `reponse.ok`, puis lit le JSON. Si le serveur est arrêté, `fetch` échoue, l'erreur part dans le `catch` et Cap Web répond « Le serveur ne répond pas » au lieu de planter. L'écouteur `submit` est devenu `async` pour pouvoir attendre la réponse avec `await`.

## Deux points à renforcer

1. **Git à deux.** Mon binôme et moi avons fait plusieurs étapes en double, ce qui a créé des branches divergentes et des fusions à répétition. Je dois faire un `git pull` avant chaque début de travail, annoncer l'étape que je prends, et passer par une branche et une pull request, comme pour le commit `e3c5bbd` (feat: nouvelle couleur).

2. **Le DOM et les événements sans modèle.** Le compteur de caractères (commit `f7e0767`) écoute l'événement `input` et met la page à jour avec `textContent`. Je dois encore m'entraîner à écrire seul ce genre d'écouteur, sans partir d'un exemple.

## Mon objectif

Écrire seul une petite page qui réagit aux événements et appelle une API avec `fetch`, en faisant un commit par étape et sans copier de modèle.
