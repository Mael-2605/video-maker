# Fiche lieu

> Rôle : créer un lieu réutilisable pour chaque projet (toujours un lieu) (bar, bureau, cuisine, terrasse…) : planche de référence du décor vide et fiche dans la bibliothèque. Seulement si le lieu n'existe pas déjà dans `bibliotheque/lieux/`.
> Anti-slop : `references/anti-slop.md` pour le prompt, `references/anti-slop.md#Ton du skill` pour les phrases au client. Modèles, prix et duel : `references/choix-modele.md`.

## Dans l'inventaire

Toujours un lieu : la vidéo tient son décor par les vues du lieu vide (`references/vues-lieu.md`), faites à partir de son image. La fiche fixe forme, échelle et matière : pour un extérieur, hauteur et profil des sommets, arête, pente et son angle, neige ou glace, végétation ; pour un intérieur, disposition, matières, mobilier fixe. Un second lieu (voiture d'une accroche) a sa propre fiche. Recherche dans la bibliothèque : `references/fiche-personnage.md#Dans l'inventaire`.

## Questions au client

Aucune question séparée. Le concepteur choisit disposition, matières, mobilier, lumière et heure, et les résume dans `Je fixe` (« le bar, bois sombre, soleil bas par la vitrine »). Photos d'un lieu réel : `media_upload_widget` par l'agent principal, seul outil de ce tour, avant la carte ; jamais de chemin local.

## Template du prompt

En anglais, un seul bloc, sections dans cet ordre. `Location` (avec ses matières nommées) et `Light` sont écrits une fois, puis figés : ils sont recopiés mot pour mot dans la fiche (`## Description figée`) et dans chaque prompt d'image suivant, dont les vues du lieu (`references/vues-lieu.md#Prompt d'une vue`). La vidéo n'en garde qu'une proposition dans son ouverture (`references/script.md#Ouverture`).

Le décor est **vide** : personne dedans. Le personnage vient de sa fiche, les objets de la leur. La planche est horizontale (16:9) et contient trois panneaux verticaux côte à côte, chacun cadré comme la vidéo.

```
LOCATION REFERENCE SHEET — "<Name>"

Location: <Name>, <kind of place, size, layout: where the entrance, the window,
the counter or the desk are>. Named materials, always identical:
- <Surface> (call it "<the named material>"): <material, colour, finish, wear>.
- <Surface> (call it "<the named material>"): <…>.
<Fixed furniture and fittings, each with its position in the room>.

Light (fixed): <source, direction, time of day, colour, shadows>, e.g. low
late-afternoon sun from camera-left through the street-facing window, warm,
long soft shadows across the counter.

Layout: one single wide image made of three equal vertical panels side by side,
each panel framed like a vertical phone video. The place is empty, with nobody
inside: the room holds only its fixed furniture and fittings.
1. Wide shot of the whole room from the entrance, at chest height.
2. Reverse angle: the same room seen from the opposite side, looking back
   toward the entrance.
3. Medium shot of the exact spot where the action happens: <e.g. the stretch of
   counter in front of the coffee machine>, at the eye height of a person
   standing there.

Consistency rules: the same architecture, furniture positions, named materials
and colours in all three panels. The light comes from the same source, in the
same direction, at the same time of day in every panel, and the shadows fall
the same way.

Style: unretouched photograph taken on a phone at chest height, natural colour,
fine sensor grain, straight vertical lines, standard lens perspective. Looks
like a photo of a real place on an ordinary day, not an architecture render.

Output: one horizontal 16:9 image, three vertical panels.
Signs, menus, labels and posters are blank or turned away, so that no text is
readable. Avoid generating any text, subtitles, watermark or logo.
```

## Règles de cohérence

- **La référence image fait le décor.** Un lieu ne reste identique d'une vidéo à l'autre que par ses images : ses panneaux servent de base à chaque vue, et les vues entrent dans la vidéo (`references/vues-lieu.md`). Le texte seul ne suffit pas.
- **Lumière figée dans la fiche** : source, direction, heure, couleur, ombres. Elle est reprise mot pour mot dans chaque vue, et en quelques mots dans l'ouverture de la vidéo. Une autre heure ou une autre lumière demande une nouvelle image du lieu, pas seulement un autre texte.
- **Lumière de la carte** : le bloc `Light` d'un lieu nouveau reprend la ligne Lumière de la carte vision (`vision.md`, `## Carte`), traduite en source, direction, heure, couleur et ombres, pour que le décor, les vues et la vidéo partagent la même lumière. Lieu existant : la carte reprend sa lumière figée, sauf demande du client ; une autre lumière demandée donne une nouvelle image du lieu. Les fiches personnage et objet restent sur fond neutre, lumière de studio.
- **Forme et échelle** : un extérieur nomme dans `Location` le relief qui se voit (hauteur et profil du sommet, arête étroite ou large, pente en degrés, neige ou glace) ; le modèle ne l'invente plus d'un plan à l'autre.
- **Décor vide.** Jamais de figurant, de client assis au fond ou de passant dans la vitrine. Formulé en positif (« the place is empty, with nobody inside »), voir `references/anti-slop.md#Exclusions en positif`.
- **Matières nommées** (« the dark oak counter », « the rough stone wall ») : reprises mot pour mot dans chaque prompt d'image.
- **Aucun texte lisible** : enseignes, menus, affiches et étiquettes vides ou tournés.
- **Planche découpée** avant les vues : ses trois panneaux deviennent trois images d'un seul cadre (`references/vues-lieu.md#Préparer la base`).
- **Mots interdits** : jamais `moody`, `luxury`, `beautiful`, `stunning` pour décrire l'ambiance ; nommer la source de lumière et les matières. Voir `references/anti-slop.md#Mots interdits dans les prompts`.
- **Correction** : une seule modification à la fois (« le bar plus sombre » → une lumière plus basse ou des matières plus foncées, pas les deux), et seul ce lieu est refait.

Relecture avant de montrer, en plus de `references/anti-slop.md#Relecture avant de montrer` : les trois panneaux montrent-ils le même lieu, avec la lumière du même côté ? Y a-t-il une personne, ou un texte lisible ? Si oui, refaire.

## Modèle et appel

1. Modèle : `nano_banana_pro`, revérifié en début de session (voir `references/choix-modele.md#Vérification en début de session`).
2. Prix : `generate_image {model:"nano_banana_pro", prompt, aspect_ratio:"16:9", resolution:"2k", get_cost:true}` (~2 crédits). Prix compté dans la ligne `Je fixe`.
3. Lieux, objets et tenues nouvelles (`references/fiche-personnage.md#Tenues`) à créer pour le même projet partent ensemble : estimer chacun avec `generate_image {get_cost:true}`, puis un seul `generate_image_batch` par l'atelier (second lot seulement pour les tenues d'un personnage nouveau). Avec des photos du lieu : `medias: [{role:"image_references", value:<media id>}]`.
4. `jobs_wait`, relecture ; l'agent principal montre toutes les images ensemble au moment 2.
5. Échec technique : `references/choix-modele.md#Échec technique d'une image`. Refus du client : duel (`references/choix-modele.md#Duel`). Nano Banana Pro n'est pas encore éprouvé sur les lieux : les duels notés au journal diront quel modèle garder.

## Enregistrement

Par l'atelier, dès que l'image passe la relecture.

1. **Dossier** : `bibliotheque/lieux/<slug>/`. Slug = le nom complet, normalisé : minuscules, accents retirés, tout caractère autre qu'une lettre ou un chiffre remplacé par un tiret. « bar du Rhône » → `bar-du-rhone`. Règle de slug de SKILL.md.
2. **Fichier** `fiche.md`, copié de `templates/fiche-element.md` et rempli ainsi :
   - Titre : le nom du lieu. `Type : lieu`. `Créé le :` date du jour.
   - `## Description figée` : les blocs `Location` et `Light`, tels qu'envoyés au modèle.
   - `## Tenues` : supprimer cette section.
   - `## Identifiants Higgsfield` : job id de l'image validée ; `Panneaux (media ids)` une fois la planche découpée (`references/vues-lieu.md#Préparer la base`) ; `Element id` vide sauf si le lieu existe comme Element sur le compte.
   - `## Historique` : `AAAA-MM-JJ : création`.
3. **Journal** : ajouter une ligne à `bibliotheque/journal.md`, au format de `references/apprentissage.md#Format des lignes` :
   `AAAA-MM-JJ | element-cree | lieu=<slug> | projet=<slug du projet>`
