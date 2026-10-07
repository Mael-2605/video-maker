# Script

> Rôle : le format du découpage montré au client au moment 2, la liste interne des plans et le gabarit du prompt vidéo, écrits par le réalisateur.
> Anti-slop : voir `references/anti-slop.md` (prompt modèle) et `references/anti-slop.md#Ton du skill` (texte au client). Conformité : `references/conformite.md#Interdits`. Jeu, répliques et son : `references/jeu.md`. Vues du lieu : `references/vues-lieu.md`.

## Découpage

Ce que le client lit au moment 2, sous les images. Une phrase par plan : timecode, ce qu'on voit (action, geste, ce que ressent le personnage), comment c'est filmé, en mots de tous les jours, puis la réplique s'il parle. Les images sont montrées au-dessus ; le découpage ne les décrit pas. Ni prompt, ni anglais, ni jargon (`references/anti-slop.md#Ton du skill`), ni le pourquoi des plans. Une ligne `Sons :` ferme le découpage, du plus proche au plus lointain, avec la musique s'il y en a. Le prix et la question ne font pas partie du découpage : `SKILL.md` les ajoute.
```
Découpage (15 s) :
1. 0–5 s · Au comptoir, Clara s'approche un peu hésitante et jette un œil vers la vitrine, où un bel homme brun est assis seul en terrasse. Elle dit au barman, tout bas, avec un petit rire gêné : « Le monsieur, dehors... je peux lui offrir un verre ? »
2. 5–8 s · De près, les joues qui rougissent, elle évite son regard : « Un spritz, s'il vous plaît. » Elle paie avec sa carte ; le barman hoche la tête, un sourire en coin.
3. 8–10 s · Elle retourne à sa petite table contre la vitrine et s'assied, un œil dehors.
4. 10–13 s · À travers la vitre, depuis sa place : le barman pose le spritz devant l'homme et montre le bar d'un geste. Il lève les yeux, surpris.
5. 13–15 s · De près, Clara se mord la lèvre avec un petit sourire amusé quand il lève son verre vers elle.
Sons : les verres qui tintent, le murmure du bar, une musique lounge discrète.
```
Film en segments (`## Segments`) : titre `Découpage (1 min, en 2 parties assemblées) :`, plans numérotés en continu sur tout le film, timecodes du film (`0:30–0:36`), rien qui signale la coupure entre deux parties. Après une correction : seulement les lignes changées, avec leur numéro.

## Plans

Liste interne, en français, jamais montrée au client. Écrite par le réalisateur avant le prompt, après `## Beats` et `## Plan du lieu` (`references/mise-en-scene.md#Dans script.md`), à partir des beats et de la piste choisie dans `## Recherche` de `vision.md` (prémisse, arc, stratégie visuelle, première image). Elle commence par la stratégie visuelle en une phrase, puis une entrée par plan, numérotée en continu sur tout le film :
```
Stratégie : on commence à distance polie, on se rapproche à mesure qu'il devient honnête.
Plan 3 · 0:12–0:18 · Jeff se relève après la rafale, piolet en appui, et repart.
Pourquoi : la reprise lente dit l'effort ; c'est la bascule, le plan le plus serré.
État : 2e heure de montée, sac de 12 kg, neige à 40°, vent de gauche, souffle court.
Début : à genoux dans la neige, regard sur ses pieds → Fin : debout, deux pas plus haut, regard vers l'arête.
Immobiles : personne.
Objets et son : piolet, main droite, planté à gauche ; crissement de la neige à chaque pas.
```
- `Pourquoi :` ce que le plan raconte (`## Plans qui racontent`). Un plan sans pourquoi est coupé.
- `État :` suit `references/jeu.md#Physique de la situation`. Plan calme : `État : reposé, intérieur calme.` L'état d'un plan part de celui du plan précédent.
- `Début : … → Fin : …`, `Immobiles :`, `Objets et son :` suivent `references/mise-en-scene.md#Blocking et jeu` et `#Registre des objets et du son`. La fin d'un plan est le début du suivant.
- Ce travail reste dans `script.md` : le prompt n'en garde que le résultat (qui fait quoi, où, vers quoi), en quelques mots par plan (`## Version prompt`).
- Puis les vues du lieu, une par position de caméra, dans la limite fixée : `references/vues-lieu.md#Prompt d'une vue`, rangées sous `## Vues du lieu` du `script.md` du projet.

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
- **Coupes motivées** : le raccord s'écrit dans les premiers mots du plan suivant (`Continuing the same motion, …`, `Through the window, from her seat …`).
- **Expérimental, pas utilisé** : la voix du plan suivant qui démarre avant la coupe, deux voix qui se chevauchent.

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

**Enregistrement** : dans `projets/<slug>/script.md`, sous `## Ouverture`, puis recopiée mot pour mot en tête de chaque prompt : chaque segment, chaque nouvelle version après une correction. Changement de décor demandé : seule la proposition du lieu est réécrite. Changement de style demandé : nouvelle famille, nouvelle ouverture. Aucun plan ne nomme un autre appareil ni un autre « look ».

