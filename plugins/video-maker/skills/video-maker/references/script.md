# Script

> Rôle : la trame montrée au client au moment 2, la liste interne des plans (un plan = un cut), leur regroupement en clips (un clip = une génération) et le gabarit du prompt d'un clip, écrits par le réalisateur.
> Anti-slop : voir `references/anti-slop.md` (prompt modèle) et `references/anti-slop.md#Ton du skill` (texte au client). Conformité : `references/conformite.md#Interdits`. Jeu, répliques et son : `references/jeu.md`. Vues du lieu : `references/vues-lieu.md`.

## Trame

Ce que le client lit au moment 2, sous les images. Une ligne par cut, rangée par clip (`## Clips`) : une ligne `Clip <N> · <durée> s` ouvre chaque clip, puis ses cuts, `Cut <N> · <durée prévue> s · <ce qu'on voit, une phrase>`, puis la réplique entre guillemets s'il parle. La phrase est celle que l'agent principal reprend dans la boucle (`Clip 2/3 : <phrases de ses cuts>`) : courte, en mots de tous les jours, sans la réplique. Ni prompt, ni anglais, ni jargon, ni le pourquoi. Une ligne `Sons :` ferme la trame. Le prix et la question ne font pas partie de la trame : `SKILL.md` les ajoute.
```
Trame (15 s, 5 cuts en 3 clips) :
Clip 1 · 4 s
Cut 1 · 4 s · Au comptoir, Clara demande tout bas au barman d'offrir un verre à l'homme de la terrasse. « Le monsieur, dehors... je peux lui offrir un verre ? »
Clip 2 · 5 s
Cut 2 · 3 s · De près, elle commande en rougissant et paie. « Un spritz, s'il vous plaît. »
Cut 3 · 2 s · Le barman lui fait un clin d'œil en posant le verre sur son plateau.
Clip 3 · 6 s
Cut 4 · 3 s · À travers la vitre, il pose le spritz devant l'homme, qui lève les yeux.
Cut 5 · 3 s · Clara se mord la lèvre quand il lève son verre vers elle.
Sons : les verres qui tintent, le murmure du bar.
```
Durée : la somme des durées prévues est la durée annoncée ; celle d'un clip, la somme de ses cuts ; aucune vidéo n'est découpée en parties. Après une correction : seulement les lignes changées, avec leur numéro, sous la ligne de leur clip.

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
- `Début : … → Fin : …`, `Immobiles :`, `Objets et son :` suivent `references/mise-en-scene.md#Blocking et jeu` et `#Registre des objets et du son`. La fin d'un cut est le début du suivant (même main, même objet, même regard) : d'un clip à l'autre, générés à part, c'est la seule continuité.
- Ce travail reste dans `script.md` : le prompt n'en garde que le résultat (qui fait quoi, où, vers quoi), en quelques mots (`## Version prompt`).
- Puis les vues du lieu, une par position de caméra, dans la limite fixée (`references/vues-lieu.md#Prompt d'une vue`), rangées sous `## Vues du lieu` du `script.md` du projet.
- Puis le regroupement des cuts en clips (`## Clips`).

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
- **Coupes motivées** : d'un clip à l'autre, le raccord vient du premier plan du clip, qui reprend la fin du clip d'avant en toutes lettres (`## Continuité entre les cuts`). Dans un clip, la coupe est écrite (`Hard cut to:`) entre deux cadrages nettement différents.

## Clips

Seedance génère 4 s au moins (`references/choix-modele.md#Brouillon et finalisation`). Un clip est une génération : il contient un ou plusieurs cuts qui se suivent et dure 4 s au moins. Le brouillon, la finalisation, le « On garde ? », la correction, le prix, le statut et l'extrait de voix se font par clip ; le client lit toujours des cuts, rangés par clip dans la trame.

