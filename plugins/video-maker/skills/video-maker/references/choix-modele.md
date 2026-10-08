# Choix du modèle

> Rôle : décider en interne quel modèle génère chaque image et chaque cut de la vidéo, et décrire les appels exacts du tournage. Le client ne choisit jamais le modèle.
> Règle d'argent : la trame a un prix global (un brouillon et une finalisation par cut) et un « oui » ; chaque nouvel essai d'un cut, son prix et un « oui ». Les images (vues du lieu comprises) sont annoncées avec leur prix, sans « oui » une à une.

## Vérification en début de session

Les ids ci-dessous étaient valides au 2026-09-24. Le catalogue Higgsfield change : on les revérifie à chaque session, avant le premier appel payant. Ne jamais figer un id de mémoire.

1. `models_explore {action:"get"}` sur chaque id retenu : `gpt_image_2_5`, `nano_banana_pro`, `seedance_2_5`, `kling3_0`.
2. Si un id a disparu : `models_explore {action:"recommend", query:"<besoin>"}` (ex. « photorealistic portrait », « talking video 9:16 with image references ») et prendre le premier modèle compatible avec les paramètres et rôles médias listés ici.
3. Si une recherche par nom (`search`) ne renvoie rien : `models_explore {action:"list", type:"image"}` (ou `type:"video"`) et lire la liste. Exemple connu : `search "nano banana pro"` est vide, l'id n'apparaît qu'avec `list`.
4. Relire au passage les durées et rôles médias du modèle : ils peuvent aussi changer.
5. Le prompt vidéo d'un cut est court (`references/script.md#Version prompt`) : aucune limite de longueur rencontrée à ce jour.

Ids et paramètres retenus :

| Usage | id | Paramètres | Rôles médias |
|---|---|---|---|
| Visages | `gpt_image_2_5` | `aspect_ratio`, `quality`, `resolution` 1k/2k/4k | `image_references` |
| Objets, lieux, images de base et vues du lieu | `nano_banana_pro` | `aspect_ratio`, `resolution` 1k/2k/4k (défaut 2k) | `image_references` |
| Vidéo parlée, un cut (défaut) | `seedance_2_5` | brouillon : `mode:"omni_reference"`, `draft: true` (480p), `duration` 4–30 s (4 s au minimum), `aspect_ratio:"9:16"`, `generate_audio:true` ; finalisation : `draft_job_id`, `resolution:"1080p"`, `bitrate_mode:"high"` (même prix que `standard`) | `image_references` (9 images au plus) ; `audio_references` : l'extrait de voix d'un personnage (`references/montage.md#Extraire une voix`) ; jamais `start_image`, `end_image` ni `video_references` |
| Secours d'un cut | `kling3_0` | `duration` **3–15 s seulement**, `mode` std/pro/4k, `sound` on/off, `aspect_ratio:"9:16"` | `start_image`, `end_image` seulement : aucune planche possible |

