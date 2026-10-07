# video-promo — installation

Ce plugin Claude Code crée des vidéos courtes et verticales pour les réseaux sociaux (Reels, TikTok, LinkedIn), avec un personnage qui parle. Le sujet, c'est vous qui le donnez : un produit, un service, un événement, une idée.

## Ce que fait l'outil

Vous décrivez votre vidéo et l'outil vous répond en trois moments. D'abord une courte vision : le style, ce qu'il va fixer en image (personnages, tenues, lieu, objets) avec le prix, et trois versions de la vidéo au choix, chacune racontée en quelques phrases, du début à la fin. Ensuite les images et le découpage plan par plan, avec le prix de la vidéo : rien n'est lancé sans votre « oui ». Enfin la vidéo, que vous gardez ou faites corriger, puis les sous-titres. Une vidéo fait 15 secondes par défaut ; au-delà de 30 secondes, l'outil la tourne en plusieurs parties et les assemble lui-même.

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
/plugin marketplace add Mael-2605/video-promo
/plugin install video-promo@video-promo-marketplace
```

## Utilisation

Il suffit de demander, par exemple :

> Fais-moi une vidéo avec Jeff, un barman, qui présente le nouveau cocktail de la maison.

Répondez à la vision en choisissant une accroche, ou dites ce qu'il faut changer. Au deuxième moment, vous voyez les images des personnages, de leurs tenues, des objets et du lieu vu de plusieurs endroits, puis le découpage, une phrase par plan qui dit ce qui s'y passe ; dites « oui » pour lancer la vidéo, ou demandez une seule modification à la fois. Il n'y a rien à régler avant la première vidéo. L'outil vous vouvoie ; si vous le tutoyez, il vous tutoie.

Il n'y a pas de voix à choisir : la voix vient avec la vidéo, en français. Si elle ne vous va pas (« plus grave », « plus jeune »), dites-le après la vidéo et l'outil la corrige.

Les personnages, lieux et objets déjà créés sont repris tels quels dans les vidéos suivantes. Si un personnage ou une tenue manque, l'outil les prépare avant la vidéo.

## Vos préférences

Dans votre dossier de travail, `bibliotheque/preferences.md` garde vos choix : le dernier style de sous-titres, et ce que l'outil a appris de vos goûts au fil des vidéos.

Vous pouvez ouvrir et modifier ce fichier à tout moment.

## Coûts

Les images sont annoncées avec leur prix dans la vision et lancées quand vous répondez. La vidéo, même en plusieurs parties, a un seul prix et attend votre « oui ».

## En cas de souci

- Si l'outil dit que Higgsfield n'est pas connecté, reprenez l'étape 1 ci-dessus.
- L'outil n'écrit ni faux témoignage, ni promesse de résultat mensongère, ni message qui fait peur. Si une demande va dans ce sens, il propose une autre formulation.
- Si une réponse est refusée ou bloquée, l'outil vous explique pourquoi et propose une reformulation.
- Pour toute autre question, contactez votre prestataire.
