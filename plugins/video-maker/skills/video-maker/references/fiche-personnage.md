# Fiche personnage

> Rôle : créer un personnage réutilisable (planche, fiche) et ses tenues, quand l'inventaire le demande. Le concepteur choisit, l'atelier génère, l'agent principal montre.
> Anti-slop : `references/anti-slop.md` pour le prompt, `references/anti-slop.md#Ton du skill` pour les phrases au client. Modèles, prix et duel : `references/choix-modele.md`.

Personnages fictifs uniquement : un adulte (21 ans au moins, âge écrit en chiffres), au visage inventé. Jamais une personne réelle, une célébrité ou le client lui-même. Si le client le demande : « Je crée toujours un personnage inventé. Je peux reprendre le style d'une photo, pas le visage. »

## Dans l'inventaire

- Chaque personnage visible dans un plan entre dans l'inventaire, sans limite fixe : une planche et une image par tenue qui se voit (`## Tenues`).
- **Chercher avant de créer.** Slug (règle de `## Enregistrement`) comparé aux noms de dossiers de `bibliotheque/personnages/` : identique, ou proche (une lettre en plus, en moins ou changée ; le prénom au début d'un nom plus long). Trouvé → `existant`, repris sans question ; la carte écrit le nom de la fiche (« Jef » → « Jeff »). Rien : chercher dans les Elements du compte (`show_reference_elements`, lecture seule) ; trouvé → `existant (element)`, l'atelier crée sa fiche avec l'Element id.
- Prénom déjà pris par une autre personne (la demande décrit clairement quelqu'un d'autre : autre genre, autre âge, « un nouveau ») : le concepteur le dit dans la ligne `Je fixe` et donne un autre prénom (« Jeff existe déjà, j'appelle celui-ci Marco ») ; le client répond à la carte.
- Aucune ligne de voix à l'inventaire : la voix n'est ni choisie ni payée (`## Voix`).
- Mêmes règles de recherche pour les lieux et les objets (`references/fiche-lieu.md`, `references/fiche-objet.md`).

## Questions au client

Aucune question séparée. Ce que la demande dit du personnage est repris ; le reste, le concepteur le choisit (âge exact, cheveux, yeux, peau, signes particuliers, corpulence) et le résume en quelques mots dans la ligne `Je fixe` de la carte (« Jeff, nouveau : la quarantaine, cheveux châtains courts, barbe de 3 jours »). Le client corrige en répondant à la carte, ou au moment 2 sur l'image. Photos sur l'ordinateur du client : l'agent principal appelle `media_upload_widget`, seul outil de ce tour, avant la carte ; jamais de chemin local ; les media id passent dans les consignes du concepteur et de l'atelier.

Ces photos servent au style (allure, coiffure, vêtements), jamais au visage : voir la ligne de référence dans `## Template du prompt`. Les vêtements vus sur les photos inspirent les tenues (`## Tenues`), décrits en texte.

La planche ne fixe que l'identité : visage, cheveux, corps, peau, marques. Le personnage y porte toujours la même tenue neutre, qui ne dit rien du projet. Les tenues de la vidéo ont leurs propres images (`## Tenues`).

## Template du prompt

En anglais, un seul bloc, sections dans cet ordre. `Subject` et `Base outfit` sont écrits une fois, puis figés : ils sont recopiés mot pour mot dans la fiche (`## Description figée`) et dans les corrections et duels de la planche. `Subject` seul est aussi recopié dans chaque image de tenue (`## Tenues`). La vidéo n'en reprend que 2 à 4 mots visibles (`Clara, a young brunette woman`) : le visage vient de la planche (`references/script.md#Un plan`). `Base outfit` ne sert qu'à la planche.

`Subject` décrit l'identité seule : jamais d'expression, de pose ni d'humeur (« neutral », « smiling », « confident »). L'expression de la planche est écrite dans `Layout` ; celle de la vidéo vient des plans (`references/jeu.md#Jouer une intention`).

La planche est verticale (9:16) : trois rangées de trois panneaux, chaque panneau est lui-même un cadre vertical, comme la vidéo.

```
CHARACTER REFERENCE SHEET — "<First name>"

Subject: <First name>, a <age>-year-old <man/woman>. <Hair: colour, length,
texture, parting>. <Eyes: colour, shape>. <Facial hair if any: length, colour,
density, outline>. <Marks: mole, scar, freckles, and exactly where>. Unretouched
skin: visible pores, faint under-eye shadows, a few small blemishes, fine facial
hair, uneven redness on the cheeks, natural lips, no makeup. <Build: height,
shoulders, chest, waist, arms>. <Body hair on chest, arms and legs>. <Marks on
the body, and exactly where>.

Base outfit (neutral, always identical): a plain fitted mid-grey crew-neck
T-shirt, plain fitted charcoal trousers, plain white canvas sneakers; plain
fabric throughout, bare wrists. The fabric shows real creases and seams.

Layout: one single vertical image, a production turnaround sheet on a plain
light-grey seamless studio background. The sheet contains only <First name> and
the plain background. Nine equal panels in three rows of three, each panel a
vertical frame. On every panel where the face shows, the expression is neutral
and relaxed.
Panels 1 to 4 — head-to-toe views, all at one scale with the feet on one
shared floor line, standing at ease, arms hanging loosely:
1. Front view, framed and scaled like panels 2 to 4, with the head left out:
   the figure stops at the collar and the base of the neck, and above it there
   is only light-grey background, as on a headless shop mannequin dressed in the
   outfit. No hair shows in this panel.
2. Left profile, head to toe, the head in strict side view.
3. Right profile, head to toe, the head in strict side view.
4. Seen from behind, head to toe, the back of the head and the hair visible.
Panels 5 to 9 — close-ups:
5. The only complete frontal face: head-and-shoulders portrait, looking straight
   into the lens, neutral relaxed expression, lips closed and relaxed, jaw at
   rest, the whole face in sharp focus.
6. Tight close-up of the eye area only, the crop running from slightly above the
   brows down to the end of the nose: iris fibres, lashes, individual eyebrow hairs. The mouth
   stays outside the frame.
7. Tight close-up of the mouth and chin only<, including the beard>, framed below
   the nose: lips closed and relaxed, jaw at rest, lip texture. The eyes stay
   outside the frame.
8. <His/Her> right hand resting flat on a plain grey surface: natural nails,
   knuckle creases, fine skin lines.
9. Tight close-up of the skin on the forearm: pores, fine hair<, marks>.

Consistency rules: panel 5 is the one place where the whole face is shown;
panels 6 and 7 crop parts of that very face. Panel 1 matches panels 2 to 4 in
framing and scale, the head being the only thing missing. Across all nine
panels, one person: identical bone structure, <marks>, hair colour and length,
<facial hair>, eye colour, skin tone, build, outfit, accessories and shoes.
One daylight studio setup for the whole sheet, soft and even, with a single
white balance.

Style: unretouched casting photograph taken on a full-frame body with an 85mm
lens stopped down to f/5.6, window light from camera-left softened by a
diffuser, true-to-life colour, light film grain, skin texture in crisp focus. Looks like a casting photo taken on the
day, not a render or an airbrushed image.

Output: one vertical 9:16 image, each of the nine panels itself a vertical frame.
Avoid generating any text, subtitles, watermark or logo.
```

Avec des photos d'inspiration, ajouter cette ligne juste après `Layout` :
`@Image 1 is a style reference only: take the hairstyle and general look. The face is a new, original person who resembles no one in the reference. On this sheet the person wears the base outfit, not the clothes of @Image 1.`

Le personnage ne tient rien dans la planche : ni tasse, ni dossier, ni téléphone. Les objets ont leur propre fiche (`references/fiche-objet.md`) et entrent dans la vidéo par leur image.

## Règles de cohérence

- **Un seul visage de face**, au panneau 5. Les modèles vidéo s'ancrent sur ce visage ; deux visages de face (portrait et plein pied avec tête) font dériver l'identité. Le panneau 1 reste donc sans tête, au même cadrage que les panneaux 2 à 4. Cette règle ne se relâche jamais.
- **Texte figé.** `Subject` et `Base outfit` sont repris mot pour mot. Une correction du client qui touche `Subject` produit une nouvelle version, qui remplace l'ancienne partout, fiche et tenues comprises (les tenues déjà rangées sont refaites à leur prochain usage). Pas d'autre façon de modifier le texte figé.
- **Tenue neutre.** La planche ne montre jamais une tenue de projet : toujours `Base outfit`, tel quel. Le corps (carrure, pilosité, marques) est écrit dans `Subject`, pour qu'une tenue torse nu ou en maillot ne l'invente pas.
- **Peau non retouchée** : `Subject` le dit en toutes lettres et nomme ce qu'on doit voir (pores, léger cerne, duvet, rougeurs, petits défauts). Jamais `smooth skin`, `flawless`, `beautiful` : voir `references/anti-slop.md#Mots interdits dans les prompts`.
- **Exclusions en positif** : « the sheet contains only <First name> and the plain background » plutôt qu'une liste de « no … ». Voir `references/anti-slop.md#Exclusions en positif`.
- **Aucun texte dans l'image** : ni légende, ni numéro de panneau, ni flèche, ni nuancier. La phrase de fin `Avoid generating any text, subtitles, watermark or logo.` est toujours là.
- **Barbe et moustache** comptent comme les cheveux : décrites dans `Subject` (longueur, couleur, densité, contour), montrées au panneau 7, listées dans `Consistency rules`. Personnage chauve : garder « The hair is not visible either » au panneau 1.
- **Couvre-chef et lunettes** : jamais sur la planche, rien ne cache le visage. Ce sont des pièces de tenue (`## Tenues`).
- **Correction** : une seule modification à la fois, formulée sur le texte figé (« keep everything the same, but change X »).

Relecture avant de montrer, en plus de `references/anti-slop.md#Relecture avant de montrer` :

| Défaut sur le rendu | Correction du prompt |
|---|---|
| Deux visages de face | Rappeler que le panneau 5 est le seul ; panneau 1 sans tête |
| Panneau 1 plus serré ou zoomé | Rappeler qu'il a le cadrage des panneaux 2 à 4, sans la tête |
| Gros plan des yeux où la bouche apparaît | Recadrer : juste au-dessus des sourcils jusqu'au bout du nez, bouche hors champ |
| Peau lisse, aspect cire | Imperfections nommées une à une dans `Subject` |
| Tenue qui change d'un panneau à l'autre | `Base outfit` recopié tel quel, pièces listées une par une |
| Texte, légendes ou numéros dans l'image | Phrase de fin présente, exclusions en positif |
| Main à quatre ou six doigts | Refaire ; le signaler au client si le défaut reste visible |

## Voix

Aucune voix à choisir, aucun extrait à payer. Seedance crée la voix en même temps que l'image, à partir du personnage qu'il voit et de la réplique écrite dans le prompt (`references/jeu.md#La réplique dans le plan`). Les tests du 2026-10-02/03 ont donné un français plus naturel ainsi qu'avec un extrait de voix joint.

- Le client veut une voix particulière (« plus grave », « plus jeune ») : deux ou trois mots dans la présentation du personnage, au premier plan où il parle (`Jeff, a man in his forties with a deep, calm voice`). Jamais le mot « accent », jamais de consigne sur la bouche ni sur le débit.
- Film en segments : ces mots de voix sont identiques dans chaque segment (`references/script.md#Segments`).
- Fiche plus ancienne avec une voix, un `voice_id` ou un extrait : gardés tels quels dans la fiche, jamais utilisés.

## Modèle et appel

1. Modèle : `gpt_image_2_5` (dernière version de GPT Image), revérifié en début de session avec `models_explore` (voir `references/choix-modele.md#Vérification en début de session`). Le client ne choisit pas le modèle.
2. Prix : `generate_image {model:"gpt_image_2_5", prompt, aspect_ratio:"9:16", quality:"high", resolution:"2k", get_cost:true}` (~3 crédits). Prix compté dans la ligne `Je fixe` (concepteur).
3. Le même appel sans `get_cost`. Juste après la soumission, avant `jobs_wait` : écrire le job id dans `projets/<slug>/etat.md`, `## Jobs payants`, ligne `images (job ids)`, entrée `personnage:<slug>=<job id>`. Avec des photos d'inspiration : `medias: [{role:"image_references", value:<media id>}]`, une entrée par photo, et la ligne de référence dans le prompt.
4. `jobs_wait` jusqu'à l'état final, relecture (`## Règles de cohérence`). L'agent principal montre l'image au moment 2.
5. Échec technique : `references/choix-modele.md#Échec technique d'une image`. Refus du client au moment 2 : duel, avec le prompt corrigé (`references/choix-modele.md#Duel`), sauf gagnant net dans `preferences.md`.
6. Réponse perdue ou délai dépassé : ne jamais soumettre à nouveau à l'aveugle, reprendre le job existant (voir `references/choix-modele.md#Arbre de décision`).

## Enregistrement

Par l'atelier, dès que la planche passe la relecture. Si le client la change au moment 2, la nouvelle image remplace l'ancienne dans la fiche, avec une ligne `## Historique`.

1. **Dossier** : `bibliotheque/personnages/<slug>/`. Slug = le prénom seul, normalisé : minuscules, accents retirés, tout caractère autre qu'une lettre ou un chiffre remplacé par un tiret. « Jeff le barman » → `jeff` ; « Émilie » → `emilie`. C'est la règle de slug de SKILL.md. Un prénom déjà pris par une autre personne est réglé par le concepteur dans la carte (`## Dans l'inventaire`) : l'atelier ne pose jamais de question.
2. **Fichier** `fiche.md`, copié de `templates/fiche-element.md` et rempli ainsi :
   - Titre : le prénom. `Type : personnage`. `Créé le :` date du jour.
   - `## Description figée` : les blocs `Subject` et `Base outfit`, tels qu'envoyés au modèle, sans rien changer.
   - `## Tenues` : vide à la création ; les tenues s'y ajoutent ensuite (`## Tenues` de cette référence).
   - `## Identifiants Higgsfield` : job id de l'image validée. Element id si le personnage existe comme Element sur le compte (`show_reference_elements`), sinon laisser vide.
   - `## Historique` : `AAAA-MM-JJ : création`.
3. **Journal** : ajouter une ligne à `bibliotheque/journal.md`, au format de `references/apprentissage.md#Format des lignes` :
   `AAAA-MM-JJ | element-cree | personnage=<slug> | projet=<slug du projet>`

## Tenues

Déduites par le concepteur pour l'inventaire, générées par l'atelier. La planche fixe qui est le personnage ; chaque tenue est une image à part, faite à partir de la planche. Les images de tenue vont directement dans la vidéo, après les planches (`references/choix-modele.md#Limite de références`) : la planche donne le visage, la tenue le corps habillé.

**Déduire les tenues**

1. Lister en interne, en mots simples, les plans prévus par la demande : « il sort sur la terrasse en chemise ouverte », « il enlève sa chemise », « il plonge ».
2. En déduire, pour chaque personnage, la tenue de chaque plan, pièce par pièce : « chemise en lin ouverte + short de bain », puis « torse nu, short de bain ». Une tenue qui revient compte une fois.
3. Une image de tenue seulement quand le changement se voit nettement (chemise → torse nu, costume → maillot). Manches retroussées, veste ouverte, chemise mouillée : même tenue, l'état est écrit dans le plan.
4. Lire `## Tenues` dans la fiche du personnage. Une tenue rangée dont les pièces correspondent est reprise telle quelle, rien à payer. Les autres sont nouvelles.
5. Garder la vidéo à 9 images au plus : `references/choix-modele.md#Limite de références`. Au-delà, la tenue la plus simple d'un personnage secondaire n'a pas d'image : elle est décrite en mots dans la vidéo, et la ligne `Je fixe` la nomme sans prix.

La vidéo combine la planche (visage) et l'image de tenue sans tête (corps et vêtements) : combinaison validée au test réel du 2026-10-03. Si le visage dérive, voir SKILL.md, `## Moment 3 — Vidéo livrée`.

Tenue au choix du skill, accordée au lieu (pas de costume trois pièces dans un bar de quartier). Le client la lit dans la ligne `Je fixe` : « deux tenues de Jeff : chemise en lin ouverte, puis torse nu en short de bain ».

**Prompt d'une tenue**

En anglais. D'une tenue à l'autre, seule la clause `Outfit` change : même `Subject`, même cadrage, même lumière.

Image sans tête, comme le panneau 1 de la planche : le seul visage de face reste celui de la planche (`## Règles de cohérence`), même quand la vidéo reçoit deux tenues. Une tenue torse nu montre quand même le torse, les bras et les jambes.

```
OUTFIT PHOTO — "<First name>"

@Image 1 is <First name>'s reference sheet: keep exactly this person's body,
with the same build, proportions, skin tone, body hair and marks. Only the
clothing changes. Do not reproduce the grid, the separate panels or the
plain backdrop layout of @Image 1.

Subject: <Subject de la fiche, mot pour mot>

Outfit ("<First name>'s <outfit name>"): <each piece: garment, fabric, colour,
fit, wear; bare skin named as such, e.g. "shirtless, chest and arms as described
in Subject">. <Accessories>. <Shoes, or barefoot>. The fabric shows real
creases, seams and light wear.

Framing: one vertical photograph, front view, full length down to the feet,
standing at ease, arms hanging loosely, feet on the floor, on a plain light-grey
seamless studio background. The head is left out: the figure stops at the
collar and the base of the neck, and above it there is only light-grey
background, as on a headless shop mannequin dressed in the outfit. No hair and
no face show. The image contains only this figure and the plain background.

Style: <la ligne Style de la planche, mot pour mot>

Output: one vertical 9:16 image. Avoid generating any text, subtitles,
watermark or logo.
```

- Taches, usure et marques d'une tenue de travail : décrites comme constantes (« a faded ink stain on the left cuff, always identical »), pas comme un état passager.
- Couvre-chef et lunettes : décrits dans la clause `Outfit` de la tenue qui les porte.

**Appel**

Tenue d'un personnage existant : dans le premier lot de l'atelier, avec lieux et objets. Tenue d'un personnage nouveau : second lot, dès que sa planche passe la relecture (il faut son job id). Requête :
`{model:"gpt_image_2_5", prompt, aspect_ratio:"9:16", quality:"high", resolution:"2k", medias:[{role:"image_references", value:<job id de l'image validée du personnage>}]}`

Prix : chaque tenue estimée avant avec `generate_image {…, get_cost:true}` (~3 crédits), comptée dans la ligne `Je fixe`. Relecture avant de montrer : même carrure, mêmes marques et même teint que la planche ; aucune tête ni visage ; pièces conformes à la clause `Outfit` ; corps entier jusqu'aux pieds dans le cadre. Refus : une seule modification, seule cette tenue est refaite ; premier refus → duel pour cette tenue (`references/choix-modele.md#Duel`, type `personnage`).

**Rangement**

Tenue validée : une ligne dans la fiche du personnage, section `## Tenues` (`templates/fiche-element.md`) :

`<nom court> | <clause Outfit anglaise, mot pour mot> | <job id de l'image validée>`

Exemple : `torse nu, short de bain | Outfit ("Jeff's swim shorts"): shirtless, chest and arms as described in Subject; navy swim shorts, mid-thigh, faded drawstring; barefoot. | job_7f3a…`

Puis une ligne `AAAA-MM-JJ : tenue <nom court>` dans `## Historique` de la fiche, et au journal : `AAAA-MM-JJ | element-cree | tenue=<slug du personnage>:<nom court> | projet=<slug du projet>`. La tenue sert aux projets suivants.
