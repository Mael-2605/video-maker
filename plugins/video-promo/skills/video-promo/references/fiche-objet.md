# Fiche objet

> Rôle : créer un objet réutilisable quand l'inventaire le demande (tasse, stylo, dossier, téléphone…) : planche de référence et fiche dans la bibliothèque. Seulement si l'objet n'existe pas déjà dans `bibliotheque/objets/`.
> Anti-slop : `references/anti-slop.md` pour le prompt, `references/anti-slop.md#Ton du skill` pour les phrases au client. Modèles, prix et duel : `references/choix-modele.md`.

## Dans l'inventaire

Un objet entre dans l'inventaire s'il porte un plan (gros plan, geste central, objet d'une accroche visuelle) ou reste en main sur plusieurs plans : le piolet, oui ; un mousqueton accroché au baudrier, non. Les détails qu'on ne remarquera pas restent quelques mots dans le plan, ou hors du prompt. Plusieurs objets au-delà de la limite de références : planche commune (`references/choix-modele.md#Limite de références`). Recherche dans la bibliothèque : `references/fiche-personnage.md#Dans l'inventaire`.

## Questions au client

Aucune question séparée. Le concepteur choisit taille, matières, couleurs, usure, état et orientation, et les résume dans la ligne `Je fixe` (« la tasse en grès blanc, bord ébréché »). Photos du vrai objet : `media_upload_widget` par l'agent principal, seul outil de ce tour, avant la carte ; lui dire en une phrase que les photos doivent montrer l'objet sous tous ses angles (le modèle invente ce qu'il ne voit pas).

## Template du prompt

En anglais, un seul bloc, sections dans cet ordre. `Object` (avec ses matières nommées) et `State` sont écrits une fois, puis figés : ils sont recopiés mot pour mot dans la fiche (`## Description figée`) et dans chaque prompt d'image suivant. La vidéo ne nomme l'objet qu'en quelques mots dans `References:` (`@Image 6 is the spritz, an orange aperitif in a stemmed wine glass`, `references/script.md#Version prompt`).

La planche est verticale (9:16) : trois rangées de trois panneaux, chaque panneau est un cadre vertical.

```
OBJECT REFERENCE SHEET — "<Name>"

Object: <Name>, <what it is>, about <size in cm>. <N> clearly contrasting
materials, always identical in every panel:
- <Part> (call it "<the named material>"): <material, exact colour, finish,
  wear, marks, and where they are>.
- <Part> (call it "<the named material>"): <…>.
<Integral small parts: handle, lid, hinge, clip, fixed cord — described here>.

State: <the one fixed state: full or empty, open or closed, screen off>,
identical in every panel.

Layout: one single vertical image, a reference sheet of the <object> seen from
every side, shot against a seamless light-grey studio backdrop. Nothing but the
<object> exists in this image: in each panel it stands by itself on the bare
backdrop. Nine equal panels in three rows of three, each
panel a vertical frame.
Panels 1 to 4 — the whole <object>, at one scale and one position, <fixed
orientation in words, e.g. "standing upright on its base, handle pointing right">:
1. Front view.
2. Back view.
3. Side view showing the thickness.
4. Top-down view.
5. Three-quarter view of the whole <object>, slightly from above, all named
   materials visible together.
Panels 6 to 9 — tight macro close-ups, one per material or junction:
6. <surface of the first named material>.
7. <the part a hand would hold>.
8. <the junction between two named materials>.
9. <the base or the end>.

Consistency rules: across all nine panels it is one and the same object, with
identical shape, curves, proportions, <grain or pattern>, finish and colour, and
every mark or chip in the same place. One studio setup for the whole sheet: a
big diffused light on camera-left, a white bounce card on camera-right, a single
white balance.

Style: unretouched catalogue photograph of a real object, full-frame camera,
100mm macro lens for the close-ups and 50mm for the full views, f/8, sharp focus
everywhere, natural colour, fine grain. Reflections and materials behave as they
would on the real object. Looks like a photo from a museum catalogue, not a 3D
render.

Output: one vertical 9:16 image, each of the nine panels itself a vertical frame.
Avoid generating any text, subtitles, watermark or logo.
```

Pas de bloc `NEGATIVE PROMPT` : la phrase « Nothing but the <object> exists in this image » fait ce travail en positif (voir `references/anti-slop.md#Exclusions en positif`).

## Règles de cohérence

- **Objet seul.** Aucune main, règle, support ou table. La taille est donnée dans `Object`, jamais par un accessoire ni une annotation.
- **Parties intégrantes** (anse, couvercle, charnière, clip, cordon fixé) : décrites dans `Object`, jamais exclues.
- **Un seul état** pour toute la planche, écrit dans `State` (tasse pleine, dossier fermé, écran éteint). Un autre état se fait plus tard, par une retouche de l'image validée passée en référence (« keep everything the same, but the cup is empty »).
- **Orientation fixée en toutes lettres** pour les vues 1 à 4.
- **Matières nommées** (« the matte white stoneware body », « the chipped rim ») : reprises mot pour mot dans chaque prompt, y compris la vidéo.
- **Forme, matière, état, usure.** Jamais `luxury`, `premium`, `elegant` ni un adjectif de prestige : voir `references/anti-slop.md#Mots interdits dans les prompts`.
- **Surfaces qui brillent** (métal, verre, écran) : grande source diffuse à gauche, carton blanc à droite, écrit dans `Consistency rules`. Dans les prompts vidéo, ajouter « the <named material> reflects the surrounding scene ».
- **Aucun texte lisible** : ni étiquette, ni cote, ni flèche, ni logo. Un objet d'une marque réelle se décrit par sa forme et sa matière, sans le logo.
- **Correction** : une seule modification à la fois, et seul cet objet est refait.

Relecture avant de montrer, en plus de `references/anti-slop.md#Relecture avant de montrer` :

| Défaut sur le rendu | Correction du prompt |
|---|---|
| Quatre vues en ligne, pas de gros plans matière | Neuf panneaux : vues 1 à 5, une macro par matière ou jonction |
| Nuancier, cote « env. 10 cm », étiquette | Taille dans `Object` seulement, phrase de fin présente |
| Main, support ou table pour l'échelle | « Nothing but the <object> exists in this image » dans `Layout` |
| L'objet change d'état d'un panneau à l'autre | Une ligne `State`, un seul état |
| Reflets confus sur le métal ou le verre | Éclairage décrit, source diffuse à gauche et carton blanc à droite |

## Modèle et appel

1. Modèle : `nano_banana_pro`, revérifié en début de session (voir `references/choix-modele.md#Vérification en début de session`). GPT Image (`gpt_image_2_5`) n'intervient que dans le duel, ou s'il a gagné nettement les duels d'objets (voir `references/choix-modele.md#Modèles image`).
2. Prix : `generate_image {model:"nano_banana_pro", prompt, aspect_ratio:"9:16", resolution:"2k", get_cost:true}` (~2 crédits). Prix compté dans la ligne `Je fixe`.
3. Lieux, objets et tenues nouvelles (`references/fiche-personnage.md#Tenues`) à créer pour le même projet partent ensemble : estimer chacun avec `generate_image {get_cost:true}`, puis un seul `generate_image_batch` par l'atelier (second lot seulement pour les tenues d'un personnage nouveau). Avec des photos de l'objet : `medias: [{role:"image_references", value:<media id>}]`.
4. `jobs_wait`, relecture ; l'agent principal montre toutes les images ensemble au moment 2.
5. Échec technique : `references/choix-modele.md#Échec technique d'une image`. Refus du client : duel (`references/choix-modele.md#Duel`).

## Enregistrement

Par l'atelier, dès que l'image passe la relecture.

1. **Dossier** : `bibliotheque/objets/<slug>/`. Slug = le nom complet, normalisé : minuscules, accents retirés, tout caractère autre qu'une lettre ou un chiffre remplacé par un tiret. « tasse en céramique » → `tasse-en-ceramique`. Règle de slug de SKILL.md.
2. **Fichier** `fiche.md`, copié de `templates/fiche-element.md` et rempli ainsi :
   - Titre : le nom de l'objet. `Type : objet`. `Créé le :` date du jour.
   - `## Description figée` : les blocs `Object` et `State`, tels qu'envoyés au modèle.
   - `## Tenues` : supprimer cette section.
   - `## Identifiants Higgsfield` : job id de l'image validée ; `Element id` vide sauf si l'objet existe comme Element sur le compte.
   - `## Historique` : `AAAA-MM-JJ : création`.
3. **Journal** : ajouter une ligne à `bibliotheque/journal.md`, au format de `references/apprentissage.md#Format des lignes` :
   `AAAA-MM-JJ | element-cree | objet=<slug> | projet=<slug du projet>`
