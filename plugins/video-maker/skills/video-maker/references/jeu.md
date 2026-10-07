# Jeu, répliques et son

> Rôle : diriger le jeu, les répliques et le son dans le prompt vidéo, par le réalisateur, avec `references/script.md` (structure du prompt) et `references/anti-slop.md` (mots interdits).
> Leçon des tests réels du 2026-10-02/03 : un prompt long, plein de micro-gestes télégraphiques et de consignes sur la voix et la bouche, donnait un jeu mécanique et un français récité. Un prompt court, où chaque plan nomme ce que ressent le personnage et amène la réplique simplement, donnait un jeu naturel.

## Principes

- Tout s'écrit en anglais dans le prompt ; seules les répliques restent en français, entre guillemets.
- L'ouverture du prompt demande `natural, relaxed performances` (`references/script.md#Ouverture`) : c'est le réglage général du jeu, on ne le répète pas dans chaque plan.
- Chaque plan dit ce que fait le personnage et ce qu'il ressent, en une ou deux phrases qui se lisent comme un récit. Le modèle joue une intention nommée bien mieux qu'une liste de gestes.
- Une action principale par plan, avec son but et son arrivée. Le reste vient du modèle.
- Retenue : une personne crédible, pas un comédien. Des émotions petites et précises (`slightly embarrassed`, `a small nervous laugh`), jamais forcées.

## Jouer une intention

On écrit l'émotion que le modèle sait jouer, précise, accrochée à l'action ou à la réplique. Elle peut se dire par un mot d'émotion précis ou par ce qu'elle fait au corps, en une proposition :

| Ce qu'on veut | À écrire |
|---|---|
| Gêne | `slightly embarrassed`, `her cheeks flushing`, `she avoids his eyes`, `with a small nervous laugh` |
| Complicité | `the bartender nods with a knowing grin` |
| Timidité, plaisir retenu | `she bites back a shy smile`, `biting her lip with a shy, amused smile` |
| Hésitation | `walks up to the counter a little hesitantly`, `lowers her voice` |
| Surprise | `he looks up, surprised`, `he glances around to see who sent it` |
| Soulagement | `lets out a breath and laughs at himself` |

- **Précis plutôt que général** : `a shy smile`, `a knowing grin`, `a small nervous laugh` sont permis et voulus. Un mot seul et vague (`happy`, `confident`, `friendly`, `expressive`, `animated`, `a big smile`) ne donne rien ou une grimace : `references/anti-slop.md#Mots interdits dans les prompts`.
- **Une ou deux notations par plan**, pas une liste. Jamais de style télégraphique (`Performance: Tiago looks, eyebrows up, nods.`) : le modèle exécute chaque fragment séparément et le jeu devient mécanique.
- **Les mains** : un objet ou un geste qui a une cause dans la scène, dans la phrase de l'action (`she taps her card on the payment terminal`, `he turns his glass by the stem`). Pas de geste ajouté pour occuper les mains.
- **La réaction** : tout ce qui arrive à quelqu'un obtient une réaction nommée (`He looks up, surprised.`), dans le même plan ou le suivant (`references/mise-en-scene.md#Beats`, point 4).
- **Les regards** comptent quand ils racontent : `glances toward the window`, `watching`, `avoids the bartender's eyes`.

## Rythme et dernier plan

- Une action principale par plan, 3 à 5 s pour un plan avec jeu, 2 à 3 s pour un passage muet. Repère, pas règle : la durée découle des actions (`references/mise-en-scene.md#Blocking et jeu`).
- Plans à la suite (`Shot 1 (0-5s)`, `Shot 2 (5-8s)`…) ou plan continu : `references/script.md#Un plan`.
- Le dernier plan garde du mouvement jusqu'à la dernière image : un sourire qui arrive, un regard, un verre qu'on lève. Après la dernière réplique, la bouche est fermée, mais le visage vit.
- Pour une fin, jamais `hold`, `freeze`, `static pose` ni `stays still` : le clip s'éteint et le visage se fige.

## Physique de la situation

Par défaut, le modèle rend un corps reposé, sur un sol plat, par temps doux. Dès qu'un plan comporte un effort, un terrain ou une météo, le réalisateur fixe l'état du corps de chaque plan avant de l'écrire, et le prompt le montre.

