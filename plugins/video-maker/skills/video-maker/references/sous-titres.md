# Sous-titres

> Rôle : décrire quand et comment ajouter les sous-titres, une fois la vidéo validée par le client.

## Quand

Uniquement après le « oui » du client sur la vidéo montée (moment 4). Jamais avant : pas de sous-titres sur un brouillon, pas d'anticipation pendant le choix vidéo.

## Style

Demandé pour chaque vidéo, en une question, une fois la vidéo gardée.

- `bibliotheque/preferences.md` a une ligne `Sous-titres : <style>` (section `## Sous-titres`) : « Même style que la dernière fois (<nom du style>) ? ». Oui → ce style. Non → la question complète ci-dessous.
- Sinon : « Quel style de sous-titres ? », avec l'aperçu en mots de chaque style :
  - TikTok : lettres grasses et rondes, comme sur TikTok ;
  - Impact : lettres hautes et serrées, très lisibles ;
  - Géométrique : lettres nettes et régulières, plus sobres ;
  - Papier : mots posés sur un petit bandeau clair, comme une étiquette.
- Après le choix : écrire `Sous-titres : <style>` sous `## Sous-titres` dans `preferences.md`, à la place de l'ancienne ligne. Pas de ligne au journal.

| Style | Valeur notée | Réglage |
|---|---|---|
| TikTok | `tiktok` | `bold --font-key tiktok` |
| Impact | `impact` | `bold --font-key anton` |
| Géométrique | `geometrique` | `bold --font-key montserrat` |
| Papier | `papier` | `paper` |

Variante naturelle UGC, ajoutée par défaut à tous les styles ci-dessus : `--no-caps --single-line --stroke-frac 0.045 --max-words 4`.

## Appel

Fait par le tournage, consigne `sous-titres`, avec le style choisi, toujours sur le media id du montage.

1. `get_workflow_instructions {workflow:"subtitles"}` (gratuit) — relire les étapes à chaque session, ne pas les figer de mémoire.
2. Suivre la route « Finished remote video » de ce workflow : la vidéo montée est déjà en ligne, pas de nouvel upload.
3. Texte d'auteur : la réplique de chaque cut, dans l'ordre de la trame (pas une transcription automatique) via `--script`, plus `--language fr`.
4. Tout l'appel tient dans un seul `sandbox_exec`.
5. `media_confirm` une seule fois, sur le résultat final.
6. La vidéo sans sous-titres reste le master : ne jamais l'écraser, les sous-titres produisent un fichier à part.

Coût : voir `references/cout.md#Avant` — annoncé sans demander, estimation inconnue si `get_cost` n'existe pas pour ce workflow.

## Échec

Si le sous-titrage échoue (job en erreur, timeout, résultat vide) : livrer la vidéo sans sous-titres, avec une phrase de raison au client. Jamais de nouvelle génération vidéo pour rattraper un échec de sous-titrage — la vidéo elle-même reste valide.
