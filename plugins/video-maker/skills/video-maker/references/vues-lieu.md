# Vues du lieu

> Rôle : des images séparées du lieu **vide**, une par position de caméra utile, faites à partir de l'image de base du lieu (plan large, `references/fiche-lieu.md`). Le concepteur en fixe le nombre et le prix, le réalisateur écrit leurs prompts, l'atelier les génère, l'agent principal les montre au moment 2, après l'image de base.
> Anti-slop : `references/anti-slop.md`. Modèles, duel et échecs : `references/choix-modele.md`.

## Principe

Une image où le personnage est déjà placé dans le décor fige sa pose et sa lumière, et le modèle vidéo la recopie. Les tests du 2026-10-02/03 l'ont montré : le meilleur rendu vient des planches des personnages, de leurs tenues, de l'objet clé et de vues du lieu vide, chacune prise d'un autre endroit.

Chaque cut reçoit deux images du lieu : l'image de base, qui tient le décor autour, et la vue de sa position de caméra, qui tient l'angle. Jamais les autres vues.

- Une vue montre le lieu vide depuis une position que les cuts utilisent. Les personnages viennent de leurs planches et de leurs tenues, les objets de leur planche.
- Une vue par image : jamais un collage, un triptyque ni un écran partagé dans une même image.
- Elle part en `image_references`, jamais en `start_image` ni `end_image`. Le mouvement, les gestes et la physique viennent du prompt vidéo.
- Le cut qui se passe à cet endroit la cite en quelques mots (`the counter as in @Image 5`).

## Nombre et prix

- Une vue par position de caméra distincte des cuts esquissés : on regroupe les cuts par l'endroit où se tient la caméra. Un autre cadrage depuis le même endroit (plus serré, plus large) ne compte pas.
- Repère : 1 à 3 pour un clip de 15 s dans un seul lieu ; un second lieu a ses propres vues. Tous les cuts tournés depuis la même position partagent la même vue. La limite de 9 images de la vidéo (`references/choix-modele.md#Limite de références`) peut réduire le nombre, jamais l'augmenter.
- Le concepteur compte les positions dans `## Histoire` de `vision.md` et fixe le nombre. Prix : `generate_image {model:"nano_banana_pro", prompt, aspect_ratio:"9:16", resolution:"2k", get_cost:true}` (~2 crédits), multiplié par n, compté dans le prix de la ligne `Je fixe`. Il écrit `<n> vue(s), ~<Y> crédits` sous `## Vues du lieu` de `vision.md`, puis une ligne par vue : `vue <N> : <lieu>, <position de la caméra> — cuts <a> à <b>`. L'image de base de chaque lieu est comptée à part, dans l'inventaire (`references/fiche-lieu.md`).
- Le réalisateur ne dépasse jamais ce nombre.

## Image de base

Rien à préparer ni à découper : l'image de base du lieu part telle quelle, par son job id (lieu nouveau : `lieu:<slug>` de `images (job ids):` ; lieu existant : `Image validée (job id)` de sa fiche). L'atelier l'écrit dans `etat.md`, `## Jobs payants`, ligne `bases du lieu (job ids):`, entrée `<slug>=<job id>`, s'il ne l'y trouve pas ; le tournage l'y lit pour chaque cut.

## Prompt d'une vue

Écrit par le réalisateur, en anglais, dans `script.md`, section `## Vues du lieu`, un bloc `### Vue <N> — <position en quelques mots>` par image : les cuts qu'elle sert, ses références (l'image de base, seule), puis le prompt. D'une vue à l'autre, seul le paragraphe `Camera:` change.

```
LOCATION VIEW — "<Name>", view <N>
@Image 1 shows <Name> in a wide shot from far back: keep exactly this place, its
architecture, named materials, furniture and light. Do not reproduce the framing
of the reference.
Location: <bloc Location de la fiche du lieu, mot pour mot>
Light (fixed): <bloc Light de la fiche du lieu, mot pour mot>
Camera: <where the camera stands and at what height, which way it looks, and what
is in frame from foreground to background, e.g. behind the counter at chest
height, looking across the dark oak counter toward the large shopfront window;
the terrace and its green metal tables through the glass; the small table
against the window at the left of frame>.
The place is completely empty: no people anywhere, inside or outside, nobody
behind the glass. Only the fixed furniture and fittings are there.
Style: <ligne image de la famille>
One single vertical 9:16 frame, not a collage, no split panels, no text.
Avoid generating any text, subtitles, watermark or logo.
```