- **État du corps, par plan.** Avant d'écrire un plan, une ligne `État :` dans `script.md`, section `## Plans` (`references/script.md#Plans`) : fatigue accumulée, charge portée, terrain, météo, souffle. Dans le prompt, une courte proposition dans la phrase du plan, en anglais (`second hour of the climb, a 12 kg pack on his back`), et chaque élément se voit dans l'action.
- **Continuité.** L'état passe d'une coupe à l'autre, jamais remis à zéro. La fatigue ne fait que monter, sauf si un plan montre un repos. Neige sur les épaules, visage rougi, cheveux mouillés restent jusqu'à ce qu'une action les enlève.
- **Reprise après un arrêt.** Lente et en plusieurs temps : appui sur l'outil ou sur un genou, premiers pas courts et lourds. Jamais `stands and keeps walking` seul.
- **Tempo écrit.** Tout déplacement d'effort porte une vitesse et un poids concrets : `one step every two seconds`, `torso bent into the slope`, `boots sinking to the ankle`.
- **Le décor réagit.** La botte s'enfonce, le vent plaque la veste, l'eau freine les jambes. Chaque réaction a sa cause visible.
- **Retenue.** La physique d'effort s'applique quand la situation l'exige, dans ces plans-là.

| Situation | Tempo | Signes visibles d'effort | Erreur typique du modèle, à contrer |
|---|---|---|---|
| Montagne, froid | en montée, un pas toutes les 1,5 à 2 s ; arrêts pour souffler | buste penché vers la pente, buée à chaque expiration, gants serrés sur le piolet, givre dans la barbe | pas vifs comme sur du plat, reprise immédiate après une rafale |
| Chaleur | gestes et marche lents, pauses à l'ombre | sueur au front, tissu collé au dos, yeux plissés | peau sèche et fraîche, énergie constante |
| Course | environ 3 pas par seconde, ralentit en fin de plan | bouche ouverte, épaules qui montent, sueur | course qui flotte, souffle invisible |
| Port de charge | pas courts, un par seconde, arrêts pour rajuster | buste penché en avant, sangles qui tirent les épaules | charge sans poids, dos droit, grands pas |
| Eau | jambes freinées, mouvements amples et lents | vêtements mouillés plus sombres et collés, frisson en sortant | eau sans résistance, vêtements secs juste après |

Avant (essai réel, après une rafale) : `He waits, then pushes off the snow, stands and keeps walking.`

Après : `Shot 3 (12-18s): Second hour of the climb, a 12 kg pack on his back, the climber leans on his ice axe and rises from one knee over two seconds, torso bent into the steep snow slope, exhausted but stubborn; his first steps are short and heavy, one every two seconds, the wind pressing his jacket to his back, his breath fogging.`

## Ce qui fait robot

| Ce qu'on voit | Cause dans le prompt | Ce qu'on écrit |
|---|---|---|
| Jeu mécanique, gestes enchaînés sans vie | Micro-gestes télégraphiques en liste | Une intention nommée par plan (`## Jouer une intention`) |
| Réplique récitée, bouche qui articule | Consignes sur la voix, le volume, la bouche | La réplique amenée simplement (`## La réplique dans le plan`) |
| Réplique bousculée ou coupée | Réplique trop longue pour son plan | La fenêtre de parole : `mots ÷ 3` secondes |
| Sourire constant, grimace | `smiling`, `happy`, `big smile`, `expressive` | Un sourire précis qui arrive à un moment (`bites back a shy smile`) |
| Gestes saccadés | `fast`, `quickly` | La vitesse réelle : `in one easy motion`, `at an easy walking pace` |
| Personnage qui ne réagit pas | Aucune réaction écrite | `He looks up, surprised.` |
| Fin figée | `hold`, `freeze` | Un dernier mouvement qui continue |

Liste complète des mots à éviter : `references/anti-slop.md#Mots interdits dans les prompts`.

## La réplique dans le plan

La réplique s'écrit dans le plan où elle est dite, à la fin de la phrase qui l'amène :

`She says to the bartender in French, in a low voice, with a small nervous laugh: "Le monsieur, dehors... je peux lui offrir un verre ?"`

- Ordre : qui parle, à qui, `in French`, l'intention ou la manière en quelques mots (`quietly`, `in a low voice`, `with a small nervous laugh`, `teasing him`), puis la réplique entre guillemets.
- Juste `in French`. Jamais le mot « accent », aucune description de la voix (timbre, volume, débit), aucune phrase sur la bouche ou la mâchoire : ces consignes faisaient articuler le personnage comme à la lecture.
- La réplique tient dans sa fenêtre de parole : environ `mots ÷ 3` secondes, un geste remplit le reste du plan (`references/script.md#Durée et timing`). Une réplique de 15 mots ne tient pas dans un plan de 4 s.
- **Pauses** : un « ... » dans la réplique, là où la personne hésiterait, un seul par réplique. Jamais `(silence)` ni `pause 1s`.
- L'accroche reste dans le même ton : jamais `excited` ni `enthusiastic`.
- **Voix off** et **réplique hors champ** : `references/script.md#Règles du français`.

## Réplique en français parlé

Registre oral de Suisse romande, posé : on parle comme à un ami au café, pas comme une brochure, ni comme un auteur qui cherche la phrase qu'on retiendra. On n'écrit pas une réplique : on écrit ce que cette personne dit, à quelqu'un, pour obtenir quelque chose, à cet instant de la scène.

