---
name: mise-a-jour
description: "Met à jour le plugin video-maker vers sa dernière version. À utiliser quand l'utilisateur demande de mettre à jour le plugin, l'outil vidéo ou video-maker (« mets à jour le plugin », « il y a une nouvelle version ? », « update video-maker »). Pas pour corriger une vidéo."
---

# Mise à jour du plugin

**1. Lancer la mise à jour**, sans rien demander (c'est gratuit et ne touche pas au dossier de travail) :

```bash
C="${CLAUDE_CODE_EXECPATH:-claude}"
"$C" plugin marketplace update video-maker-marketplace && "$C" plugin update video-maker@video-maker-marketplace
```

`CLAUDE_CODE_EXECPATH` donne le programme Claude de l'application, même quand la commande `claude` n'existe pas dans le terminal.

**2. Répondre en une ou deux phrases**, selon la sortie :

- `already at the latest version (X)` : « Vous avez déjà la dernière version (X). »
- Mise à jour faite : « C'est fait, vous passez à la version X. Fermez puis rouvrez Claude pour l'utiliser. » La nouvelle version ne sert qu'après ce redémarrage.
- Erreur réseau ou GitHub : « Je n'arrive pas à joindre le serveur des mises à jour. Vérifiez votre connexion internet et redemandez-moi dans un moment. »
- `not found` ou marketplace inconnue : le plugin n'a pas été installé depuis le dépôt. Donner les deux commandes d'installation :
  ```
  /plugin marketplace add Mael-2605/video-maker
  /plugin install video-maker@video-maker-marketplace
  ```
- Programme Claude introuvable : demander de taper `/plugin`, d'ouvrir les marketplaces, de choisir `video-maker-marketplace` et d'y lancer la mise à jour, puis de fermer et rouvrir Claude.

Personnages, lieux, projets et préférences restent dans `bibliotheque/` : une mise à jour ne les modifie pas. Le dire seulement si l'utilisateur s'en inquiète.