Aucun modèle audio : Seedance crée la voix avec l'image, à partir de la réplique du prompt. Seul audio en référence : l'extrait de voix tiré d'un cut gardé du même projet (`references/montage.md#Extraire une voix`). Relire avec `models_explore` combien d'`audio_references` un appel accepte et si le mode brouillon les prend. Si le brouillon refuse `audio_references`, ou si l'extrait manque (extraction en échec, ou jamais faite), le tournage tire le cut sans l'extrait (rien n'était parti : pas de nouvel achat) et le signale ; le personnage passe ensuite aux deux ou trois mots de voix dans les cuts encore `à faire` (`references/montage.md#Échec`) ; aucun prix en plus, aucune question au client.

Pour tous les appels : ne pas passer `use_unlim`. Si l'outil renvoie `unlim_choice`, poser la question au client telle quelle avant de continuer.

## Arbre de décision

On s'arrête à la première ligne qui correspond.

| Situation | Modèle | Langue |
|---|---|---|
| Personnage qui parle (défaut) | Seedance 2.5, un cut à la fois, brouillon puis finalisation (`## Brouillon et finalisation`) | Français natif |
| Scène sans dialogue, voix off seule | Seedance 2.5, même route | Français natif |
| Même action ratée 2 fois sur un cut de 15 s ou moins | Kling 3.0 pour ce cut seulement, texte seul (`## Route Kling`) | Français natif |
| Même action ratée 2 fois sur un cut de plus de 15 s | Pas de Kling ; proposer de couper ce cut en deux, ou de changer l'action | — |

Jamais de vidéo tournée en anglais avec une voix française posée après coup : elle sonne faux et garde un fort accent anglais (tests du 2026-10-03). Veo 3 n'est jamais utilisé pour un dialogue : son français n'est pas fiable.

Ce qui compte comme un échec Seedance : génération refusée par le modèle, ou vidéo que le client ou la relecture honnête rejette pour la même action dans un cut (main déformée, geste non rendu, visage qui change). Après le premier échec : reformuler le plan fautif, puis une seule relance. Au deuxième échec sur la même action : bascule selon le tableau.

Chaque relance et chaque bascule vers Kling est un nouvel achat : nouveau `get_cost:true`, nouveau prix affiché, nouveau « oui » du client avant de soumettre. Jamais de relance implicite.

Chaque cut est une génération à part. Refaire un cut après une correction ou un échec est permis, toujours avec un nouveau prix et un nouveau « oui ».

Avant chaque génération payante : `balance`. Si le solde est trop bas, s'arrêter et annoncer le montant qui manque.

Timeout ou réponse perdue : ne jamais soumettre à nouveau à l'aveugle. Reprendre le job existant avec son id (`jobs_wait`, puis `show_generation_by_ids`). Ne relancer que si le job est connu comme échoué.

## Modèles image

- Visages (planche du personnage, images de ses tenues) : `gpt_image_2_5`, la dernière version de GPT Image.
- Objets et lieux : `nano_banana_pro`.
- Image de base d'un lieu : `nano_banana_pro`, `"16:9"`.
- Vues du lieu (`references/vues-lieu.md`) : `nano_banana_pro`, avec l'image de base du lieu en référence.
- Si `preferences.md` indique un gagnant net pour un type d'image (au moins 3 duels gagnés sur 3 pour ce type), utiliser ce modèle directement pour ce type, sans duel, même après un refus.
- Toujours `aspect_ratio` explicite. Vues du lieu : `"9:16"`.
- Les images validées servent ensuite de référence : on passe leur job id (ou media id) en `value`, jamais une URL `https://`.

## Limite de références

Seedance prend au plus **9 images** par cut. Dans cet ordre : une planche par personnage visible dans le cut, une image par tenue portée dans le cut, les objets clés du cut (ou leur planche commune), l'image de base du lieu du cut, la vue de sa position de caméra (`references/vues-lieu.md#Dans un cut`). Les autres vues n'y vont pas. L'extrait de voix part en `audio_references` et ne compte pas dans les 9 images. Le concepteur compte à l'inventaire. Cas courant : planche, tenue, objet, image de base, vue = 5.

Plus de 9, dans cet ordre jusqu'à revenir à 9 :
1. **Tenue mineure en mots** : la tenue la plus simple d'un personnage secondaire n'a pas d'image (statut `en-mots` à l'inventaire) ; le paragraphe `References:` la décrit en quelques mots (`the bartender wears a plain black T-shirt and a brown canvas waist apron`).
2. **Planche commune d'objets** : une seule image `nano_banana_pro` qui montre chaque objet seul, côte à côte, chacun nommé dans le prompt (gabarit de `references/fiche-objet.md#Template du prompt`, un panneau par objet). Dans la vidéo : `@Image 6 is a sheet of the props: the ice axe on the left, the rope on the right; use their shapes and materials only.`
3. **Planche commune des personnages muets** (même principe, `gpt_image_2_5`).
4. Le concepteur simplifie l'histoire (moins de lieux, moins de personnages) et le dit dans la carte.

## Duel

Déclencheur : le client refuse une fois un rendu d'image (personnage, objet ou lieu). Sauf gagnant net dans `preferences.md` pour ce type (voir `## Modèles image`).

1. Corriger le prompt selon le retour du client, une seule modification.
2. Annoncer le prix des deux images ensemble (💳). `generate_image_batch` n'accepte pas `get_cost` : estimer chaque modèle avec `generate_image {get_cost:true}` avant.
3. `generate_image_batch` avec deux requêtes, le **même** prompt corrigé et les mêmes références :
   - index 0 : `{model:"nano_banana_pro", prompt, aspect_ratio, medias}`
   - index 1 : `{model:"gpt_image_2_5", prompt, aspect_ratio, medias}`
   L'ordre A/B est tiré au hasard à chaque duel, pour que le client ne s'habitue pas à une place.