Le réalisateur regroupe après `## Plans` et les vues, dans `## Clips` du `script.md` du projet, une ligne par clip :
```
Clip 1 · cuts 1 · 4 s
Clip 2 · cuts 2-3 · 5 s
Clip 3 · cuts 4-5 · 6 s
```
1. Les cuts dans l'ordre de la trame. Un clip se ferme dès que la somme des durées prévues de ses cuts atteint 4 s.
2. Un cut de 4 s ou plus qui ouvre un clip le ferme aussitôt : il forme son propre clip. Un cut court suivi d'un cut de 4 s ou plus est regroupé avec lui (point 1).
3. Un cut n'est jamais coupé entre deux clips.
4. Des cuts qui restent à la fin pour moins de 4 s rejoignent le clip d'avant.
5. Lieux : un clip peut changer de lieu ou de position de caméra si ses images tiennent dans la limite de 9 (`references/choix-modele.md#Limite de références` : image de base et vue de chaque lieu ou position du clip). Sinon le cut court est allongé à 4 s et forme son propre clip.
6. Deux cuts d'un même clip ont des cadrages nettement différents (distance ou angle) : sinon Seedance fond les deux plans en un seul mouvement au lieu de couper.
7. Durée générée d'un clip = somme des durées prévues de ses cuts, en secondes entières, 4 s au moins. Le montage coupe chaque clip à cette somme, rien à l'intérieur (`references/montage.md#Assembler les cuts`).

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

**Enregistrement** : dans `projets/<slug>/script.md`, sous `## Ouverture`, puis recopiée mot pour mot en tête du prompt de chaque clip et de chaque nouvel essai. Changement de décor demandé : seule la proposition du lieu est réécrite. Changement de style demandé : nouvelle famille, nouvelle ouverture. Aucun plan ne nomme un autre appareil ni un autre « look ».

## Version prompt

Gabarit anglais d'un clip, pour Seedance 2.5. Repère : environ 150 à 220 mots pour un clip d'un cut, environ 50 à 80 mots par cut en plus ; pas une règle. Cinq parties, dans cet ordre, rien d'autre :

1. **L'ouverture** (`## Ouverture`), mot pour mot.
2. **`References:`**, un paragraphe limité aux médias du clip, dans l'ordre de `references/choix-modele.md#Limite de références` : planches, tenues, objets, puis la phrase des images du lieu (`references/vues-lieu.md#Dans un cut`), puis, s'il y a un extrait de voix, `@Audio 1 is <Name>'s voice from an earlier cut: keep this voice, not its words.` Planches : `use his face, hair and build only, not the grey clothes, the layout or the background of the sheet`. Tenues : `@Image 2 is Jeff's outfit`. Une tenue sans image : décrite en quelques mots (`the bartender wears a plain black T-shirt and a brown canvas waist apron`). Objet : son nom et ce qu'il est (`@Image 3 is the croissant, a golden butter croissant on a small white plate`).
3. **Un plan par cut**, `Hard cut to:` entre deux plans (`### Le plan du cut`).
4. **La ligne de son** (`references/jeu.md#Son`).
5. `No subtitles, no text.`

Ce qui n'entre pas dans le prompt : les blocs de description du personnage (pores, pilosité, marques), de la voix, de la bouche, du lieu et des objets, la lumière et la caméra en blocs séparés, le son daté geste par geste. Les visages viennent des planches, le décor des deux images du lieu, les objets de leur planche.

### Le plan du cut

Un plan par cut. Clip d'un seul cut :
```
Shot (0-<durée prévue>s), the place as in @Image V: <Name>, <2 to 4 visual
words>, <how the cut starts: where she stands, what she holds, where she looks>,
<action, with where she goes and what she touches>, <the emotion, named so it
can be played>. She says to <who> in French, <intention>: "<réplique>"
```

Clip de plusieurs cuts, le temps de chaque plan compté depuis le début du clip :
```
Shot 1 (0-<a>s), <framing>, the place as in @Image V: <Name>, <2 to 4 visual
words>, <how the clip starts>, <action>, <the emotion>. She says to <who> in
French, <intention>: "<réplique>"
Hard cut to:
Shot 2 (<a>-<b>s), <another framing>, the place as in @Image W: <Name>,
<action>, <the emotion>.
```

