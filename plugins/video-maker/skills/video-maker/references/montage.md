# Montage

> Rôle : extraire la voix d'un personnage d'un cut gardé, puis assembler les cuts finaux en une seule vidéo. Fait par le tournage, sans rien demander au client. Prix : compris dans celui de la trame (`references/cout.md#Avant`).

## Principe

Chaque cut est généré à part. Deux choses les tiennent ensemble à la fin : la même voix pour un même personnage, et un montage qui ne fait pas sauter le son d'un cut à l'autre. Aucun autre traitement du son ni de l'image : ni musique, ni bruitage, ni étalonnage.

## Extraire une voix

Quand : dès qu'un cut où un personnage parle seul (personne d'autre ne parle dans ce cut) est gardé, si ce personnage parle encore dans un cut suivant et qu'il n'a pas encore d'extrait dans `## Voix` de `etat.md`. Un extrait par personnage. Il n'est remplacé que par une nouvelle consigne `extraire voix`, quand le client a demandé de corriger cette voix.

Source : le brouillon gardé (colonne `brouillon` de `## Cuts`), jamais sa finalisation. Le cut suivant peut ainsi partir tout de suite, sans attendre le 1080p.

1. `media_upload` pour un fichier `voix-<slug>.mp3` : adresse d'envoi et media id.
2. Un seul `sandbox_exec` :
   - télécharger le brouillon (adresse lue par `show_generation_by_ids`) en `cut.mp4` ;
   - couper la fenêtre de la réplique (début lu dans le plan du cut, durée `mots ÷ 3` secondes ; incertaine : le cut entier). `-ss` et `-t` se placent avant `-i` : le temps repart de 0, et les fondus de 0,05 s se placent à 0 et à `<durée>-0.05` :
     `ffmpeg -ss <début> -t <durée> -i cut.mp4 -vn -ac 1 -ar 44100 -af "highpass=f=80,afade=t=in:st=0:d=0.05,afade=t=out:st=<durée-0.05>:d=0.05" -c:a libmp3lame -b:a 192k voix.mp3`
   - `ffprobe` : 2 s au moins (sinon le cut entier) ;
   - envoi vers l'adresse (`curl -X PUT --upload-file voix.mp3 <adresse>`).
3. `media_confirm {type:"audio", media_ids}`.
4. Dans `etat.md`, `## Voix` : `<slug> = <media id>`. Ligne `voix <slug>` dans `cout.md`.

Dans le prompt d'un cut suivant où il parle : `audio_references` = ce media id, et `References:` porte `@Audio 1 is <Name>'s voice from an earlier cut: keep this voice, not its words.` (`references/script.md#Continuité entre les cuts`). L'extrait de voix tient le timbre, pas les mots.

Personnage qui ne parle jamais seul dans un cut : pas d'extrait ; deux ou trois mots de voix, identiques dans chaque cut où il parle (`references/fiche-personnage.md#Voix`).

Repli sur les mots de voix : `## Échec`.

## Assembler les cuts

Quand : tous les cuts de `## Cuts` sont `final`. Entrée : les job ids de la colonne `final`, dans l'ordre de la trame, et la durée prévue de chaque cut (colonne `durée`). Chaque cut a été généré à 4 s au moins : le montage le coupe à sa durée prévue.

