# Apprentissage

> Rôle : comment noter les décisions du client, et comment `preferences.md` se met à jour toute seule au fil des projets.

## Journal

`bibliotheque/journal.md`. Append-only : on ajoute une ligne, on n'en modifie ni n'en supprime jamais. Ne jamais le lire en entier — au plus les 20 dernières lignes, via une lecture de fin de fichier, au bilan.

## Format des lignes

`AAAA-MM-JJ | <type> | <détail> | projet=<slug>`

Types : `duel`, `hook`, `script-correction`, `video-avis`, `image-refus`, `element-cree`, `preference-promue`, `preference-retiree`.

Exemples :

- `2026-10-02 | duel | lieu | gagnant=gpt-image | perdant=nano-banana-pro | projet=2026-10-02-canape-jeff`
- `2026-10-02 | duel | vue-lieu | gagnant=nano-banana-pro | perdant=gpt-image | projet=2026-10-02-canape-jeff`
- `2026-10-02 | hook | nature=parle | famille=question-directe | rejetes=visuel:plein-geste,situation:mini-scene | projet=2026-10-02-canape-jeff` (détail de la ligne `hook`, dont le champ facultatif `replique=` : `references/hooks.md#Ligne de journal`)
- `2026-10-02 | script-correction | "plus court, moins formel" | projet=2026-10-02-canape-jeff`
- `2026-10-02 | element-cree | personnage=jeff | projet=2026-10-02-canape-jeff`
- `2026-10-02 | preference-promue | hooks:question-directe | projet=2026-10-02-canape-jeff`
- `2026-10-09 | preference-promue | hooks:nature-visuel | projet=2026-10-09-jeff-voiture`

## Préférences

`bibliotheque/preferences.md`, 40 lignes au plus, lu à chaque session. Chaque ligne porte un score (« 5 fois sur 6 »). Le client peut le modifier ou effacer une ligne librement — un fichier écrit à la main prime sur ce que le skill aurait déduit.

Une section fait exception : `## Sous-titres` (« Sous-titres : tiktok »). C'est un choix du client, pas une tendance : le skill l'écrit ou la remplace directement, sans score, et elle ne passe ni par `## Promotion` ni par `## Retrait`. Une ancienne section `## Voix` est laissée telle quelle et n'est plus lue.

## Promotion

Au bilan : lire les 20 dernières lignes du journal. Une tendance vue au moins 3 fois de façon concordante, et absente de `preferences.md`, donne une phrase au client (« Vous choisissez souvent les questions directes, je le retiens ? »). Si oui : écrire la ligne dans `preferences.md`, puis une ligne `preference-promue` dans le journal. Une seule proposition par bilan, même si plusieurs tendances qualifient.

Pour les hooks, une tendance est soit une famille (`famille=`, ou `replique=` quand une réplique de cette famille suit un hook visuel ou de situation, ex. « question directe : 3 fois sur 3 », promue en `hooks:question-directe`), soit une nature (`nature=`, 3 fois la même, ex. « hooks visuels : 3 fois sur 4 », promue en `hooks:nature-visuel`). Si les deux qualifient, proposer la famille, plus précise. Une ancienne ligne `hook` sans `nature=` compte seulement pour sa famille. Ce que change une préférence de hook : `references/hooks.md#Tenir compte des préférences`.

## Retrait

Une préférence contredite deux fois de suite (le client s'en écarte deux fois d'affilée ; pour un hook, il choisit une proposition qui ne la suit pas, ni par `famille=`, ni par `replique=`, ni par `nature=`) est retirée de `preferences.md` sans demander, avec une ligne `preference-retiree` dans le journal, et une phrase le mentionnant au bilan suivant.