1. **À qui, pour obtenir quoi** : chaque réplique s'adresse à quelqu'un de présent (le barman, l'ami, la personne qui filme ; le spectateur seulement quand le personnage parle face caméra) et sert un but concret : commander, demander, taquiner, s'excuser, réagir à ce qu'il vient de voir. Une réplique à laquelle personne dans la scène ne pourrait répondre est écrite pour le public : on la réécrit.
2. **Personne ne commente ce qu'il ressent ni pourquoi il agit.** Pas de « sinon je me dégonfle », « j'ose », « je repousse » : la gêne, le courage, l'hésitation se jouent (un rire nerveux, un regard qui fuit). Exception : un aveu à un proche, dit de travers, avec gêne.
3. **Aucune formule.** Pas de réplique en deux temps symétriques (« X, j'ose. Y, je repousse. »), pas de chute citable, pas de jeu de mots. Si la phrase ferait une bonne légende sous la vidéo, elle est trop écrite.
4. **Courte et facile à dire.** Une phrase entière qu'on dit d'un souffle, sans buter : environ 6 mots à l'écran, plus seulement si la demande l'exige et si le plan est assez long (`mots ÷ 3` secondes). Deux répliques courtes valent mieux qu'une longue.
5. **Des phrases entières, lâches, comme à l'oral** : sujet, verbe, une politesse (« s'il vous plaît »), un mot qui désigne au lieu de décrire (« le monsieur, dehors », « celui-là », « là-bas »). Jamais de style télégraphique (« Lui, dehors. Un spritz. ») : personne ne commande un verre en trois mots-clés. Une hésitation (« ... ») quand la situation l'appelle (gêne, timidité), jamais par quota. Un « franchement », « bon » ou « hein » là où il tomberait à l'oral, deux au plus par clip, jamais en premier mot de l'accroche (`references/hooks.md#Ouvertures bannies`).
6. **Personne n'explique le produit.** Le produit, le service ou le message de la demande ne s'explique pas dans une réplique. Son nom est dit une fois au plus, jamais dans l'accroche. Dans une scène qui parle d'autre chose, il ne se colle pas à la réplique : il arrive à la fin, par un objet, un geste ou l'image seule.
7. **Réponses indirectes** quand la situation s'y prête : une question gênante obtient une esquive, une contre-question ou un geste. Une question pratique (« Lequel ? ») obtient une réponse pratique.
8. **Pas de « ne » systématique** : « c'est pas », « je sais pas ». On le garde quand la phrase doit sonner sérieuse ou polie (« je peux lui offrir un verre ? »). « T'as » et « y a » sont admis ; pas d'autre orthographe phonétique.
9. **Couleur romande** : deux helvétismes au plus par clip, seulement là où ils viennent tout seuls, choisis pour l'âge et le milieu du personnage (natel, souper, ça joue, septante, « ou bien ? » en fin de phrase) ; jamais en premier mot, jamais en caricature.
10. **La chute ne résume pas.** Elle finit sur un geste, un regard ou une réplique courte qui décale. Jamais de morale.
11. **Tics d'écriture** : chaque réplique passe `references/anti-slop.md#Tics d'écriture à bannir (répliques)`.
12. **Test de l'improvisation** : écrire aussi la réponse de l'autre (même si on ne l'entend pas), puis lire l'échange à voix haute. Quelqu'un dirait-il exactement ces mots, dans cet ordre, s'il n'y avait pas de caméra ? Si on bute, le modèle butera aussi.
13. **Réplique fournie par le client** : gardée mot pour mot si elle passe la conformité et se dit à l'oral. En langage de brochure : son intention gardée, dite à l'oral, et une phrase au client : « Je l'ai gardée, dite comme on la dirait au café. » Il redemande la phrase exacte : elle est reprise telle quelle.
14. **Scène à deux** (seulement quand la demande a deux personnages) : répliques alternées, jamais deux voix en même temps.

Budget : environ 20 à 30 mots dits pour 15 s ; moins de mots, plus d'image (`references/script.md#Durée et timing`). Le budget se tient avec moins de répliques, jamais avec des phrases amputées. Conformité inchangée : `references/conformite.md#Interdits`.

Avant / après :
- Commander un verre pour un inconnu. Avant : « Un spritz pour le brun. Je paie maintenant, sinon je me dégonfle. » Après : « Le monsieur, dehors... je peux lui offrir un verre ? », puis « Un spritz, s'il vous plaît. »
- Demander le secret. Avant : « Lui, dehors. Un spritz. Et tu dis pas que c'est moi. » Après : « Tu lui amènes un spritz, au gars sur la terrasse ? Mais tu dis pas que c'est moi, hein. »
- Sujet collé à une scène qui n'a rien à voir. Avant : « Offrir un verre à un inconnu, j'ose. Le cours de salsa, je repousse. » Après : la scène garde ses mots ; le cours de salsa arrive à la fin, par l'image, ou pas dans cette version.
- Trop long pour le plan. Avant, dans un plan de 4 s : « Tu pourrais envoyer un verre au monsieur en veste beige qui est assis tout seul dehors ? » (17 mots). Après : « Le monsieur, dehors... je peux lui offrir un verre ? » (9 mots, 3 s, le reste pour le regard vers la vitrine).

## Son

Une seule ligne, après le dernier plan, juste avant `No subtitles, no text.` : l'ambiance du lieu, du plus proche au plus lointain, puis la musique si elle va au lieu.

`Ambient bar sounds, clinking glasses, soft murmur, quiet lounge music in the background.`

- 2 ou 3 sons du lieu, ceux qu'on entendrait vraiment là (verres, machine à café, rue, vagues, vent). Chaque son a sa source dans l'image (`references/mise-en-scene.md#Registre des objets et du son`) : pas d'oiseaux dans une pièce fermée.
- **Musique permise** quand elle va au lieu et qu'on l'y entendrait : musique lounge discrète dans un bar, radio dans une cuisine, rien sur une arête en montagne. Elle rend la scène moins figée ; elle reste `quiet`, `in the background`.
- Jamais de voix off ni de narration sauf demande du client, jamais de `whoosh` ni de montée d'effet : on ne les écrit pas. Un rendu précédent en avait ajouté : la ligne finit par `no voice-over, no whooshes` (`references/anti-slop.md#Exclusions en positif`).
- Pas de liste de bruits datés geste par geste : le modèle met lui-même le son des gestes qu'il montre.

Exemples : `Café sounds, the espresso machine hissing, cups on saucers, street noise through the window, a radio playing softly behind the counter.` · `Open-air terrace, water lapping at the pool edge, a light breeze in the olive trees, distant birds.` · `Wind across the ridge, crampons crunching in the snow.`

## Exemple avant / après

Plan 2 du verre offert, dans le style d'une version longue du plugin (prompt complet d'environ 1 500 mots, avec blocs de personnage, de voix et de bouche en plus). Résultat au test réel : jeu mécanique, français récité.

