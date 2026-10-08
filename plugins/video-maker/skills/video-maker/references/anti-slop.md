# Anti-slop

> À lire avant d'écrire un prompt modèle, un hook, un script ou une phrase pour le client.
> Les prompts modèle s'écrivent en anglais ; les explications ci-dessous sont en français.

## Mots interdits dans les prompts

Un mot de qualité ne donne aucune instruction exploitable au modèle ; ce qui produit du réalisme, ce sont les cinq leviers concrets détaillés plus bas. Pour chaque mot à éviter, le tableau donne la formulation concrète à écrire à la place.

| Mot interdit | Par quoi le remplacer |
|---|---|
| `cinematic` (seul, sans description) | Vidéo : l'ouverture de la famille (`references/script.md#Ouverture`), où `Cinematic` est suivi du genre, de la lumière, du lieu et du rendu de la caméra ; image de fiche : la ligne `Style` (boîtier, objectif, lumière) |
| `hyper-realistic` · `ultra-realistic` · `photorealistic` | `visible pores`, `unretouched`, `candid` |
| `realistic` · `very realistic` | Les 5 leviers ci-dessous |
| `8K` · `4K` · `ultra HD` | Régler la résolution dans l'outil, pas dans le prompt |
| `masterpiece` · `award-winning` · `best quality` · `high quality` | Rien : le rendu est déjà dans l'ouverture (vidéo) ou dans la ligne `Style` (image) |
| `ultra detailed` · `hyper-detailed` | Nommer les textures une par une |
| `beautiful` · `stunning` | Description physique du sujet |
| `perfect` · `flawless` | `slight blemishes and uneven skin tone`, `slight asymmetry in facial features` |
| `smooth skin` · `glossy skin` | `oily T-zone, drier cheeks`, `subsurface scattering` |
| `luxury` · `premium` | Nommer la matière : `mirror-polished head`, `aged oak handle` |
| `moody` · `epic` · `dramatic lighting` | Traduire en source de lumière nommée + événement visible |
| `slow motion` (générique) | `real-time speed, 24fps`, ou `suspended`, `frozen in time` si l'arrêt est voulu |
| `camera follows the subject` | `lateral tracking shot, strict side view, constant distance` |
| `fast` · `quickly` | La vitesse réelle : `in one easy motion`, `over about a second`, `at an easy walking pace`, `a decisive step toward the lens` (« fast » produit des saccades) |
| `happy` · `confident` · `friendly` · `expressive` · `animated` · `big smile` pour décrire le visage ou le jeu | Une émotion précise et jouable, accrochée à l'action : `slightly embarrassed`, `a small nervous laugh`, `her cheeks flushing`, `a knowing grin`, `a shy, amused smile` (`references/jeu.md#Jouer une intention`). `smile`, `grin`, `shy smile`, `nervous laugh` sont permis et voulus quand ils sont précis |
| `hold` · `freeze` · `static pose` en fin de clip | Un dernier mouvement qui continue : un sourire qui arrive, un regard, un verre qu'on lève |
| Micro-gestes en liste télégraphique (`looks, eyebrows up, nods`) | Une phrase de récit avec une intention nommée (`the bartender nods with a knowing grin`) |

Exception : `cinematic` peut rester dans un prompt s'il est immédiatement suivi d'une description concrète (genre, lumière, lieu, rendu de la caméra) ; sans elle, il compte comme mot interdit. Dans un prompt vidéo, il n'apparaît qu'une fois, dans l'ouverture : aucun plan ne nomme un autre appareil, une pellicule ou un « look » (`references/script.md#Ouverture`).

### Voix et bouche

Toute consigne sur la voix ou la bouche fait parler le personnage comme à la lecture : le modèle articule chaque syllabe. La réplique est amenée simplement (`references/jeu.md#La réplique dans le plan`) : `She says to <who> in French, <intention>: "…"`.

| À éviter pour la voix ou le dialogue | À écrire à la place |
|---|---|
| Une description de la voix : `breathy`, `husky`, `low conversational volume`, `easy everyday pace`, `words running together`, `relaxed casual diction` | Rien : la voix vient du modèle ; seulement l'intention de la réplique |
| Une phrase sur la bouche, les lèvres ou la mâchoire | Rien |
| Le mot `accent`, sous toutes ses formes | `in French` |
| `clear`, `crisp diction`, `precise diction`, `enunciates`, `pronounces every word` | Rien |
| `slow`, `slowly`, `unhurried` appliqués à la parole | `quietly`, `in a low voice` ; le calme passe par la situation |
| `energetic`, `emphatic`, `animated`, `excited`, `enthusiastic`, `passionate`, `projects his voice`, `loud`, `presenter`, `like a spokesperson` | Une intention précise : `with a small nervous laugh`, `teasing him`, `half to himself` |

