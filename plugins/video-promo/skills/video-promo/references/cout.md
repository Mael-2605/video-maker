# Coût

> Rôle : la procédure d'argent utilisée à chaque étape payante, et le bilan de fin de projet.

## Avant

- Au début de chaque projet : `balance` → écrire le solde de départ dans `projets/<slug>/cout.md` (gabarit `templates/projet-cout.md`).
- Avant chaque génération payante, quand l'outil accepte `get_cost:true` : l'appeler pour connaître le prix exact.
- Images nouvelles et vues du lieu : prix total dans la ligne `Je fixe` de la carte vision (`get_cost` par élément, par le concepteur ; vues : leur nombre × le prix d'une, ~2 crédits, `references/vues-lieu.md#Nombre et prix`), annoncé sans question ; elles partent à la réponse du client, les vues une fois le découpage écrit. Aucune voix à payer : pas d'extrait, pas de réplique générée à part. Une vue refaite ou ajoutée après une correction : son prix en une ligne, sans question. Un plan corrigé ne refait aucune image.
- Vidéo : prix affiché au moment 2, puis « oui » explicite. Ce « oui » couvre la vidéo entière : tous les segments et le montage (estimation faible, comprise dans le total). Repères Seedance pour 15 s : ~105 crédits en 720p, ~180 en 1080p (lus fin septembre et début octobre 2026) ; secours Kling ~30 crédits pour 15 s ; toujours relus avec `get_cost`.
- Sous-titres (workflow `subtitles`, sandbox) : coût inconnu ou faible. Si aucune estimation n'est disponible, l'annoncer comme « estimation inconnue, faible ». Annoncés sans question.
- Relance payante ou bascule de modèle d'une vidéo (voir `references/choix-modele.md#Arbre de décision`) : un `get_cost` à jour et un nouveau « oui », jamais l'ancien prix ni l'ancien accord. Relance d'une image après un échec technique : sans annonce, sous le `Plafond` de l'atelier (`references/choix-modele.md#Échec technique d'une image`). Relance des sous-titres : un `get_cost` à jour et le nouveau prix annoncé en une phrase, sans « oui ».
- Si le solde est inférieur à l'estimation : arrêter, dire « Il manque X crédits. », puis `show_plans_and_credits`. Un sous-agent qui trouve un solde trop bas s'arrête et le signale (atelier ou tournage : `ARRÊT: solde`) ; ce cas est distinct du plafond.
- Consignes d'argent des sous-agents : l'atelier reçoit `Plafond` = prix annoncé + 3 × une image au tarif du modèle d'image le plus cher (la relance muette et les deux rendus du duel d'un échec technique) ; le tournage reçoit `Montant autorisé` = prix affiché au moment 2. Ni l'un ni l'autre ne dépense au-delà.

## Pendant

Après chaque poste payant effectivement soumis : `balance` → ajouter une ligne à `cout.md` :

`| <poste> | <avant> | <après> | <crédits> |`

Un poste par ligne, dans l'ordre où il a été payé. La colonne crédits est la dépense réelle de ce poste (avant − après), pas l'estimation. La ligne est écrite par le sous-agent qui a payé.

## Bilan

Au bilan, une ligne au client : « Cette vidéo a coûté X crédits : images A, vidéo B, sous-titres C. » (postes à zéro omis ; les vues du lieu comptent dans « images »). Le tableau complet reste dans `cout.md`, total = solde de départ − solde final.