Avant :
```
[00:04-00:07] Shot 2: Tiago (@Image 2, black hair, beard) sets out an empty wine
glass; Clara speaks, then pays by phone: green check.
Performance: Tiago looks, eyebrows up, nods. Clara, eyes down, taps phone.
Voice: same voice as before; low, a little embarrassed, she says quietly: "Tu
pourrais lui envoyer un spritz, au monsieur en veste beige qui est dehors ?"
```

Après :
```
Shot 2 (5-8s): Close-up at the counter. Her cheeks flushing, she avoids the
bartender's eyes and says quietly in French: "Un spritz, s'il vous plaît." She
taps her card on the payment terminal; the bartender nods with a knowing grin,
and she bites back a shy smile.
```

## Contrôle du prompt

Avant d'enregistrer le prompt, le relire ligne à ligne. Un point manque : corriger d'abord.

- [ ] Cinq parties seulement : ouverture, `References:`, plans, ligne de son, `No subtitles, no text.` ; environ 200 à 350 mots pour 15 s (repère).
- [ ] `References:` : une courte proposition par média, dans l'ordre des médias ; une tenue sans image décrite en mots.
- [ ] Plan 1 : il ouvre sur l'accroche choisie (`references/hooks.md#Du hook au premier plan`), en mouvement dès la première image.
- [ ] Chaque plan : `Shot N (a-bs)`, l'endroit s'il a sa vue, une action principale avec son arrivée, une émotion nommée et jouable, sa réaction quand quelque chose arrive à quelqu'un.
- [ ] Chaque personnage présenté une fois en 2 à 4 mots visibles, ensuite par son prénom.
- [ ] Chaque réplique : `says to <who> in French, <intention>: "…"`, sans le mot « accent », sans description de voix ni de bouche ; elle tient dans sa fenêtre de parole (`mots ÷ 3` secondes).
- [ ] Le dernier plan bouge jusqu'à la fin ; la dernière réplique finit au moins 1 s avant.
- [ ] Le son en une ligne, sources présentes dans l'image, musique seulement si elle va au lieu ; ni voix off non demandée, ni effet.
- [ ] Effort, terrain ou météo : l'état du corps dans la phrase du plan, sans recul d'un plan à l'autre (`## Physique de la situation`).
- [ ] `references/mise-en-scene.md#Auto-contrôle` coché ; aucun mot de `references/anti-slop.md#Mots interdits dans les prompts` ; chaque réplique passe `references/anti-slop.md#Tics d'écriture à bannir (répliques)`.