- `Hard cut to:` seul sur sa ligne, entre chaque paire de plans : Seedance le lit à la lettre et coupe. Jamais de fondu ni de transition.
- Chaque plan d'un clip de plusieurs cuts dit son cadrage en tête (`medium shot`, `close-up`, `over his shoulder`, `through the window`), nettement différent du plan d'avant : distance ou angle (`## Clips`, point 6).
- Le personnage est présenté avec 2 à 4 mots visibles au premier plan où il paraît dans le clip (`Clara, a young brunette woman`), les mêmes dans chaque clip : le modèle voit chaque clip seul. Ensuite, dans le clip, son nom suffit.
- Le premier plan du clip reprend la fin du clip d'avant (`## Continuité entre les cuts`) ; dans un clip, le plan suivant n'a pas à la redire.
- Une action principale par plan, avec son but et son arrivée (`walks back to her small table by the window and sits down`). L'état d'un objet n'est écrit que s'il rend l'action possible (`taps her card on the payment terminal`).
- Le jeu par intention nommée, jamais une liste de micro-gestes : `references/jeu.md#Jouer une intention`.
- Une réplique tient dans son plan ; elle ne passe jamais d'un plan à l'autre. Le dernier plan du clip finit par un geste qui continue (`references/jeu.md#Rythme et dernier plan`).
- Le cut 1 ouvre sur l'accroche choisie (`references/hooks.md#Du hook au premier plan`).
- Une tenue par cut : un personnage à deux tenues change de tenue d'un cut à l'autre, le plan dit laquelle (`in the outfit of @Image 5`).

### Exemple complet

```
Cinematic tender everyday short film, warm early-morning light through the shop window, a small neighbourhood bakery with a long wooden counter. Shallow depth of field, smooth handheld camera, natural, relaxed performances.

References: @Image 1 is Jeff: use his face, hair and build only, not the grey clothes, the layout or the background of the sheet. @Image 2 is Jeff's outfit. @Image 3 is the croissant, a golden butter croissant on a small white plate. @Image 4 shows the whole bakery wide, from far back; @Image 5 is the same bakery from behind the counter; @Image 6 is the same bakery from the customer's side of the counter. They fix the set and the light only. @Audio 1 is Jeff's voice from an earlier cut: keep this voice, not its words.

Shot 1 (0-2s), medium shot, the counter as in @Image 5: Jeff, a man in his forties with a short grey beard, still holding the small white plate in his right hand, slides it across the counter toward the customer, a quietly proud smile. He says to her in French, pleased: "Il sort du four."
Hard cut to:
Shot 2 (2-5s), close-up from the customer's side, the counter as in @Image 6: in the foreground her hands break the croissant in two, steam rising from the flaky inside; behind it, Jeff leans on the counter and waits for her verdict, a small hopeful lift of his eyebrows.

Bakery sounds, the oven door creaking, paper bags rustling, quiet street noise through the door. No subtitles, no text.
```

Environ 250 mots, dans le repère d'un clip de deux cuts. Clip 2 de la boulangerie : cuts 2 et 3, prévus à 2 et 3 s, générés ensemble à 5 s ; deux positions de caméra, donc deux vues, et 6 images en tout.

