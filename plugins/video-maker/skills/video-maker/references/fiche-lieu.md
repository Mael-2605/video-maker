# Fiche lieu

> Rôle : créer un lieu réutilisable (toujours un lieu) : une image de base du lieu **vide**, en plan large pris de loin, et sa fiche dans la bibliothèque. Seulement si le lieu n'existe pas déjà dans `bibliotheque/lieux/`.
> Anti-slop : `references/anti-slop.md` pour le prompt, `references/anti-slop.md#Ton du skill` pour les phrases au client. Modèles, prix et duel : `references/choix-modele.md`.

## Dans l'inventaire

Toujours un lieu : son image de base part dans chaque cut qui s'y passe, avec la vue de la position de caméra du cut (`references/vues-lieu.md`). La fiche fixe forme, échelle et matière : pour un extérieur, hauteur et profil des sommets, arête, pente et son angle, neige ou glace, végétation ; pour un intérieur, disposition, matières, mobilier fixe. Un second lieu a sa propre fiche. Recherche dans la bibliothèque : `references/fiche-personnage.md#Dans l'inventaire`.

**Fiche plus ancienne dont l'image est une planche en trois parties** (16:9, une ligne `Panneaux` dans `## Identifiants Higgsfield`) : le lieu est repris sous son nom, blocs `Location` et `Light` mot pour mot, mais son image de base est refaite en plan large avec ce gabarit (statut `nouveau`, choix « plan large depuis la fiche », prix dans `Je fixe`). L'ancienne planche n'entre jamais dans une vue ni dans un cut. À l'enregistrement de la nouvelle image de base, la fiche est mise à jour, pas recréée : son job id remplace celui de `Image validée (job id)`, les lignes `Panneaux` et `Layout` sont supprimées, et `## Historique` reçoit `AAAA-MM-JJ : image de base en plan large, remplace la planche en trois parties`. Un projet suivant reprend cette image de base sans la repayer.

## Questions au client

Aucune question séparée. Le concepteur choisit disposition, matières, mobilier, lumière et heure, et les résume dans `Je fixe` (« le bar, bois sombre, soleil bas par la vitrine »). Photos d'un lieu réel : `media_upload_widget` par l'agent principal, seul outil de ce tour, avant la carte ; jamais de chemin local.

## Template du prompt

En anglais, un seul bloc, sections dans cet ordre. `Location` (avec ses matières nommées) et `Light` sont écrits une fois, puis figés : ils sont recopiés mot pour mot dans la fiche (`## Description figée`) et dans chaque prompt d'image suivant, dont les vues du lieu (`references/vues-lieu.md#Prompt d'une vue`). La vidéo n'en garde qu'une proposition dans son ouverture (`references/script.md#Ouverture`).

Le décor est **vide** : personne dedans. Une seule image horizontale (16:9), un plan large pris de loin, qui montre les alentours de l'endroit où la scène se joue : murs, porte, fenêtres, mobilier, lumière. Le personnage vient de sa fiche, les objets de la leur.

```
LOCATION BASE — "<Name>"

Location: <Name>, <kind of place, size, layout: where the entrance, the window,
the counter or the desk are>. Named materials, always identical:
- <Surface> (call it "<the named material>"): <material, colour, finish, wear>.
- <Surface> (call it "<the named material>"): <…>.
<Fixed furniture and fittings, each with its position in the room>.

Light (fixed): <source, direction, time of day, colour, shadows>, e.g. low
late-afternoon sun from camera-left through the street-facing window, warm,
long soft shadows across the counter.

Camera: a wide shot taken from far back, <from the corner opposite the spot
where the action happens, or from across the street>, at chest height, standard
lens, showing the whole place around that spot: <the walls, the entrance door,
the windows, the fixed furniture>, with <the spot, e.g. the stretch of counter
in front of the coffee machine> clearly visible in the middle of the frame.

The place is completely empty: no people anywhere, inside or outside, nobody
behind the glass. Only the fixed furniture and fittings are there.

Style: unretouched photograph taken on a phone at chest height, natural colour,
fine sensor grain, straight vertical lines, standard lens perspective. Looks
like a photo of a real place on an ordinary day, not an architecture render.

One single horizontal 16:9 frame, not a collage, no split panels.
Signs, menus, labels and posters are blank or turned away, so that no text is
readable. Avoid generating any text, subtitles, watermark or logo.
```

## Règles de cohérence

