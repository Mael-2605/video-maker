# Mise en scène

> Rôle : la méthode du réalisateur avant d'écrire un seul plan. Le look, la lumière et la cohérence d'image tiennent déjà ; ce qui trahit l'IA, c'est la logique : une action sans cause, un objet dans un état impossible, un trajet qui va au mauvais endroit, un personnage qui ne réagit pas, une réplique qu'aucun humain ne dirait. Tout ce travail reste en coulisse, rangé dans `script.md` ; le client ne voit que le découpage (`references/script.md#Découpage`).
> Jeu et voix : `references/jeu.md`. Plans et prompt : `references/script.md`. Vues du lieu : `references/vues-lieu.md`.

## Principe

Un plan sans fonction narrative est supprimé, pas amélioré. Chaque plan répond à une question : qu'est-ce que le spectateur comprend maintenant qu'il ne comprenait pas une seconde avant ? C'est la ligne `Pourquoi :` du plan (`references/script.md#Plans`).

Ordre de travail, toujours le même, même pour une idée simple : beats, plan du lieu, registre des objets et du son, blocking, répliques, auto-contrôle. Le prompt s'écrit ensuite, court (`references/script.md#Version prompt`) : il ne garde que le résultat de ce travail, qui fait quoi, où, vers quoi (`## Dans le prompt`).

## Beats

1. **Actions demandées** : chaque action que décrit `## Demande` de `vision.md`, plus la première image de l'accroche choisie, numérotées. Aucune ne disparaît du script. Si la durée ne permet pas de tout montrer : fusionner deux actions dans un plan, ou passer l'une en conséquence visible (on voit le verre déjà posé plutôt que le trajet). Si rien ne fusionne, garder toutes les actions quand même, serrer les plans, et la `PHRASE` du retour propose une durée plus longue au client en une phrase. Jamais de beat supprimé en silence.
2. **Chaîne de causalité** : pour chaque beat, `cause → action visible → conséquence visible`. Un maillon ni visible ni audible, le spectateur ne le comprend pas : on l'ajoute à l'image (un geste, un regard, un objet) ou on change le beat.
3. **Beats implicites** : les étapes que la demande ne dit pas mais que la logique impose. Une demande se formule et désigne sa cible (un mouvement de menton, un doigt, un regard). Un cadeau s'attribue (« de la part de… », ou un geste vers la personne qui l'offre). Une personne qui reçoit quelque chose réagit. Une observation à distance a une conséquence : on est vu, ou pas, et le plan le montre.
4. **Règle de la réaction** : tout événement dirigé vers un personnage (objet, regard, parole, contact) déclenche un beat de réaction chez lui, écrit en micro-actions physiques, dans l'ordre : il regarde l'objet → lève les yeux → cherche d'où ça vient → trouve → réagit. La réaction peut tenir dans le plan suivant ; elle ne disparaît pas.
5. **Test du spectateur muet** : relire les beats en imaginant la vidéo sans le son et sans le prompt. Un beat qu'on ne comprend pas manque d'une action, d'un geste qui désigne ou d'un regard.

## Plan du lieu

La géographie se fixe avant les plans et ne bouge plus, comme une carte qu'un inconnu pourrait suivre.

- **Positions** : chaque élément clé (comptoir, porte, fenêtre, tables, terrasse, bureau) et chaque personnage, en repères stables : mur gauche ou droit, avant ou fond, côté rue ou côté cour.
- **Règle d'orientation**, en une phrase valable dans tous les plans : où est la caméra par rapport à l'axe principal du lieu, et de quel côté du cadre on va vers la sortie. Exemple : « Caméra côté salle, comptoir à gauche, porte vitrée au fond à droite : sortir, c'est aller vers la droite du cadre. »
- **Trajets** : toujours `point de départ → direction → repère d'arrivée`. Le repère d'arrivée (porte, table, personne) est dans le cadre, du côté où va le personnage. Personne ne se tient sur la trajectoire d'un autre, sauf si c'est sa destination : sinon le modèle comprend que l'objet va vers lui.
- Les positions de caméra des plans donnent les vues du lieu à faire, une par position (`references/vues-lieu.md#Prompt d'une vue`).

## Registre des objets et du son

Pour chaque accessoire, plan par plan : son état, qui le tient, de quelle main, où il est.

