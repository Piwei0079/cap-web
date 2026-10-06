# Spécification de Cap Web

Chaque critère dit ce que fait Cap Web, puis ce qui le vérifie. Les tests cités sont dans `tests/contrat/brain.contrat.test.js` (lancer `npm test` depuis `atelier`) ; les essais se font dans la page, après `npm start`, sur http://127.0.0.1:3000.

1. Quand on envoie un message vide ou fait seulement d'espaces, Cap Web le refuse, n'ajoute aucune ligne à la conversation et affiche « Le message ne doit pas être vide. » dans le statut.
   Vérifié par : test « refuse le vide et les espaces seuls » ; essai : taper trois espaces puis Envoyer.

2. Quand on envoie 201 caractères, Cap Web refuse et l'erreur cite 200 (« Le message doit contenir 200 caractères au maximum. ») ; avec 200 caractères, le message est accepté.
   Vérifié par : test « accepte 200 caractères et refuse 201 ».

3. Quand on écrit « mission » ou « chemin », quelles que soient les majuscules et les espaces autour (par exemple «  MISSION  »), Cap Web répond la phrase propre à ce mot, différente des réponses à « salut », « aide » et « test ».
   Vérifié par : test « reconnaît les deux mots du cahier personnel, quelles que soient la casse et les espaces autour ».

4. Quand on écrit une phrase qu'il ne connaît pas (par exemple « parle-moi de la météo »), Cap Web répond par un repli qui n'est ni la réponse à « salut », ni celle à « aide », ni celle à « test ».
   Vérifié par : test « répond à une phrase inconnue par un repli distinct ».

5. Quand on envoie `<b>gras</b>`, Cap Web affiche « Vous : <b>gras</b> » avec les chevrons, sans mettre le texte en gras : un message reste toujours du texte, jamais du HTML.
   Vérifié par : test « view.js affiche du texte et ne décide pas des réponses » ; essai : envoyer `<b>gras</b>` dans la page.