**Ligne image de la famille** (famille de la ligne Style de la carte, `references/script.md#Ouverture`) :

- `cinema` : `photograph taken on a full-frame cinema camera with a vintage prime lens, soft highlight roll-off, fine organic grain`
- `telephone` : `photograph taken on a recent smartphone main camera, the phone's own colour processing, fine sensor noise in the shadows`
- `documentaire` : `photograph taken on a compact documentary camera with a standard zoom lens, natural exposure, fine digital grain`
- `produit` : `photograph taken on a full-frame camera with a macro prime lens, one large soft studio source, crisp true-to-life textures`

Règles :
- **Lieu vide** : aucun personnage, aucun figurant, aucun objet clé du film (le verre, la tasse viennent de leur planche). Le mobilier fixe reste à sa place.
- **Position** : celle que prendra la caméra dans les cuts, selon `## Plan du lieu` du `script.md` (`references/mise-en-scene.md#Plan du lieu`), dans le même sens que la règle d'orientation.
- **Lumière** : le bloc `Light` de la fiche, mot pour mot, identique dans chaque vue.
- **Cadrage vertical**, assez de décor pour situer l'endroit, rien d'important tout en bas ni tout en haut (couverts par l'interface).

## Dans un cut

Ordre des médias et limite : `references/choix-modele.md#Limite de références`. Dans le paragraphe `References:` du cut, une phrase pour les deux images du lieu :

```
@Image 4 shows the whole bar wide, from far back; @Image 5 is the same bar from
behind the counter. They fix the set and the light only.
```

Le plan du cut le dit en tête : `Shot (0-4s), the counter as in @Image 5:`. Il ne redécrit pas le lieu. Jamais `keyframe`, `start from`, `opens on`, `match this frame`, `in that order` ni `first frame`.

## Relecture

Par l'atelier, en plus de `references/anti-slop.md#Relecture avant de montrer`. Un « non » suffit à refaire la vue :

1. Personne dans l'image, ni derrière une vitre, ni en reflet ?
2. Une seule image, sans panneau, sans collage, sans texte lisible ?
3. Même architecture, mêmes matières, même mobilier que l'image de base ?
4. Lumière du même côté et de la même chaleur que dans l'image de base ? (Piège connu : la lumière devient plus chaude ou plus froide d'une vue à l'autre.)
5. Vitres et reflets plausibles : la vitre montre ce qu'il y a derrière, sans décor inventé ni reflet d'une autre pièce ? (Piège connu.)
6. La caméra est-elle à la position demandée, tournée du bon côté ?

Échec : `references/choix-modele.md#Échec technique d'une image` (relance muette, duel muet, blocage). Modèle par défaut du type : `nano_banana_pro` ; duel contre `gpt_image_2_5`.

## Enregistrement

Les vues servent à un seul projet : pas de fiche. Juste après la soumission, chaque job id dans `etat.md`, `## Jobs payants`, ligne `vues du lieu (job ids):`, une entrée `vue<N>=<job id>`. Poste `images` dans `cout.md`.

## Corrections

Une seule modification à la fois (`references/script.md#Retouches`). Une vue n'est refaite que si la correction touche ce qu'elle fixe.

- **Cut corrigé** (action, geste, réplique, cadrage depuis le même endroit) : aucune vue refaite. Un cut qui demande une position de caméra qu'aucune vue ne couvre : une vue de plus, son prix en une ligne.
- **Personnage, tenue ou objet changés** : aucune vue refaite.
- **Lieu changé** : l'image de base d'abord, puis toutes ses vues.
- **Style ou lumière changés** : toutes les vues du projet sont refaites.
- **Rendu refusé, vue inchangée** (lumière fausse, reflet bizarre, mauvais angle) : duel comme pour les autres images (`references/choix-modele.md#Duel`), type `vue-lieu`.

Chaque vue refaite : son prix en une ligne `💳 ~X crédits`, sans question, puis on réaffiche la vue et les lignes changées de la trame.