- **L'état rend l'action possible.** Un paiement : terminal allumé qui affiche un montant, téléphone allumé qui affiche une carte, puis l'écran de confirmation. Une lettre qu'on lit : dépliée, face au lecteur. Jamais « écran éteint » sur un objet qui sert à l'action.
- **Son et image** : chaque son a une source visible, dans le bon état, au bon moment. Pas de bip si rien ne s'allume ; pas de pas si personne ne marche ; pas de tasse sur la soucoupe si personne ne la pose. La ligne de son ne nomme que des sources du lieu ou du registre (`references/jeu.md#Son`).
- **Persistance** : un objet qui doit rester reste visible et non masqué dans le plan. S'il doit disparaître puis revenir, la disparition se fait sur une coupe.
- **Remise de main à main** : sur une coupe. Fin du plan A, l'objet est tendu ; début du plan B, il est déjà tenu par l'autre main.

## Blocking et jeu

Pour chaque plan :

- **Début → une action principale → fin**, avec une pose finale écrite. « Il marche vers la porte » n'est pas un plan ; « il franchit la porte vitrée et s'arrête à la table de Matteo » en est un. La pose finale du plan N est la pose de départ du plan N+1 (position, objet, main, regard).
- **Un seul personnage moteur.** Les autres ont une consigne d'immobilité écrite : « elle ne bouge pas, accoudée au comptoir ».
- **Un seul mouvement de caméra**, ou aucun.
- **Regards** : où regarde chacun au début, à la fin, et à quel moment ça change.
- **Émotion jouable** : ce que ressent le personnage, nommé avec précision et accroché à l'action (`he looks up, surprised`, `slightly embarrassed, she glances toward the window`), jamais un état vague ni une liste de micro-gestes (`references/jeu.md#Jouer une intention`).
- **Durée** : elle découle des actions, pas l'inverse. Repères : 3 à 5 s pour un plan avec visage et jeu, 3 à 4 s pour un geste de main ; nombre de plans environ la durée ÷ 3,5 (4 plans pour 15 s, 8 ou 9 pour 30 s). Ce sont des repères, pas des règles : une demande riche en texte ou en actions peut demander des plans plus longs ou plus nombreux ; on s'en écarte quand l'histoire l'exige, jamais pour remplir.

## Répliques à l'écran

Elles suivent `references/jeu.md#Réplique en français parlé`. En plus :