## Version prompt

Gabarit anglais pour Seedance 2.5. Repère : environ 200 à 350 mots pour un clip de 15 s, pas un plafond. Cinq parties, dans cet ordre, rien d'autre :

1. **L'ouverture** (`## Ouverture`), mot pour mot.
2. **`References:`**, un paragraphe : une courte proposition par média, dans l'ordre de `references/choix-modele.md#Route Seedance`. Planches : `use their faces, hair and build only, not the grey clothes, the layout or the backgrounds of the sheets`. Tenues : `@Image 4 is Clara's outfit`. Une tenue sans image : décrite en quelques mots dans ce paragraphe (`the bartender wears a plain black T-shirt and a brown canvas waist apron`). Objet : son nom et ce qu'il est (`@Image 6 is the spritz, an orange aperitif in a stemmed wine glass`). Vues : une phrase pour toutes (`references/vues-lieu.md#Dans la vidéo`).
3. **Les plans**, un paragraphe chacun : `Shot <N> (<a>-<b>s)`, l'endroit s'il a sa vue (`the counter as in @Image 7`), puis l'action, l'émotion nommée, la réplique (`## Un plan`).
4. **Le son**, une ligne (`references/jeu.md#Son`).
5. La phrase de fin : `No subtitles, no text.`

Ce qui n'entre pas dans le prompt vidéo : les blocs de description du personnage (pores, pilosité, marques), de la voix, de la bouche, du lieu et des objets, la lumière et la caméra en blocs séparés, le son daté geste par geste. Les visages viennent des planches, le décor des vues, les objets de leur planche.

### Un plan

```
Shot <N> (<a>-<b>s)<, the place as in @Image V>: <Name>, <2 to 4 visual words
the first time only>, <action, with where she goes and what she touches>, <the
emotion, named so it can be played>. She says to <who> in French, <intention>:
"<réplique>"
```

- Le personnage est nommé avec 2 à 4 mots visibles la première fois (`Clara, a young brunette woman`), ensuite par son prénom seul.
- Une action principale par plan, avec son but et son arrivée (`walks back to her small table by the window and sits down`). L'état d'un objet n'est écrit que s'il rend l'action possible (`taps her card on the payment terminal`).
- Le jeu par intention nommée, jamais une liste de micro-gestes : `references/jeu.md#Jouer une intention`.
- La caméra seulement quand elle change ou qu'elle raconte : `Close-up at the counter.`, `Through the window, from her seat as in @Image 8:`.
- Le premier plan ouvre sur l'accroche choisie (`references/hooks.md#Du hook au premier plan`). Le dernier plan garde du mouvement jusqu'à la fin (`references/jeu.md#Rythme et dernier plan`).
- **Plan continu** (une seule prise) : `One continuous take, …`, puis des repères dans le texte : `From 0 to 4 seconds, …`, `From 4 to 9 seconds, …`.
- Tenues : un personnage à deux tenues, le plan dit laquelle (`now in the outfit of @Image 5`) ; le changement se fait sur une coupe.

### Exemple complet

```
Cinematic romantic short film, warm late-afternoon golden light, a cozy neighbourhood bar with a large shopfront window opening onto a sunny terrace. Shallow depth of field, smooth handheld camera, natural, relaxed performances.

References: @Image 1 is Clara, @Image 2 is the bartender, @Image 3 is the man on the terrace: use their faces, hair and build only, not the grey clothes, the layout or the backgrounds of the sheets. @Image 4 is Clara's outfit, @Image 5 the man's outfit; the bartender wears a plain black T-shirt and a brown canvas waist apron. @Image 6 is the spritz, an orange aperitif in a stemmed wine glass. @Image 7, @Image 8 and @Image 9 are the same bar seen from three places: @Image 7 from behind the counter toward the window, @Image 8 from the small table against the window looking out at the terrace, @Image 9 from the terrace looking at the facade. They fix the set and the light only.

Shot 1 (0-5s), the counter as in @Image 7: Clara, a young brunette woman, walks up to the counter a little hesitantly, leans in and, slightly embarrassed, glances toward the window, where a handsome dark-haired man sits alone at a terrace table. She says to the bartender in French, in a low voice, with a small nervous laugh: "Le monsieur, dehors... je peux lui offrir un verre ?"
Shot 2 (5-8s): Close-up at the counter. Her cheeks flushing, she avoids the bartender's eyes and says quietly in French: "Un spritz, s'il vous plaît." She taps her card on the payment terminal; the bartender nods with a knowing grin, and she bites back a shy smile.
Shot 3 (8-10s): She walks back to her small table by the window and sits down, glancing outside.
Shot 4 (10-13s): Through the window, from her seat as in @Image 8: the bartender crosses the terrace and sets the orange spritz in front of the man, gesturing back toward the bar. He looks up, surprised.
Shot 5 (13-15s): Close-up on Clara at her table by the window, watching, biting her lip with a shy, amused smile as he raises his glass toward her.

Ambient bar sounds, clinking glasses, soft murmur, quiet lounge music in the background. No subtitles, no text.
```