Avant d'enregistrer le prompt : `references/jeu.md#Contrôle du prompt`, puis relire le prompt contre `references/anti-slop.md#Mots interdits dans les prompts` et chaque réplique contre `references/anti-slop.md#Tics d'écriture à bannir (répliques)` : le contrôleur automatique du plugin ne lit pas l'anglais, cette relecture est manuelle et obligatoire à chaque fois.

## Règles du français

- Tout le prompt est en anglais, sauf les répliques, en français entre guillemets, mot pour mot, jamais reformulées ni traduites.
- La réplique est amenée simplement : `She says to <who> in French, <intention>: "…"`. Juste `in French` : jamais le mot « accent », aucune description de la voix (timbre, volume, débit), aucune phrase sur la bouche. Seule exception : deux ou trois mots de voix dans la présentation du personnage, quand le client demande une voix particulière, ou qu'un personnage parle dans plusieurs clips sans aucun clip où il parle seul (pas d'extrait possible), ou que son extrait manque (`references/montage.md#Échec`) : les mêmes mots dans chaque clip où il parle (`references/fiche-personnage.md#Voix`).
- Registre oral romand, phrases entières et courtes : `references/jeu.md#Réplique en français parlé`.
- Jamais d'orthographe phonétique pour simuler l'accent : ça casse le calage des lèvres.
- Voix off (personne ne parle à l'écran) : `A woman's voice-over says in French, <intention>: "…"` ; seulement si la demande en veut une.
- Réplique hors champ : `<Name>, off screen, says in French: "…"`, et le personnage qu'on voit écoute, bouche fermée.

## Durée et timing

- 15 s par défaut. Une vidéo se fait clip par clip, quelle que soit sa durée. Un cut : 3 à 5 s avec visage et jeu, 2 à 4 s pour un geste (repères). Seedance génère 4 s au minimum (`references/choix-modele.md#Brouillon et finalisation`) : les cuts courts se regroupent en clips de 4 s au moins (`## Clips`). Un clip est généré à la somme des durées prévues de ses cuts ; rien n'est généré pour être coupé.
- Prompt : environ 150 à 220 mots pour un clip d'un cut, environ 50 à 80 mots par cut en plus (repère, pas règle). Répliques : environ 20 à 30 mots dits pour 15 s, environ 6 par réplique à l'écran (`references/mise-en-scene.md#Répliques à l'écran`). Repères, pas plafonds.
- **Fenêtre de parole** : une réplique prend à peu près `mots ÷ 3` secondes ; elle doit tenir dans son cut, donc dans son plan du clip, un geste remplit le reste. 6 mots ≈ 2 s ; 15 mots ≈ 5 s, donc jamais dans un cut de 4 s. Trop long pour le cut : on raccourcit la réplique ou on allonge le cut, jamais les deux mots qui la font vivre.
- La dernière phrase d'un cut finit avant sa fin prévue, un geste remplit le reste.
- Une action principale par cut : la durée découle des actions (`references/mise-en-scene.md#Blocking et jeu`).

## Continuité entre les cuts

- Chaque clip est une génération à part : rien ne passe d'un clip à l'autre que les références et le texte. Dans un clip, Seedance garde le décor, la lumière, les visages et les voix d'un plan à l'autre ; la coupe y est écrite (`Hard cut to:`).
- L'ouverture est identique mot pour mot dans chaque clip ; la ligne de son aussi, pour tous les clips d'un même lieu. Pas de musique dans un clip : chaque génération jouerait un autre morceau.
- Un personnage est présenté avec les mêmes mots au premier plan où il paraît dans chaque clip.
- Le premier plan d'un clip reprend en toutes lettres la fin du clip d'avant : objet, main, position, regard (`still holding the small white plate in his right hand`). À l'intérieur d'un clip, le plan suivant n'a pas à le redire. Jamais « previous cut », jamais une vidéo en référence.
- Voix : dès qu'un clip où un personnage parle seul est gardé, son extrait de voix part dans tous les clips suivants où il parle (`references/montage.md#Extraire une voix`) ; le bloc du clip le liste dans `Médias :` (`voix:<slug>`) et `References:` porte `@Audio 1 is <Name>'s voice from an earlier cut: keep this voice, not its words.` Le clip d'où vient l'extrait n'en a pas.
- Une réplique ne passe jamais d'un plan à l'autre, ni d'un clip à l'autre. L'accroche n'ouvre que le cut 1.

## Retouches

Une seule modification par itération : jamais deux changements en même temps, même si le client en demande plusieurs — proposer de les traiter l'un après l'autre. Noter la correction dans `journal.md` (bibliothèque du client) avant de relancer. Journal : `cut-correction` (correction d'un cut), `script-correction` (trame).

- **Correction d'un cut** : le client parle de cuts (« change le cut 3 ») ; c'est le clip qui contient ce cut qui est refait. Le réalisateur ne réécrit que le bloc de ce clip, en version suivante (`### Clip 2 — v2`) : seul le plan de ce cut change, plus la reprise du plan suivant dans le même clip si la fin change ; et la ligne de trame si elle change ; `script.md` passe en version suivante. Prix : nouveau brouillon du clip, plus sa finalisation s'il était `gardé` ou `final`. Un clip gardé ou final n'est jamais retouché sans demande, et la `PHRASE` signale une rupture avec lui.
- **Correction de la trame** : seulement les lignes changées et les blocs des clips ajoutés ou changés. Un clip `gardé` ou `final` ne change jamais de limites sans que la correction le vise. Un cut ajouté rejoint un clip voisin pas encore gardé ; si ses deux voisins sont gardés ou finaux, il est allongé à 4 s et forme son propre clip. Les clips encore `à faire` peuvent être regroupés à nouveau.
- Une vue du lieu n'est refaite que si la correction touche ce qu'elle fixe : le lieu, la lumière, le style, ou une position de caméra nouvelle (`references/vues-lieu.md#Corrections`).
