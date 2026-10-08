# Script

> Rôle : la trame montrée au client au moment 2, la liste interne des plans (un plan = un cut) et le gabarit du prompt d'un cut, écrits par le réalisateur.
> Anti-slop : voir `references/anti-slop.md` (prompt modèle) et `references/anti-slop.md#Ton du skill` (texte au client). Conformité : `references/conformite.md#Interdits`. Jeu, répliques et son : `references/jeu.md`. Vues du lieu : `references/vues-lieu.md`.

## Trame

Ce que le client lit au moment 2, sous les images. Une ligne par cut : `Cut <N> · <durée prévue> s · <ce qu'on voit, une phrase>`, puis la réplique entre guillemets s'il parle. La phrase est celle que l'agent principal reprend dans la boucle (`Cut 2/4 : <phrase>`) : courte, en mots de tous les jours, sans la réplique. Ni prompt, ni anglais, ni jargon, ni le pourquoi. Une ligne `Sons :` ferme la trame. Le prix et la question ne font pas partie de la trame : `SKILL.md` les ajoute.
```
Trame (15 s, 4 cuts) :
Cut 1 · 4 s · Au comptoir, Clara demande tout bas au barman d'offrir un verre à l'homme de la terrasse. « Le monsieur, dehors... je peux lui offrir un verre ? »
Cut 2 · 3 s · De près, elle commande en rougissant et paie. « Un spritz, s'il vous plaît. »
Cut 3 · 4 s · À travers la vitre, le barman pose le spritz devant l'homme, qui lève les yeux.
Cut 4 · 4 s · Clara se mord la lèvre quand il lève son verre vers elle.
Sons : les verres qui tintent, le murmure du bar.
```
Durée : la somme des durées prévues est la durée annoncée ; aucune vidéo n'est découpée en parties. Après une correction : seulement les lignes changées, avec leur numéro.

## Plans

Liste interne, en français, jamais montrée au client. Écrite par le réalisateur avant le prompt, après `## Beats` et `## Plan du lieu` (`references/mise-en-scene.md#Dans script.md`), à partir des beats et de la piste choisie dans `## Recherche` de `vision.md` (prémisse, arc, stratégie visuelle, première image). Elle commence par la stratégie visuelle en une phrase, puis une entrée par cut, numérotée en continu sur tout le film :
```
Stratégie : on commence à distance polie, on se rapproche à mesure qu'il devient honnête.
Cut 3 · 4 s · Jeff se relève après la rafale, piolet en appui, et repart.
Pourquoi : la reprise lente dit l'effort ; c'est la bascule, le plan le plus serré.
État : 2e heure de montée, sac de 12 kg, neige à 40°, vent de gauche, souffle court.
Début : à genoux dans la neige, regard sur ses pieds → Fin : debout, deux pas plus haut, regard vers l'arête.
Immobiles : personne.
Objets et son : piolet, main droite, planté à gauche ; crissement de la neige à chaque pas.
```
- `Pourquoi :` ce que le cut raconte (`## Plans qui racontent`). Un cut sans pourquoi est coupé.
- `État :` suit `references/jeu.md#Physique de la situation`. Plan calme : `État : reposé, intérieur calme.` L'état d'un cut part de celui du cut précédent.
- `Début : … → Fin : …`, `Immobiles :`, `Objets et son :` suivent `references/mise-en-scene.md#Blocking et jeu` et `#Registre des objets et du son`. La fin d'un cut est le début du suivant (même main, même objet, même regard) : chaque cut est généré seul, c'est la seule continuité.
- Ce travail reste dans `script.md` : le prompt n'en garde que le résultat (qui fait quoi, où, vers quoi), en quelques mots (`## Version prompt`).
- Puis les vues du lieu, une par position de caméra, dans la limite fixée (`references/vues-lieu.md#Prompt d'une vue`), rangées sous `## Vues du lieu` du `script.md` du projet.

## Plans qui racontent

L'émotion d'abord, la caméra ensuite : fixer à qui appartient la scène et où est la bascule, puis choisir chaque plan pour ce qu'il raconte.