## Les 5 leviers de réalisme

**1. Lumière : source + direction + ombre.** Une lumière sans source visible se voit tout de suite.
Avant : `gorgeous cinematic lighting, dramatic and moody`
Après : `late-afternoon light through the café window, falling from camera-left; the shadow of his coffee cup lands clearly on the table`

**2. Mouvement : verbe + partie du corps + vitesse + cause.** Sans cause physique, le mouvement flotte. Une action claire à la fois (une par fenêtre d'environ 3 s, `references/jeu.md#Rythme et dernier plan`) et un seul mouvement de caméra par plan.
Avant : `the man gestures expressively while talking`
Après : `he taps the rim of his cup twice with one finger, then sets both hands flat on the table before he speaks`

**3. Caméra crédible, celle de la famille.** L'ouverture fixe le rendu de la caméra et sa façon de bouger ; les plans s'y tiennent. `cinema` : caméra à la main, souple, faible profondeur de champ. `telephone` et `documentaire` : tenue à bout de bras ou à l'épaule. `produit` : slider lent. Un seul mouvement par plan, avec une cause ; jamais de glisse de drone.
Avant : `sweeping drone-like camera glide around the table`
Après : `handheld at chest height, faint side-to-side sway even though he stands still, no stabilizer` — ou, à l'inverse : `camera fixed on a small tripod in the corner of the office, frame does not move for the whole shot`

**4. Texture de peau, optique à un seul endroit.** Dans une image de fiche, la ligne `Style` nomme le boîtier et l'objectif. Dans une vidéo, la peau et le visage viennent des planches ; l'ouverture donne le rendu une fois, et un plan parle de cadrage (« close-up », « through the window »).
Avant : `flawless 8K studio headshot, perfect skin`
Après (plan vidéo) : `closer on his face; the side light catches the fine lines around his eyes and a faint five-o'clock shadow`

**5. Zéro mot IA.** On retire les adjectifs de qualité et on décrit la prise de vue concrète à la place.
Avant : `stunning, premium, hyperrealistic portrait of a man, 4k`
Après : rien — remplacé par une description de plan construite avec les quatre leviers ci-dessus.

## Exclusions en positif

Ce qu'on ne veut pas voir se décrit par ce qu'on veut voir à la place. Dans le corps d'un prompt, la plupart des modèles lisent mal « no … » : ils gardent surtout le mot qui suit, et l'objet refusé finit à l'image.

| Avant (négatif) | Après (positif) |
|---|---|
| `no other customers in the background` | `the café is empty except for Jeff and his friend` |
| `don't change his tie or haircut` | `his tie, jacket and haircut stay identical in every shot` |
| `no messy desk` | `a bare desk with a single closed folder on it` |
| `no extra cups on the table` | `exactly one coffee cup on the table` |
| `no slow motion` | `real-time speed, 24fps` |

**Exceptions documentées**, les seules :
- la phrase de fin du prompt vidéo, `No subtitles, no text.` ;
- dans le prompt d'une vue du lieu, `completely empty: no people anywhere` et `One single vertical 9:16 frame, not a collage, no split panels, no text.` (`references/vues-lieu.md#Prompt d'une vue`) : la forme positive seule laissait passer des passants et des images en panneaux ;
- dans la ligne de son, `no voice-over`, `no whooshes` seulement si le modèle en a ajouté dans un rendu précédent.

Phrase de fin standard des prompts d'image : `Avoid generating any text, subtitles, watermark or logo.` Prompt vidéo : `No subtitles, no text.`

## Bloc de secours

Dernier recours à coller en fin de prompt quand un rendu part en plastique ou semble flotter, une fois les leviers ci-dessus déjà appliqués. Il allonge le prompt : jamais d'office, seulement après un rendu raté pour ces raisons, et raccourci à ce qui a manqué.

```
Realism fallback: light comes from a single identifiable source and casts
consistent shadows on every surface it touches. Skin shows real texture --
visible pores, fine facial hair, uneven tone, a hint of asymmetry -- with no
retouching or beautification. Eyes blink at a natural rate and hold a steady,
coherent gaze. Every movement keeps real-world weight and speed (24fps, no
slow motion), with a clear starting position, a physical cause, and a
finished end position. The camera behaves like an operator held it: handheld
shots carry a faint sway and breathing motion, tripod shots stay completely
still with no drift. Color stays natural and ungraded, with a realistic white
balance. Hands show five distinct fingers with plausible joints. The frame
contains no on-screen text, logo, watermark or subtitle.
```

Variante trépied (plan fixe, personnage statique) — remplacer la phrase caméra par :
`The camera sits on a fixed tripod: level horizon, one uninterrupted position, no pan and no drift for the whole shot.`

Famille `telephone` (`references/script.md#Ouverture`) : le bloc tel quel. Autres familles : retirer la phrase `Color stays natural and ungraded, with a realistic white balance.`

## Tics d'écriture à bannir (répliques)

Pour les accroches, les répliques et la carte, relus mot à mot avant la carte (concepteur) et avant la trame (réalisateur). Le fond du problème : l'écriture automatique choisit le mot le plus prudent, pas le plus précis. Test : si la phrase peut figurer telle quelle dans la pub d'une autre marque, on la réécrit avec un détail concret (un objet, un lieu, une habitude).

- **Ouvertures** : « Dans un monde où », « À l'ère de », « Au cœur de », « Il est important de », « Imaginez », « Et si… », « Découvrez », « Plongez », « Saviez-vous que ».
- **Structures** : « Ce n'est pas qu'un X, c'est un Y » ; « non seulement… mais » ; trois adjectifs ou trois bénéfices à la suite ; question rhétorique d'ouverture ; chute morale en fin de phrase ; tiret cadratin ou point-virgule dans une réplique.
- **Mots** : véritable, crucial, essentiel, précieux, dynamique, optimiser, valoriser, favoriser, accompagner, sur mesure, solution, révolutionner.
- **Vocabulaire de pub** : tranquillité d'esprit, sérénité, en toute confiance, vous le méritez, ceux qu'on aime, simplifiez-vous la vie, prendre le contrôle, l'expérience ultime.
- **Connecteurs de rédaction** : en outre, par ailleurs, en somme, tout d'abord / ensuite / enfin.
- **Répliques d'auteur** (`references/jeu.md#Réplique en français parlé`) : deux temps symétriques (« X, j'ose. Y, je repousse. ») ; style télégraphique (« Lui, dehors. Un tricycle. ») ; personnage qui commente ses sentiments ou ses raisons (« sinon je me dégonfle ») ; réplique qui ne s'adresse à personne dans la scène ; le message de la marque collé à une scène qui parle d'autre chose.