4. `jobs_wait` sur les deux jobs jusqu'à l'état final.
5. Un seul `show_generation_by_ids` avec les deux jobs.
6. Relecture honnête des deux rendus avant de les montrer (voir `references/anti-slop.md#Relecture avant de montrer`).
7. Question au client : « A ou B ? ». Ne jamais nommer les modèles.
8. Noter la décision dans `journal.md`, une ligne, au format commun du journal, voir [references/apprentissage.md#Format des lignes](references/apprentissage.md#Format des lignes) :
   `AAAA-MM-JJ | duel | <type> | gagnant=<modèle> | perdant=<modèle> | projet=<slug>`
   `<type>` : `personnage` (tenues comprises), `objet`, `lieu` ou `vue-lieu`. `<modèle>` : `gpt-image` ou `nano-banana-pro`.

Si le client refuse A et B : nouvelle correction, nouveau duel.

## Échec technique d'une image

Échec : job `failed` ou refusé, résultat vide, ou image rejetée par la relecture de l'atelier (défauts listés dans la fiche du type). Délai dépassé ou réponse perdue n'est pas un échec : reprendre le job par son id (`jobs_wait`, `show_generation_by_ids`), jamais de nouvelle soumission à l'aveugle. L'atelier gère seul, sans question au client :
1. **1er échec** : une relance automatique, même requête, même prix, sans annonce. Job id écrit dans `etat.md` avant d'attendre.
2. **2e échec** : duel des deux modèles (`## Duel`, points 3 à 6), même prompt, sans question. L'atelier garde le rendu qui passe sa relecture ; les deux passent : celui du modèle par défaut du type. Pas de « A ou B ? », pas de ligne `duel` au journal.
3. **3e échec** (aucun rendu du duel ne passe) : l'élément est `bloque`. L'atelier renvoie une phrase pour le client : ce qui ne sort pas, en mots simples, et une solution : simplifier l'élément, ou le retirer de la trame. Un lieu ne se retire pas : simplifier, ou changer de lieu. Exemple : « Le chalet ne sort pas correctement. Je le simplifie (une pièce en bois, sans cheminée), ou on change de lieu ? »
4. Chaque relance reste sous le `Plafond` de la consigne : prix annoncé + 3 × une image au tarif du modèle d'image le plus cher (la relance muette et les deux images du duel). Plafond atteint : arrêt, retour `PLAFOND`. Solde trop bas avant une relance : arrêt aussi, retour `ARRÊT: solde` de l'atelier (comme le tournage), cas distinct du plafond.
Aucune vidéo tant qu'un élément de l'inventaire n'est pas prêt.

## Route Seedance

Route par défaut. Seedance génère la parole en français à partir des répliques écrites dans le prompt du cut, en même temps que le jeu et les gestes.

Le prix de la trame est la somme, pour chaque cut, de son brouillon (`get_cost:true` sur l'appel exact ci-dessous) et de sa finalisation (`## Brouillon et finalisation`). Les vues pas encore faites : la planche du personnage à leur place, même nombre de références ; l'extrait de voix pas encore fait : appel sans audio. Rien n'est soumis avant le « oui ».

**Le brouillon d'un cut** 💳

```
generate_video {
  model: "seedance_2_5",
  mode: "omni_reference",
  draft: true,
  duration: <durée générée du cut, 4 au minimum>,
  aspect_ratio: "9:16",
  generate_audio: true,
  medias: [
    {role: "image_references", value: <planche du personnage>},
    {role: "image_references", value: <image de sa tenue>},
    {role: "image_references", value: <planche de l'objet>},
    {role: "image_references", value: <image de base du lieu>},
    {role: "image_references", value: <vue du cut>},
    {role: "audio_references", value: <extrait de voix, seulement s'il y en a un>}
  ],
  prompt: <script.md, bloc du cut>
}
```

**La finalisation d'un cut gardé** 💳

```
generate_video {
  model: "seedance_2_5",
  draft_job_id: <job id du brouillon gardé>,
  resolution: "1080p",
  bitrate_mode: "high"
}
```

- **Ordre des médias**, toujours celui de `## Limite de références` : planche du personnage (celui qui parle le plus d'abord), tenues dans le même ordre, objets clés (ou leur planche commune), image de base du lieu (`bases du lieu (job ids):` de `etat.md`), vue du cut (`vues du lieu (job ids):`). Les `@Image N` du prompt suivent cet ordre, `@Audio 1` désigne le premier extrait de voix.
- **Toutes les images en `image_references`.** Jamais de `start_image` ni d'`end_image` : une image imposée pousse le modèle à recopier sa pose.
- **9 images au plus** : `## Limite de références`.
- `bitrate_mode: "high"` à la finalisation : un débit plus haut garde le grain et les pores ; même `get_cost` que `standard` (constaté le 2026-09-28, à relire avec le prix).
- Si le schéma de `models_explore` demande de répéter le prompt et les médias à la finalisation, les reprendre tels qu'ils sont partis au brouillon.
- Le rôle de chaque image tient dans le paragraphe `References:` du prompt, une courte proposition par média (`references/script.md#Version prompt`).

**Proposition de preset.** `generate_video` peut renvoyer une recommandation de preset (`preset_recommendation`, par exemple « IN THE DARK ») au lieu de soumettre : rien n'est parti. Le tournage la refuse sans rien demander au client : il renvoie le même appel, mot pour mot, avec `declined_preset_id` égal à l'id proposé. Jamais de bascule vers le preset, jamais de question au client. Même chose sur un appel `get_cost:true` et sur un appel de finalisation.

## Brouillon et finalisation

- **Durée** : Seedance génère 4 s au minimum. Un cut prévu à 2 ou 3 s est généré à 4 s (son prix aussi) et coupé à sa durée prévue au montage (`references/montage.md#Assembler les cuts`).
- **Brouillon** : 480p, ~3 crédits par seconde (15 crédits pour 5 s), relu avec `get_cost:true`. C'est lui que le client voit et garde.
- **Finalisation** : seulement pour un brouillon gardé, avec son `draft_job_id`, en 1080p, `bitrate_mode:"high"`. Le brouillon doit être finalisé dans les sept jours (date écrite avec son job dans la colonne `brouillon` de `## Cuts`) ; au-delà, il n'est plus repris : nouveau brouillon, nouveau prix, nouveau « oui ».
- **Prix d'une finalisation** : inconnu tant qu'aucun brouillon n'existe. À la trame, il est estimé par `get_cost:true` sur le même cut en 1080p direct (`resolution:"1080p"`, sans `draft`), prix majorant (~12 crédits par seconde) ; avant chaque finalisation, le tournage relit `get_cost:true` sur l'appel exact.
- **Un cut à la fois** : jamais de lot de cuts. Le cut suivant part dès que le précédent est gardé, pendant que celui-ci se finalise.
- **Échec d'un cut** : pas de relance automatique ; nouvel essai, nouveau prix, nouveau « oui ». Les cuts gardés ne sont jamais refaits sans demande du client.

## Route Kling

Secours d'un seul cut, de 15 s au plus, après deux échecs Seedance sur la même action dans ce cut. Sans brouillon ni finalisation : le rendu Kling est montré, et gardé tel quel s'il convient (il devient le cut final, recadré au montage). Au-delà de 15 s : `## Arbre de décision`.

Sur Higgsfield, `kling3_0` ne prend que `start_image` et `end_image` (vérifié avec `models_explore` le 2026-10-03) : aucune planche, aucune tenue, aucune vue. Le prompt est donc en texte seul, et le visage ne sera pas celui des planches. Le client le sait avant de dire « oui » (`## Phrase au client`).

Le prompt reprend le bloc du cut (`references/script.md#Version prompt`), répliques françaises comprises, avec un seul changement : le paragraphe `References:` est remplacé par une description en mots de chaque personnage (âge, cheveux, carrure, tenue, en une phrase chacun), de l'objet clé et du lieu, tirée des fiches. Les mentions `as in @Image N` sont retirées des plans.

**Le cut en secours** 💳 (~2 crédits par seconde en `std`, à relire avec `get_cost:true`)

```
generate_video {
  model: "kling3_0",
  mode: "std",
  duration: <durée générée du cut, 3 à 15>,
  aspect_ratio: "9:16",
  sound: "on",
  prompt: <script.md, bloc du cut, route Kling>
}
```

## Phrase au client

Une phrase, sans nom de modèle, sauf si le client le demande. Vouvoiement, sauf si le client tutoie.

- Route Seedance : « Je prends le modèle le plus fiable pour faire parler un personnage en français. »
- Bascule vers Kling : « Ce cut ne passe pas avec le premier modèle. J'en essaie un autre pour ce cut seulement, en français aussi, mais il ne prend pas les images : le visage ne sera pas exactement celui des fiches. »
- Cut de plus de 15 s après deux échecs : « Ce cut ne passe pas sur cette durée. Je peux le couper en deux, ou on change l'action. »
- Duel : « Je vous montre deux versions. A ou B ? »

Si le client demande le nom du modèle : le donner, en une phrase, sans comparaison ni jugement.