Environ 370 mots, un peu au-dessus du repère : trois personnages et trois vues à présenter. Neuf images : trois planches, deux tenues (celle du barman passe en mots), l'objet, trois vues. C'est ce prompt qui a battu la version longue au test réel du 2026-10-03.

Avant d'enregistrer le prompt : `references/jeu.md#Contrôle du prompt`, puis relire le prompt contre `references/anti-slop.md#Mots interdits dans les prompts` et chaque réplique contre `references/anti-slop.md#Tics d'écriture à bannir (répliques)` : le contrôleur automatique du plugin ne lit pas l'anglais, cette relecture est manuelle et obligatoire à chaque fois.

## Règles du français

- Tout le prompt est en anglais, sauf les répliques, en français entre guillemets, mot pour mot, jamais reformulées ni traduites.
- La réplique est amenée simplement : `She says to <who> in French, <intention>: "…"`. Juste `in French` : jamais le mot « accent », aucune description de la voix (timbre, volume, débit), aucune phrase sur la bouche. Seule exception : deux ou trois mots de voix dans la présentation du personnage, quand le client demande une voix particulière ou que le film a plusieurs segments (`references/fiche-personnage.md#Voix`).
- Registre oral romand, phrases entières et courtes : `references/jeu.md#Réplique en français parlé`.
- Jamais d'orthographe phonétique pour simuler l'accent : ça casse le calage des lèvres.
- Voix off (personne ne parle à l'écran) : `A woman's voice-over says in French, <intention>: "…"` ; seulement si la demande en veut une.
- Réplique hors champ : `<Name>, off screen, says in French: "…"`, et le personnage qu'on voit écoute, bouche fermée.

## Durée et timing

- 15 s par défaut. 30 s ou moins : une seule génération, jamais découpée, jamais de plan testé à part. Au-delà : `## Segments`.
- Prompt : environ 200 à 350 mots pour 15 s, environ le double pour 30 s. Repère : une demande riche en actions peut aller au-delà ; un prompt qui dépasse de beaucoup se raccourcit d'abord dans les plans muets.
- Répliques : environ 20 à 30 mots dits pour 15 s, environ 6 par réplique à l'écran (`references/mise-en-scene.md#Répliques à l'écran`). Repères, pas plafonds.
- **Fenêtre de parole** : une réplique prend à peu près `mots ÷ 3` secondes ; elle doit tenir dans son plan, un geste remplit le reste. 6 mots ≈ 2 s ; 15 mots ≈ 5 s, donc jamais dans un plan de 4 s. Trop long pour le plan : on raccourcit la réplique ou on allonge le plan, jamais les deux mots qui la font vivre.
- La dernière phrase se termine au moins 1 seconde avant la fin du clip.
- Une action principale par plan ; 3 à 5 s pour un plan avec visage et jeu, 2 à 3 s pour un passage muet (elle regagne sa table). Repères : la durée découle des actions (`references/mise-en-scene.md#Blocking et jeu`).

## Segments

Au-delà de 30 s, le film est fait de N segments générés à part puis assemblés (`references/choix-modele.md#Film en segments`).
- N = durée ÷ 30, arrondi au-dessus ; segments de même durée, en secondes entières, 4 à 30 s chacun (45 s → 23 + 22 ; 1 min → 30 + 30).
- Chaque segment a son prompt complet, sous `### Segment k` dans `## Version prompt`, avec des timecodes qui repartent de 0. L'ouverture et le paragraphe `References:` sont identiques d'un segment à l'autre, mot pour mot ; mêmes médias, dans le même ordre.
- Sans extrait de voix, la voix d'un personnage peut changer d'un segment à l'autre : un personnage qui parle dans plusieurs segments est présenté avec les mêmes mots dans chacun, plus deux ou trois mots sur sa voix (`Clara, a young brunette woman with a soft, warm voice`), identiques partout.
- Le dernier plan d'un segment finit sur une position exacte, écrite en toutes lettres. Le plan 1 du segment suivant part de cette position : `Continues from the previous segment: <position exacte>.`
- Aucune réplique ne passe d'un segment à l'autre ; la dernière phrase d'un segment finit au moins 1 s avant sa fin.
- L'accroche n'ouvre que le segment 1.

## Retouches

Une seule modification par itération : jamais deux changements en même temps, même si le client en demande plusieurs — proposer de les traiter l'un après l'autre. Noter la correction dans `journal.md` (bibliothèque du client) avant de relancer. Au moment 2 ou après la vidéo : le réalisateur réécrit `script.md` (v2, v3…) et ne renvoie que les lignes du découpage qui changent. Une vue du lieu n'est refaite que si la correction touche ce qu'elle fixe : le lieu, la lumière, le style, ou une position de caméra nouvelle (`references/vues-lieu.md#Corrections`).