| Plan | Ce qu'il raconte | Exemple |
|---|---|---|
| Insert d'objet | L'état sans un mot | L'enveloppe fermée sous un aimant du frigo |
| Plan de conséquence | L'effet, jamais l'acte | La chaise vide au bureau du collègue parti |
| Plan de réaction | Le visage qui comprend vaut plus que l'information | L'ami qui s'arrête de mâcher |
| Vue subjective | Le spectateur fait le geste lui-même | Ouvrir la boîte aux lettres de l'immeuble |
| Par-dessus l'épaule | Le spectateur est dans la conversation | Par-dessus l'épaule de l'ami, Jeff qui esquive |
| À travers une vitre | On regarde avec le personnage, de loin | Depuis sa table, le barman qui sert en terrasse |
| Légère plongée, puis légère contre-plongée | De vulnérable à sûr de soi | Une étudiante seule à sa table le soir, puis debout |
| Rapprochement lent | Prise de conscience | Pendant le silence de la bascule |
| Recul brusque | Solitude, ou chute comique | On découvre qu'il parle à son chat |
| Mise au point qui bascule | Du petit objet au visage | Les baskets d'enfant, puis le père |
| Vue de dessus | Cadre rare, geste ordinaire | La table du souper, une main qui glisse un papier |
| Verticale du décor | Le 9:16 utilisé | Escalier d'immeuble : il monte, elle descend |

Règles :
- Jamais deux fois le même cadrage. On serre à mesure que ça devient honnête ; le plan le plus marquant va à la bascule. La réaction plutôt que l'action.
- Plans parlés en mi-poitrine ou plus près ; les gros plans vont aux réactions et aux répliques gênées ou intimes.
- Le visage dans le tiers haut, rien d'important en bas du cadre.
- **Coupes motivées** : chaque cut est généré seul, le raccord vient de sa première phrase, qui reprend la fin du cut d'avant en toutes lettres (`## Continuité entre les cuts`).

## Ouverture

Le premier paragraphe du prompt vidéo, écrit par le réalisateur à partir des lignes Style et Lumière de la carte vision validée. Une ou deux phrases : le genre et le ton, la lumière, le lieu en une proposition, le rendu de la caméra, puis `natural, relaxed performances`. Le client ne le voit jamais : la ligne Style de la carte, en mots simples, en est la seule trace.

**Famille**, d'après la ligne Style de la carte :
- Scène de fiction, petite histoire, personnages qui vivent une situation : `cinema`, par défaut.
- `telephone` seulement quand le concept est filmé par quelqu'un de la scène (un ami qui filme, un selfie, un vlog, un téléphone posé qui tourne) : jamais pour une scène où personne ne filmerait.
- `documentaire` : façon reportage, si la carte le dit. `produit` : un objet seul, filmé en studio.

Gabarits, recopiés tels quels, seules les parties entre chevrons changent :

- `cinema` : `Cinematic <genre> short film, <light: hour, colour, source>, <the place in one clause>. Shallow depth of field, smooth handheld camera, natural, relaxed performances.`
- `telephone` : `Vertical phone video filmed by <who films, e.g. her friend across the table>, <light>, <the place in one clause>. Handheld at arm's length with a light natural sway, the phone's own colours, natural, relaxed performances.`
- `documentaire` : `Documentary-style short film, <light>, <the place in one clause>. Camera on the shoulder at a steady distance, focus settling after each move, natural, relaxed performances.`
- `produit` : `Product film, <light>, <the place in one clause>. Deep focus, a slow slider move across the object, crisp true-to-life textures.`

`<genre>` : un ou deux mots qui donnent le ton (`romantic`, `light comedy`, `tender everyday`). Exemple : `Cinematic romantic short film, warm late-afternoon golden light, a cozy neighbourhood bar with a large shopfront window opening onto a sunny terrace. Shallow depth of field, smooth handheld camera, natural, relaxed performances.`

**Enregistrement** : dans `projets/<slug>/script.md`, sous `## Ouverture`, puis recopiée mot pour mot en tête du prompt de chaque cut et de chaque nouvel essai. Changement de décor demandé : seule la proposition du lieu est réécrite. Changement de style demandé : nouvelle famille, nouvelle ouverture. Aucun plan ne nomme un autre appareil ni un autre « look ».

## Version prompt

Gabarit anglais d'un cut, pour Seedance 2.5. Repère : environ 150 à 220 mots par cut, pas une règle. Cinq parties, dans cet ordre, rien d'autre :