1. `media_upload` pour `final.mp4` : adresse d'envoi et media id.
2. Un seul `sandbox_exec` qui fait tout :
   1. télécharger les cuts finaux dans l'ordre (`cut1.mp4`, `cut2.mp4`…) ;
   2. **même volume pour chaque cut** : mesurer `I=$(ffmpeg -i cutK.mp4 -vn -af ebur128 -f null - 2>&1 | grep -E '^\s+I:' | tail -1 | awk '{print $2}')`, gain `G = -16 - I` ramené entre -6 et +6 dB (un cut qui n'a que de l'ambiance ne doit pas être gonflé), calculé avec `awk` ou `python`, jamais avec l'arithmétique du shell (nombres décimaux), puis `ffmpeg -i cutK.mp4 -c:v copy -af "volume=${G}dB,alimiter=limit=0.79:level=disabled" -c:a aac -b:a 192k -ar 48000 normK.mp4` ;
   3. **coupe, raccord et fondu sonore**, en une commande : chaque image coupée à sa durée prévue et ramenée en 1080×1920 à 24 images par seconde (un cut de secours peut avoir une autre taille) ; le son de chaque cut, sauf le dernier, pris 0,2 s plus long que son image (sur la suite du cut s'il a été généré plus long, sinon complété de silence), puis enchaîné au suivant par un fondu de 0,2 s : l'image coupe net, l'ambiance ne saute pas, et le son dure exactement la somme des images. Exemple pour trois cuts prévus à 3, 4 et 4 s :
      ```
      ffmpeg -i norm1.mp4 -i norm2.mp4 -i norm3.mp4 -filter_complex \
      "[0:v]trim=0:3,setpts=PTS-STARTPTS,scale=1080:1920:force_original_aspect_ratio=increase,crop=1080:1920,fps=24,setsar=1[v0];\
      [1:v]trim=0:4,setpts=PTS-STARTPTS,scale=1080:1920:force_original_aspect_ratio=increase,crop=1080:1920,fps=24,setsar=1[v1];\
      [2:v]trim=0:4,setpts=PTS-STARTPTS,scale=1080:1920:force_original_aspect_ratio=increase,crop=1080:1920,fps=24,setsar=1[v2];\
      [v0][v1][v2]concat=n=3:v=1:a=0[v];\
      [0:a]atrim=0:3.2,asetpts=PTS-STARTPTS,apad=whole_dur=3.2[a0];\
      [1:a]atrim=0:4.2,asetpts=PTS-STARTPTS,apad=whole_dur=4.2[a1];\
      [2:a]atrim=0:4,asetpts=PTS-STARTPTS[a2];\
      [a0][a1]acrossfade=d=0.2:c1=tri:c2=tri[a01];[a01][a2]acrossfade=d=0.2:c1=tri:c2=tri[a]" \
      -map "[v]" -map "[a]" -c:v libx264 -crf 16 -preset slow -pix_fmt yuv420p -c:a aac -b:a 192k -ar 48000 final.mp4
      ```
      Si l'ffmpeg du bac à sable ne connaît pas `apad=whole_dur`, remplacer `atrim=0:3.2,asetpts=PTS-STARTPTS,apad=whole_dur=3.2` par `atrim=0:3.2,asetpts=PTS-STARTPTS,apad=pad_dur=0.2,atrim=0:3.2` (compléter de silence, puis recouper à la durée prévue + 0,2 s ; `apad=pad_dur` seul rallongerait le son) ;
   4. `ffprobe` : durée de `final.mp4` = somme des durées prévues, à 0,1 s près ; envoi vers l'adresse.
3. `media_confirm {type:"video", media_ids}`. Media id dans `montage (media id):` de `etat.md`, ligne `montage` dans `cout.md`.
4. Relecture (`references/anti-slop.md#Relecture avant de montrer`) : au plus deux défauts vus, chacun avec son cut et son instant dans la vidéo montée ; jamais un défaut inventé.

Les sous-titres se font ensuite sur ce media id (`references/sous-titres.md`).

## Échec

- Un seul `sandbox_exec` par extraction et par montage, jamais une seconde exécution sans nouveau prix annoncé par l'agent principal.
- Un extrait de voix en échec, refusé par le brouillon, ou absent de `## Voix` : pas d'`ARRÊT`. Le tournage tire le cut sans `audio_references` et sans la phrase `@Audio 1 …`, et rend `VOIX: aucune — <slug> sans extrait`. L'agent principal fait alors écrire par le réalisateur deux ou trois mots de voix (`references/fiche-personnage.md#Voix`) dans les cuts encore `à faire` où ce personnage parle. Aucun prix, aucune question au client.
- Montage en échec : retour `ARRÊT: échec` ; les cuts finaux restent intacts, rien n'est régénéré.