- **La référence image fait le décor.** Un lieu ne reste identique d'un cut à l'autre et d'une vidéo à l'autre que par son image de base : elle sert de référence à chaque vue et part dans chaque cut qui s'y passe (`references/vues-lieu.md`). Le texte seul ne suffit pas.
- **Lumière figée dans la fiche** : source, direction, heure, couleur, ombres. Elle est reprise mot pour mot dans chaque vue, et en quelques mots dans l'ouverture de la vidéo. Une autre heure ou une autre lumière demande une nouvelle image du lieu, pas seulement un autre texte.
- **Lumière de la carte** : le bloc `Light` d'un lieu nouveau reprend la ligne Lumière de la carte vision (`vision.md`, `## Carte`), traduite en source, direction, heure, couleur et ombres, pour que le décor, les vues et la vidéo partagent la même lumière. Lieu existant : la carte reprend sa lumière figée, sauf demande du client ; une autre lumière demandée donne une nouvelle image du lieu. Les fiches personnage et objet restent sur fond neutre, lumière de studio.
- **Forme et échelle** : un extérieur nomme dans `Location` le relief qui se voit (hauteur et profil du sommet, arête étroite ou large, pente en degrés, neige ou glace) ; le modèle ne l'invente plus d'un plan à l'autre.
- **Décor vide.** Jamais de figurant, de client assis au fond ou de passant dans la vitrine. Formulé en positif (« the place is empty, with nobody inside »), voir `references/anti-slop.md#Exclusions en positif`.
- **Matières nommées** (« the dark oak counter », « the rough stone wall ») : reprises mot pour mot dans chaque prompt d'image.
- **Aucun texte lisible** : enseignes, menus, affiches et étiquettes vides ou tournés.
- **Mots interdits** : jamais `moody`, `luxury`, `beautiful`, `stunning` pour décrire l'ambiance ; nommer la source de lumière et les matières. Voir `references/anti-slop.md#Mots interdits dans les prompts`.
- **Correction** : une seule modification à la fois (« le bar plus sombre » → une lumière plus basse ou des matières plus foncées, pas les deux), et seul ce lieu est refait.

Relecture avant de montrer, en plus de `references/anti-slop.md#Relecture avant de montrer` : une seule image, sans panneau ? Voit-on le lieu entier autour de l'endroit de la scène (murs, porte, fenêtres, mobilier) ? Y a-t-il une personne, ou un texte lisible ? Si oui, refaire.

## Modèle et appel

1. Modèle : `nano_banana_pro`, revérifié en début de session (voir `references/choix-modele.md#Vérification en début de session`).
2. Prix : `generate_image {model:"nano_banana_pro", prompt, aspect_ratio:"16:9", resolution:"2k", get_cost:true}` (~2 crédits). Prix compté dans la ligne `Je fixe`.
3. Lieux, objets et tenues nouvelles (`references/fiche-personnage.md#Tenues`) à créer pour le même projet partent ensemble : estimer chacun avec `generate_image {get_cost:true}`, puis un seul `generate_image_batch` par l'atelier (second lot seulement pour les tenues d'un personnage nouveau). Avec des photos du lieu : `medias: [{role:"image_references", value:<media id>}]`.
4. `jobs_wait`, relecture ; l'agent principal montre toutes les images au moment 2, l'image de base de chaque lieu en premier.
5. Échec technique : `references/choix-modele.md#Échec technique d'une image`. Refus du client : duel (`references/choix-modele.md#Duel`). Nano Banana Pro n'est pas encore éprouvé sur les lieux : les duels notés au journal diront quel modèle garder.

## Enregistrement

Par l'atelier, dès que l'image passe la relecture.

1. **Dossier** : `bibliotheque/lieux/<slug>/`. Slug = le nom complet, normalisé : minuscules, accents retirés, tout caractère autre qu'une lettre ou un chiffre remplacé par un tiret. « bar du Rhône » → `bar-du-rhone`. Règle de slug de SKILL.md.
2. **Fichier** `fiche.md`, copié de `templates/fiche-element.md` et rempli ainsi :
   - Titre : le nom du lieu. `Type : lieu`. `Créé le :` date du jour.
   - `## Description figée` : les blocs `Location` et `Light`, tels qu'envoyés au modèle.
   - `## Tenues` : supprimer cette section.
   - `## Identifiants Higgsfield` : job id de l'image de base validée (`Image validée (job id)`) ; `Element id` vide sauf si le lieu existe comme Element sur le compte.
   - `## Historique` : `AAAA-MM-JJ : création`.
3. **Journal** : ajouter une ligne à `bibliotheque/journal.md`, au format de `references/apprentissage.md#Format des lignes` :
   `AAAA-MM-JJ | element-cree | lieu=<slug> | projet=<slug du projet>`
