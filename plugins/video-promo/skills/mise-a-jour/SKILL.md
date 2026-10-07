---
name: mise-a-jour
description: "Met à jour le plugin video-promo vers sa dernière version. À utiliser quand l'utilisateur demande de mettre à jour le plugin, l'outil vidéo ou video-promo (« mets à jour le plugin », « il y a une nouvelle version ? », « update video-promo »). Pas pour corriger une vidéo."
---

# Mise à jour du plugin

**1. Lancer la mise à jour**, sans rien demander (c'est gratuit et ne touche pas au dossier de travail) :

```bash
C="${CLAUDE_CODE_EXECPATH:-claude}"
"$C" plugin marketplace update video-promo-marketplace && "$C" plugin update video-promo@video-promo-marketplace
```

`CLAUDE_CODE_EXECPATH` donne le programme Claude de l'application, même quand la commande `claude` n'existe pas dans le terminal.

**2. Répondre en une ou deux phrases**, selon la sortie :

- `already at the latest version (X)` : « Vous avez déjà la dernière version (X). »
- Mise à jour faite : « C'est fait, vous passez à la version X. Fermez puis rouvrez Claude pour l'utiliser. » La nouvelle version ne sert qu'après ce redémarrage.
- Erreur réseau ou GitHub : « Je n'arrive pas à joindre le serveur des mises à jour. Vérifiez votre connexion internet et redemandez-moi dans un moment. »
- `not found` ou marketplace inconnue : le plugin n'a pas été installé depuis le dépôt. Donner les deux commandes d'installation :
  ```
  /plugin marketplace add Mael-2605/video-promo
  /plugin install video-promo@video-promo-marketplace
  ```
- Programme Claude introuvable : demander de taper `/plugin`, d'ouvrir les marketplaces, de choisir `video-promo-marketplace` et d'y lancer la mise à jour, puis de fermer et rouvrir Claude.

Personnages, lieux, projets et préférences restent dans `bibliotheque/` : une mise à jour ne les modifie pas. Le dire seulement si l'utilisateur s'en inquiète.
