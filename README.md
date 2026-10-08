# video-maker — installation

Ce plugin Claude Code crée des vidéos courtes et verticales pour les réseaux sociaux (Reels, TikTok, LinkedIn), avec un personnage qui parle. Le sujet, c'est vous qui le donnez : un produit, un service, un événement, une idée.

## Ce que fait l'outil

Vous décrivez votre vidéo et l'outil vous répond en quatre moments. D'abord une courte vision : le style, ce qu'il va fixer en image (personnages, tenues, lieu, objets) avec le prix, et trois versions de la vidéo au choix, chacune racontée en quelques phrases. Ensuite les images et la trame, un cut par ligne, avec le prix de toute la vidéo : rien n'est lancé sans votre « oui ». Puis les cuts arrivent un par un : vous gardez chacun, ou le faites corriger avant de passer au suivant. Enfin la vidéo montée, que vous gardez ou faites corriger, puis les sous-titres. Une vidéo fait 15 secondes par défaut.

## Installation

### 1. Connecter Higgsfield

L'outil utilise Higgsfield pour créer images et vidéos. Il faut connecter ce service une seule fois, dans Claude Desktop :

1. Ouvrez Réglages, puis Connecteurs.
2. Cliquez sur « Ajouter un connecteur personnalisé ».
3. Nom : `Higgsfield`. URL : `https://mcp.higgsfield.ai/mcp`
4. Cliquez sur Connecter, puis connectez-vous à votre compte Higgsfield.

### 2. Ouvrir un dossier de travail

Dans l'onglet Code, ouvrez un dossier dédié à vos vidéos, par exemple `Documents/Videos-promo`. C'est là que l'outil gardera vos personnages, vos lieux et vos projets.

### 3. Installer le plugin

Toujours dans l'onglet Code, tapez ces deux commandes l'une après l'autre :

```
/plugin marketplace add Mael-2605/video-maker
/plugin install video-maker@video-maker-marketplace
```

Fermez puis rouvrez Claude pour que le plugin soit pris en compte.

### 4. Mettre à jour

Dites simplement à Claude :

> Mets à jour le plugin video-maker.

Claude installe la dernière version, ou vous dit que vous l'avez déjà. Après une mise à jour, fermez et rouvrez Claude. Vos personnages, lieux, projets et préférences ne bougent pas : ils restent dans votre dossier de travail.

Pour ne plus y penser : tapez `/plugin`, ouvrez l'onglet des marketplaces, choisissez `video-maker-marketplace` et activez la mise à jour automatique. Claude vérifie alors à chaque démarrage.

### 5. Garder l'ancienne version (vidéo entière d'un coup)

La version 0.6.0 tourne la vidéo entière en une fois, sans passer par les cuts. Elle ne reçoit plus d'évolution, mais reste installable. Dans l'onglet Code, tapez ces trois commandes l'une après l'autre :

```
/plugin marketplace remove video-maker-marketplace
/plugin marketplace add Mael-2605/video-maker#video-entiere
/plugin install video-maker@video-maker-marketplace
```

La première commande retire la version actuelle ; vos personnages, lieux, projets et préférences ne bougent pas. Fermez puis rouvrez Claude. Pour revenir à la version cut par cut, refaites les mêmes commandes sans `#video-entiere`.

## Utilisation

Il suffit de demander, par exemple :

> Fais-moi une vidéo avec Jeff, un barman, qui présente le nouveau cocktail de la maison.

Répondez à la vision en choisissant une accroche, ou dites ce qu'il faut changer. Au deuxième moment, vous voyez le lieu pris de loin, puis vu de plusieurs endroits, les personnages, leurs tenues et les objets, puis la trame, une ligne par cut ; dites « oui » pour lancer le premier cut. Chaque cut arrive ensuite seul : « on garde » lance le suivant, ou demandez une seule modification à la fois. Il n'y a rien à régler avant la première vidéo. L'outil vous vouvoie ; si vous le tutoyez, il vous tutoie.

Il n'y a pas de voix à choisir : la voix vient avec la vidéo, en français, et reste la même d'un cut à l'autre. Si elle ne vous va pas (« plus grave », « plus jeune »), dites-le après la vidéo et l'outil la corrige.

Les personnages, lieux et objets déjà créés sont repris tels quels dans les vidéos suivantes. Si un personnage ou une tenue manque, l'outil les prépare avant la vidéo.

## Vos préférences

Dans votre dossier de travail, `bibliotheque/preferences.md` garde vos choix : le dernier style de sous-titres, et ce que l'outil a appris de vos goûts au fil des vidéos.

Vous pouvez ouvrir et modifier ce fichier à tout moment.

## Coûts

Les images sont annoncées avec leur prix dans la vision et lancées quand vous répondez. La vidéo a un prix global, annoncé avec la trame, qui couvre tous les cuts ; il attend votre « oui ». Refaire un cut a son propre prix, annoncé avant, et attend aussi votre « oui ».

## En cas de souci

- Si l'outil dit que Higgsfield n'est pas connecté, reprenez l'étape 1 ci-dessus.
- L'outil n'écrit ni faux témoignage, ni promesse de résultat mensongère, ni message qui fait peur. Si une demande va dans ce sens, il propose une autre formulation.
- Si une réponse est refusée ou bloquée, l'outil vous explique pourquoi et propose une reformulation.
- Pour toute autre question, contactez votre prestataire.