1. **L'ouverture** (`## Ouverture`), mot pour mot.
2. **`References:`**, un paragraphe limité aux médias du cut, dans l'ordre de `references/choix-modele.md#Limite de références` : planches, tenues, objets, puis la phrase des deux images du lieu (`references/vues-lieu.md#Dans un cut`), puis, s'il y a un extrait de voix, `@Audio 1 is <Name>'s voice from an earlier cut: keep this voice, not its words.` Planches : `use his face, hair and build only, not the grey clothes, the layout or the background of the sheet`. Tenues : `@Image 2 is Jeff's outfit`. Une tenue sans image : décrite en quelques mots (`the bartender wears a plain black T-shirt and a brown canvas waist apron`). Objet : son nom et ce qu'il est (`@Image 3 is the croissant, a golden butter croissant on a small white plate`).
3. **Un seul plan** (`### Le plan du cut`).
4. **La ligne de son** (`references/jeu.md#Son`).
5. `No subtitles, no text.`

Ce qui n'entre pas dans le prompt : les blocs de description du personnage (pores, pilosité, marques), de la voix, de la bouche, du lieu et des objets, la lumière et la caméra en blocs séparés, le son daté geste par geste. Les visages viennent des planches, le décor des deux images du lieu, les objets de leur planche.

### Le plan du cut

```
Shot (0-<durée prévue>s), the place as in @Image V: <Name>, <2 to 4 visual
words>, <how the cut starts: where she stands, what she holds, where she looks>,
<action, with where she goes and what she touches>, <the emotion, named so it
can be played>. She says to <who> in French, <intention>: "<réplique>"
```

- Le personnage est nommé avec 2 à 4 mots visibles dans chaque cut (`Clara, a young brunette woman`) : le modèle voit chaque cut seul.
- Une action principale, avec son but et son arrivée (`walks back to her small table by the window and sits down`). L'état d'un objet n'est écrit que s'il rend l'action possible (`taps her card on the payment terminal`).
- Le jeu par intention nommée, jamais une liste de micro-gestes : `references/jeu.md#Jouer une intention`.
- Un cut prévu plus court que sa durée générée finit par un geste qui continue (`references/jeu.md#Rythme et dernier plan`) ; sa réplique tient dans la durée prévue.
- Le cut 1 ouvre sur l'accroche choisie (`references/hooks.md#Du hook au premier plan`).
- Une tenue par cut : un personnage à deux tenues change de tenue d'un cut à l'autre, le cut dit laquelle (`in the outfit of @Image 5`).

### Exemple complet

```
Cinematic tender everyday short film, warm early-morning light through the shop window, a small neighbourhood bakery with a long wooden counter. Shallow depth of field, smooth handheld camera, natural, relaxed performances.

References: @Image 1 is Jeff: use his face, hair and build only, not the grey clothes, the layout or the background of the sheet. @Image 2 is Jeff's outfit. @Image 3 is the croissant, a golden butter croissant on a small white plate. @Image 4 shows the whole bakery wide, from far back; @Image 5 is the same bakery from behind the counter. They fix the set and the light only. @Audio 1 is Jeff's voice from an earlier cut: keep this voice, not its words.

Shot (0-3s), the counter as in @Image 5: Jeff, a man in his forties with a short grey beard, still holding the small white plate in his right hand, slides it across the counter toward the customer, a quietly proud smile. He says to her in French, pleased: "Il sort du four." He keeps his hand on the plate a moment longer.

Bakery sounds, the oven door creaking, paper bags rustling, quiet street noise through the door. No subtitles, no text.
```

Environ 200 mots, dans le repère. Prévu à 3 s, généré à 4 s : le dernier geste remplit la seconde coupée au montage.

