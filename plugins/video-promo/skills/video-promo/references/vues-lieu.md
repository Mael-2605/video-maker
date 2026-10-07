# Vues du lieu

> Rôle : des images séparées du lieu **vide**, une par position de caméra utile (derrière le comptoir, assis à la table, dehors face à la façade). Elles fixent le décor et la lumière, rien d'autre : jamais de personnage dedans. Le concepteur en fixe le nombre et le prix, le réalisateur écrit leurs prompts, l'atelier les génère, l'agent principal les montre au moment 2 avec les planches et le découpage.
> Anti-slop : `references/anti-slop.md`. Modèles, duel et échecs : `references/choix-modele.md`.

## Principe

Une image où le personnage est déjà placé dans le décor fige sa pose et sa lumière, et le modèle vidéo la recopie. Les tests du 2026-10-02/03 l'ont montré : le meilleur rendu vient des planches des personnages, de leurs tenues, de l'objet clé et de vues du lieu vide, chacune prise d'un autre endroit.

- Une vue montre le lieu vide, depuis une position de caméra que les plans utilisent. Les personnages viennent de leurs planches et de leurs tenues, les objets de leur planche.
- Une vue par image : jamais un collage, un triptyque ni un écran partagé dans une même image.
- Elle part en `image_references`, jamais en `start_image` ni `end_image`. Le mouvement, les gestes et la physique viennent du prompt vidéo.
- Le plan qui se passe à cet endroit la cite en quelques mots (`the counter as in @Image 7`, `from her seat as in @Image 8`).

## Nombre et prix

- Une vue par position de caméra distincte des plans esquissés : on regroupe les plans par l'endroit où se tient la caméra. Un autre cadrage depuis le même endroit (plus serré, plus large) ne compte pas.
- Repère : 1 à 3 pour un clip de 15 s dans un seul lieu ; un second lieu a ses propres vues. Au-delà de 30 s, les segments partagent les mêmes vues. La limite de 9 images de la vidéo (`references/choix-modele.md#Limite de références`) peut réduire le nombre, jamais l'augmenter.
- Le concepteur compte les positions dans `## Histoire` de `vision.md` et fixe le nombre. Prix : `generate_image {model:"nano_banana_pro", prompt, aspect_ratio:"9:16", resolution:"2k", get_cost:true}` (~2 crédits), multiplié par n, compté dans le prix de la ligne `Je fixe`. Il écrit `<n> vue(s), ~<Y> crédits` sous `## Vues du lieu` de `vision.md`, puis une ligne par vue : `vue <N> : <lieu>, <position de la caméra> — plans <a> à <b>`.
- Le réalisateur ne dépasse jamais ce nombre.

## Préparer la base

Gratuit, par l'atelier, avant la première vue d'un lieu. L'image du lieu (`references/fiche-lieu.md`) est une planche de trois panneaux côte à côte : envoyée telle quelle, elle pousse le modèle à refaire une image en panneaux. On la découpe donc en trois images d'un seul cadre.

1. La fiche du lieu a déjà `Panneaux (media ids)` dans `## Identifiants Higgsfield` : on les reprend, rien à faire.
2. Sinon : adresse de l'image validée (`show_generation_by_ids` sur son job id), téléchargement dans le dossier du projet, découpe en trois largeurs égales avec l'outil présent sur la machine (`ffmpeg -i lieu.png -vf "crop=iw/3:ih:<k>*iw/3:0" panneau<k+1>.png` pour k = 0, 1, 2 ; ou `python3` avec PIL ; ou `sips` sur Mac).
3. `media_upload` avec les trois fichiers (`files`), envoi de chaque fichier à son adresse (`curl -X PUT --upload-file`), puis `media_confirm {type:"image", media_ids}`.
4. Les trois media ids dans la fiche du lieu, `## Identifiants Higgsfield`, ligne `Panneaux (media ids) : 1=…, 2=…, 3=…`, pour les projets suivants.

Aucun outil de découpe disponible, ou envoi refusé : la planche entière en référence, et le prompt dit de ne pas reproduire ses panneaux.

