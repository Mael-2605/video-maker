# Coût

> Rôle : la procédure d'argent utilisée à chaque étape payante, et le bilan de fin de projet.

## Avant

- Au début de chaque projet : `balance` → écrire le solde de départ dans `projets/<slug>/cout.md` (gabarit `templates/projet-cout.md`).
- Avant chaque génération payante, quand l'outil accepte `get_cost:true` : l'appeler pour connaître le prix exact.
- Images nouvelles et vues du lieu : prix total dans la ligne `Je fixe` de la carte vision (`get_cost` par élément, par le concepteur ; vues : leur nombre × le prix d'une, ~2 crédits, `references/vues-lieu.md#Nombre et prix`), annoncé sans question ; elles partent à la réponse du client, les vues une fois la trame écrite. Pas de voix générée à part : dès le 2e cut, la voix extraite d'un cut gardé où le personnage parle seul va en `audio_references` (`references/montage.md#Extraire une voix`) ; ni voix de synthèse, ni voix choisie dans une bibliothèque. Une vue refaite ou ajoutée après une correction : son prix en une ligne, sans question. Un plan corrigé ne refait aucune image.
- **Trame** : prix global affiché au moment 2, puis « oui » explicite. Il couvre, pour chaque cut, un brouillon et une finalisation (`references/choix-modele.md#Brouillon et finalisation`), plus l'extraction des voix et le montage (sandbox, estimation inconnue, faible, compris sans ligne à part). Repères : brouillon ~3 crédits par seconde ; 1080p direct ~12 crédits par seconde ; secours Kling ~2 crédits par seconde ; toujours relus avec `get_cost`. Prix d'une finalisation estimé au prix du 1080p direct tant qu'aucun prix réel n'est lu.
- **Nouvel essai d'un cut** (correction ou échec) : son prix (brouillon ; plus la finalisation si le cut était déjà final), affiché, puis « oui ». **Cuts ajoutés ou changés** en cours de route : leur prix, « de plus », puis « oui ». **Reprise** dans une nouvelle conversation : ce qui reste à payer, « pour la suite », puis « oui ».
- Sous-titres (workflow `subtitles`, sandbox) : coût inconnu ou faible. Si aucune estimation n'est disponible, l'annoncer comme « estimation inconnue, faible ». Annoncés sans question.
- Bascule d'un cut vers Kling (voir `references/choix-modele.md#Arbre de décision`) : un `get_cost` à jour et un nouveau « oui », jamais l'ancien prix ni l'ancien accord. Relance d'une image après un échec technique : sans annonce, sous le `Plafond` de l'atelier (`references/choix-modele.md#Échec technique d'une image`). Relance des sous-titres : un `get_cost` à jour et le nouveau prix annoncé en une phrase, sans « oui ».
- Si le solde est inférieur à l'estimation : arrêter, dire « Il manque X crédits. », puis `show_plans_and_credits`. Un sous-agent qui trouve un solde trop bas s'arrête et le signale (atelier ou tournage : `ARRÊT: solde`) ; ce cas est distinct du plafond.
- Consignes d'argent des sous-agents : l'atelier reçoit `Plafond` = prix annoncé + 3 × une image au tarif du modèle d'image le plus cher (la relance muette et les deux rendus du duel d'un échec technique) ; le tournage reçoit `Montant autorisé` = le prix des consignes de cet appel (lignes `Prix :` des blocs de cut de `script.md`, ou le prix d'un nouvel essai annoncé). Ni l'un ni l'autre ne dépense au-delà.

## Pendant

Après chaque poste payant effectivement soumis : `balance` → ajouter une ligne à `cout.md` :

`| <poste> | <avant> | <après> | <crédits> |`

Un poste par ligne, dans l'ordre où il a été payé. La colonne crédits est la dépense réelle de ce poste (avant − après), pas l'estimation. La ligne est écrite par le sous-agent qui a payé.

Postes : `images`, `cut <N> brouillon v<k>`, `cut <N> finalisation`, `voix <personnage>`, `montage`, `sous-titres`. Une ligne par brouillon et par finalisation, écrite par le tournage.

## Bilan

Au bilan, une ligne au client : « Cette vidéo a coûté X crédits : images A, vidéo B, sous-titres C. » (postes à zéro omis ; les vues du lieu comptent dans « images » ; brouillons, finalisations, voix et montage comptent dans « vidéo »). Le tableau complet reste dans `cout.md`, total = solde de départ − solde final.