Avant d'enregistrer le prompt : `references/jeu.md#Contrôle du prompt`, puis relire le prompt contre `references/anti-slop.md#Mots interdits dans les prompts` et chaque réplique contre `references/anti-slop.md#Tics d'écriture à bannir (répliques)` : le contrôleur automatique du plugin ne lit pas l'anglais, cette relecture est manuelle et obligatoire à chaque fois.

## Règles du français

- Tout le prompt est en anglais, sauf les répliques, en français entre guillemets, mot pour mot, jamais reformulées ni traduites.
- La réplique est amenée simplement : `She says to <who> in French, <intention>: "…"`. Juste `in French` : jamais le mot « accent », aucune description de la voix (timbre, volume, débit), aucune phrase sur la bouche. Seule exception : deux ou trois mots de voix dans la présentation du personnage, quand le client demande une voix particulière, ou qu'un personnage parle dans plusieurs cuts sans aucun cut où il parle seul (pas d'extrait possible), ou que son extrait manque (`references/montage.md#Échec`) : les mêmes mots dans chaque cut (`references/fiche-personnage.md#Voix`).
- Registre oral romand, phrases entières et courtes : `references/jeu.md#Réplique en français parlé`.
- Jamais d'orthographe phonétique pour simuler l'accent : ça casse le calage des lèvres.
- Voix off (personne ne parle à l'écran) : `A woman's voice-over says in French, <intention>: "…"` ; seulement si la demande en veut une.
- Réplique hors champ : `<Name>, off screen, says in French: "…"`, et le personnage qu'on voit écoute, bouche fermée.

## Durée et timing

- 15 s par défaut. Une vidéo se fait cut par cut, quelle que soit sa durée. Un cut : 3 à 5 s avec visage et jeu, 3 à 4 s pour un geste (repères). Seedance génère 4 s au minimum (`references/choix-modele.md#Brouillon et finalisation`) : un cut prévu à 2 ou 3 s est généré à 4 s et coupé au montage ; sa réplique tient dans la durée prévue.
- Prompt : environ 150 à 220 mots par cut (repère, pas règle). Répliques : environ 20 à 30 mots dits pour 15 s, environ 6 par réplique à l'écran (`references/mise-en-scene.md#Répliques à l'écran`). Repères, pas plafonds.
- **Fenêtre de parole** : une réplique prend à peu près `mots ÷ 3` secondes ; elle doit tenir dans son cut, un geste remplit le reste. 6 mots ≈ 2 s ; 15 mots ≈ 5 s, donc jamais dans un cut de 4 s. Trop long pour le cut : on raccourcit la réplique ou on allonge le cut, jamais les deux mots qui la font vivre.
- La dernière phrase d'un cut finit avant sa fin prévue, un geste remplit le reste.
- Une action principale par cut : la durée découle des actions (`references/mise-en-scene.md#Blocking et jeu`).

## Continuité entre les cuts

- Chaque cut est une génération à part : rien ne passe d'un cut à l'autre que les références et le texte.
- L'ouverture est identique mot pour mot dans chaque cut ; la ligne de son aussi, pour tous les cuts d'un même lieu. Pas de musique dans un cut : chaque génération jouerait un autre morceau.
- Un personnage est présenté avec les mêmes mots dans chaque cut où il paraît.
- Le début d'un cut reprend en toutes lettres la fin du précédent : objet, main, position, regard (`still holding the small white plate in his right hand`). Jamais « previous cut », jamais une vidéo en référence.
- Voix : dès qu'un cut où un personnage parle seul est gardé, son extrait de voix part dans tous les cuts suivants où il parle (`references/montage.md#Extraire une voix`) ; le bloc du cut le liste dans `Médias :` (`voix:<slug>`) et `References:` porte `@Audio 1 is <Name>'s voice from an earlier cut: keep this voice, not its words.` Le cut d'où vient l'extrait n'en a pas.
- Une réplique ne passe jamais d'un cut à l'autre. L'accroche n'ouvre que le cut 1.

## Retouches

Une seule modification par itération : jamais deux changements en même temps, même si le client en demande plusieurs — proposer de les traiter l'un après l'autre. Noter la correction dans `journal.md` (bibliothèque du client) avant de relancer. Journal : `cut-correction` (correction d'un cut), `script-correction` (trame).

- **Correction d'un cut** : le réalisateur ne réécrit que le bloc de ce cut, en version suivante (`### Cut 3 — v2`), et la ligne de trame si elle change ; `script.md` passe en version suivante. Un cut gardé ou final n'est jamais retouché sans demande, et la `PHRASE` signale une rupture avec lui.
- **Correction de la trame** : seulement les lignes changées et les blocs des cuts ajoutés ou changés.
- Une vue du lieu n'est refaite que si la correction touche ce qu'elle fixe : le lieu, la lumière, le style, ou une position de caméra nouvelle (`references/vues-lieu.md#Corrections`).