## Prompt d'une vue

Écrit par le réalisateur, en anglais, dans `script.md`, section `## Vues du lieu`, un bloc `### Vue <N> — <position en quelques mots>` par image : les plans qu'elle sert, ses références (les panneaux les plus proches de la position voulue, ou les trois), puis le prompt. D'une vue à l'autre, seul le paragraphe `Camera:` change.

```
LOCATION VIEW — "<Name>", view <N>
@Image 1, @Image 2 and @Image 3 show <Name>: keep exactly this place, its
architecture, named materials, furniture and light. Do not reproduce the
layout of the references.
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
- **Position** : celle que prendra la caméra dans les plans, selon `## Plan du lieu` du `script.md` (`references/mise-en-scene.md#Plan du lieu`), dans le même sens que la règle d'orientation.
- **Lumière** : le bloc `Light` de la fiche, mot pour mot, identique dans chaque vue.
- **Cadrage vertical**, assez de décor pour situer l'endroit, rien d'important tout en bas ni tout en haut (couverts par l'interface).

## Dans la vidéo

Ordre des médias et limite : `references/choix-modele.md#Route Seedance`. Dans le paragraphe `References:`, une phrase pour toutes les vues :

```
@Image 7, @Image 8 and @Image 9 are the same bar seen from three places: @Image 7
from behind the counter toward the window, @Image 8 from the small table against
the window looking out at the terrace, @Image 9 from the terrace looking at the
facade. They fix the set and the light only.
```

- Le plan qui se passe à un endroit le dit en tête, en quelques mots : `Shot 1 (0-5s), the counter as in @Image 7:`. Le plan ne redécrit pas le lieu.
- Jamais `keyframe`, `start from`, `opens on`, `match this frame`, `in that order` ni `first frame` : une vue n'est pas un point de départ.

## Relecture

Par l'atelier, en plus de `references/anti-slop.md#Relecture avant de montrer`. Un « non » suffit à refaire la vue :

1. Personne dans l'image, ni derrière une vitre, ni en reflet ?
2. Une seule image, sans panneau, sans collage, sans texte lisible ?
3. Même architecture, mêmes matières, même mobilier que l'image du lieu ?
4. Lumière du même côté et de la même chaleur que dans l'image du lieu ? (Piège connu : la lumière devient plus chaude ou plus froide d'une vue à l'autre.)
5. Vitres et reflets plausibles : la vitre montre ce qu'il y a derrière, sans décor inventé ni reflet d'une autre pièce ? (Piège connu.)
6. La caméra est-elle à la position demandée, tournée du bon côté ?

Échec : `references/choix-modele.md#Échec technique d'une image` (relance muette, duel muet, blocage). Modèle par défaut du type : `nano_banana_pro` ; duel contre `gpt_image_2_5`.

## Enregistrement

Les vues servent à un seul projet : pas de fiche (seuls les panneaux découpés vont dans la fiche du lieu). Juste après la soumission, chaque job id dans `etat.md`, `## Jobs payants`, ligne `vues du lieu (job ids):`, une entrée `vue<N>=<job id>`. Poste `images` dans `cout.md`.

## Corrections

Une seule modification à la fois (`references/script.md#Retouches`). Une vue n'est refaite que si la correction touche ce qu'elle fixe.

- **Plan changé** (action, geste, réplique, cadrage depuis le même endroit) : aucune vue refaite. Un plan qui demande une position de caméra qu'aucune vue ne couvre : une vue de plus, son prix en une ligne.
- **Personnage, tenue ou objet changés** : aucune vue refaite.
- **Lieu changé** : l'image du lieu d'abord, puis toutes ses vues.
- **Style ou lumière changés** : toutes les vues du projet sont refaites.
- **Rendu refusé, vue inchangée** (lumière fausse, reflet bizarre, mauvais angle) : duel comme pour les autres images (`references/choix-modele.md#Duel`), type `vue-lieu`.

Chaque vue refaite : son prix en une ligne `💳 ~X crédits`, sans question, puis on réaffiche la vue et les lignes changées du découpage.