## Relecture avant de montrer

Ne jamais montrer un rendu au client sans passer par cette relecture. Six questions, un « non » suffit à refaire le clip :

1. Voit-on clairement d'où vient la lumière ?
2. Chaque mouvement a-t-il un appui réel (sol, objet, contact) ?
3. La caméra bouge-t-elle comme une vraie caméra (tenue à la main, fixée, ou sur slider selon la famille de l'ouverture) — pas au hasard, sans glisse sans cause ?
4. Le décor reste-t-il cohérent si on regarde un autre plan du même lieu ?
5. Le défaut ne se voit-il qu'en zoomant, ou saute-t-il aux yeux dès le premier visionnage ?
6. (Vidéo seulement.) Le personnage vit-il ? Signes de robot : jeu mécanique, sourire constant, réplique récitée ou bousculée, personnage qui ne réagit pas, image figée à la fin.

Si un défaut passe la relecture, le nommer avec son instant précis (exemple : main floue à 0:08) plutôt que de le taire.

## Ton du skill

- Phrases courtes. Dire ce qu'on fait et ce qu'on attend, dans cet ordre.
- Zéro superlatif : `captivant`, `percutant`, `incroyable`, `époustouflant`, `révolutionnaire`, `magique`, `sublime`, `exceptionnel` sont bannis, même dans une variante.
- Zéro emoji décoratif. Seul 💳 est autorisé, pour signaler un coût.
- Pas de « Super ! », « Excellent choix ! », « Voici une proposition ». On propose directement la chose.
- Pas de récapitulatif non demandé : on avance à l'étape suivante plutôt que de reformuler ce qui vient d'être dit.
- **Cinq lignes au plus par message**, hors trame et versions de la carte (`Trois versions`, leurs trois paragraphes et la question qui suit).
- Jamais dans un message au client : nom de modèle, identifiant (job, media, voix), prompt ou mot anglais, chemin de fichier, paramètre (résolution, format, tokens, durée technique), nom d'étape interne (moment, étape, phase, coulisse, sous-agent, concepteur, atelier, réalisateur, tournage), récapitulatif de ce que le skill vient de faire.
- Prix : une ligne, `💳 ~X crédits`.
- Attente : une ligne (« Images en cours, environ une minute. »), puis rien jusqu'au résultat.
- Erreur : une phrase qui dit ce qui se passe ensuite.
- Style, lumière et prises de vue en mots de tous les jours :

| Jargon | À la place |
|---|---|
| golden hour | soleil bas de fin de journée |
| rim light, contre-jour doux | lumière qui dessine le contour |
| handheld | caméra tenue à la main |
| close-up, plan serré | de près |
| shallow depth of field, bokeh | fond flou |
| tracking shot | la caméra le suit de côté |