- Écrire ce qu'une vraie personne dirait à voix haute dans cette situation précise, en réaction à ce qu'elle voit à cet instant. Relire à voix haute : une réplique qui sonne écrite, télégraphique ou ambiguë est réécrite.
- **Bouche visible** : 6 mots environ par réplique, un seul personnage parle par plan, bouche visible en plan rapproché. Repère par défaut, pas plafond : une demande qui porte beaucoup de texte peut aller au-delà, on garde alors la fenêtre de parole de `references/script.md#Durée et timing`.
- **Réplique longue ou deuxième voix** : hors champ, sur un plan d'écoute ou de réaction ; celui qu'on voit garde la bouche fermée. Forme encore à confirmer au test réel.
- **Variantes** : pour la réplique clé (l'accroche, ou la bascule), écrire 2 ou 3 variantes dans `## Beats` de `script.md`, garder la meilleure dans les plans et dire pourquoi en une ligne. Le client ne les voit pas ; elles servent si la réplique est corrigée.

## Dans script.md

Avant `## Plans` du `script.md` du projet, deux sections internes, jamais montrées au client :

```
## Beats
Actions demandées : 1. … 2. … 3. …
1. <cause> → <action visible> → <conséquence visible>
2. (implicite) <cause> → … → …
Réplique clé : A « … » (gardée : <raison courte>) · B « … » · C « … »

## Plan du lieu
<positions, en repères stables>
Orientation : <une phrase>
Trajets : <départ> → <direction> → <repère d'arrivée>
```

Puis, dans chaque plan de `## Plans`, en plus de `Pourquoi :` et `État :` (`references/script.md#Plans`) :

```
Début : <pose et regard de chacun> → Fin : <pose finale, regard>
Immobiles : <qui ne bouge pas, et comment>
Objets et son : <objet : état, qui, quelle main, où> ; <son : sa source visible>
```

Un plan calme sans objet ni second personnage : `Immobiles : personne.` et `Objets et son : rien.`

## Dans le prompt

Beats, plan du lieu, registre et blocking restent dans `script.md`. Le prompt n'en garde que le résultat, en quelques mots par plan (`references/script.md#Un plan`) :

- **Où** : le plan qui a sa vue la cite en tête (`the counter as in @Image 7`) ; l'ouverture donne le lieu en une proposition qui porte déjà l'orientation utile (`a large shopfront window opening onto a sunny terrace`). Pas de phrase de géographie à part.
- **Qui va où** : chaque trajet nomme son arrivée (`walks back to her small table by the window and sits down`, `crosses the terrace and sets the spritz in front of the man`). Un immobile n'est écrit que s'il risque de bouger (`the man stays seated at his table`).
- **Objets** : leur état n'est écrit que s'il rend l'action possible (`taps her card on the payment terminal`). Une remise d'objet se fait sur une coupe.
- **Réactions** : nommées, dans le plan ou le suivant (`He looks up, surprised.`).
- **Réplique hors champ** : `<Name>, off screen, says in French: "…"`, et le personnage visible écoute, bouche fermée.
- **Sons** : pas de son de geste listé ; la ligne de son donne seulement l'ambiance du lieu (`references/jeu.md#Son`).

## Auto-contrôle

Avant de rendre le découpage, cocher chaque point ; un point manque, corriger d'abord.

- [ ] Toutes les actions demandées sont là ; aucune supprimée en silence ; si elles ne tiennent pas, la `PHRASE` propose une durée plus longue.
- [ ] Chaque beat a une cause et une conséquence visibles (test du spectateur muet).
- [ ] Chaque événement dirigé vers quelqu'un a sa réaction, en micro-actions dans l'ordre.
- [ ] Fin du plan N = début du plan N+1 : position, objet, main, regard.
- [ ] Aucun objet dans un état incompatible avec son usage ; chaque son a une source visible et active.
- [ ] Chaque trajet a un repère d'arrivée visible et va dans le sens de la règle d'orientation ; personne sur la trajectoire d'un autre sans raison.
- [ ] Un personnage moteur, un mouvement de caméra au plus, une pose finale écrite, par plan.
- [ ] Les répliques sonnent dites et non écrites ; environ 6 mots à l'écran, sauf demande qui exige plus ; chacune tient dans sa fenêtre de parole (`mots ÷ 3` secondes).
- [ ] La somme des durées des plans = la durée annoncée.

## Exemple — le verre offert

Demande : une jeune femme brune, dans un bar, demande au barman d'envoyer un verre à un beau brun assis en terrasse. Elle commande un tricycle, paie, retourne à sa table et observe de loin le barman apporter le verre.

Ce qui n'allait pas dans une première version : elle payait en tapant un téléphone éteint sur un terminal éteint, avec un bip ; elle ne désignait jamais le garçon, donc rien ne disait qu'il s'agissait d'un cadeau ; elle se tenait sur la trajectoire du plateau, qui semblait partir vers elle ; Matteo recevait le verre sans savoir d'où il venait ; la dernière réplique, « Il a vu ? », sonnait faux.

Version corrigée, 20 s, 6 plans. Plan du lieu : comptoir le long du mur gauche, porte vitrée au fond à droite, la table de Lou contre la vitrine au fond à gauche, terrasse derrière la vitrine ; sortir, c'est aller vers la droite du cadre.

| # | Durée | Ce qu'on voit | Objets et son | Réplique |
|---|---|---|---|---|
| 1 | 3,5 s | Comptoir. Lou, accoudée, désigne la terrasse d'un mouvement du menton ; Sami suit son regard, puis hoche la tête. | — | Lou : « Tu lui amènes un tricycle, au gars là-bas ? » |
| 2 | 3 s | Vue de dessus du comptoir : Sami pose le verre sur le plateau ; Lou approche son téléphone allumé (carte affichée) du terminal allumé qui affiche le montant ; coche verte. | Bip quand la coche apparaît | — |
| 3 | 3,5 s | Derrière Sami : il sort par la porte vitrée, devant lui à droite, vers la terrasse. Au fond à gauche, Lou regagne sa table près de la vitrine, hors de la trajectoire. | Glaçons qui tintent, pas sur le carrelage | — |
| 4 | 3,5 s | Terrasse : Sami pose le plateau devant Matteo et, sans parler, désigne la vitrine de la main ouverte. | Plateau sur la table en métal | — |
| 5 | 3,5 s | Matteo regarde le verre, puis tourne la tête vers la vitrine ; petit sourire étonné ; il lève un peu le verre vers elle. | — | — |
| 6 | 3 s | Intérieur, la table de Lou : Sami, revenu, attend à côté d'elle sans bouger, plateau vide sous le bras. Lou voit le geste de Matteo et remonte la carte des boissons jusqu'au nez, les yeux qui dépassent. | Froissement de la carte | Lou, à mi-voix, à Sami : « Tu lui as dit que c'était moi ? » |

Variantes de la réplique finale, toujours à Sami : « Il a dit quoi, quand tu lui as posé le verre ? » (si on coupe avant la réaction de Matteo) · « T'avais besoin de me montrer du doigt ? »
