---
name: video-promo
description: "Crée une vidéo verticale courte pour les réseaux (reel, TikTok, LinkedIn) avec un personnage IA hyper réaliste qui parle français, via Higgsfield. À utiliser dès que l'utilisateur veut une vidéo, un reel, un TikTok, une pub, ou parle d'un personnage (« Jeff ») à mettre en scène. Trois moments : vision, images et découpage, vidéo livrée."
---

# Vidéo promo

Une vidéo verticale pour les réseaux, sur le sujet que donne le client. Le skill n'apporte aucun savoir métier : le sujet vient toujours de la demande.
Le client n'intervient qu'à trois moments : la vision, les images avec le découpage, la vidéo livrée.
Tout le reste se fait en coulisse, par quatre sous-agents (`## Sous-agents`).
Ce fichier dit **quand** et **dans quel ordre**. Le **comment** est dans `references/` et dans les fichiers des sous-agents : on ne lit une référence qu'au moment où elle est citée.

## Principes

- Messages : `references/anti-slop.md#Ton du skill` (cinq lignes au plus, hors découpage et versions de la carte ; ni modèle, ni identifiant, ni prompt, ni chemin, ni paramètre, ni anglais, ni nom d'étape interne, ni récapitulatif). Une seule question à la fois.
- Le client n'intervient qu'aux trois moments. Rien du travail de coulisse ne s'affiche : ni retour de sous-agent, ni fichier, ni appel.
- Dire ce qu'on fait, puis ce qu'on attend. Pas de compliment.
- Vouvoiement par défaut. Si le client tutoie, passer au tutoiement pour la suite de la conversation. Aucune question sur le ton.
- Tu ou vous dans les accroches et la réplique : la forme de la demande quand elle en donne une (« t'as goûté le nouveau menu ? »), sinon celle que le client emploie avec le skill.
- Le client ne choisit jamais le modèle. Ne jamais nommer un modèle, sauf s'il le demande (alors une phrase, sans comparaison).
- Aucune mention « généré par IA » dans le flow : le client la coche lui-même à la publication.
- Conformité : chaque texte produit (accroche, réplique, découpage, texte fourni par le client) passe par `references/conformite.md` avant d'être montré ou mis dans un prompt. Les sous-agents l'appliquent ; l'agent principal ne montre rien qui ne l'ait pas passée.
- **Règle de slug** (une seule pour tout le skill et les fiches) : personnage = le **prénom seul** ; lieu ou objet = le **nom complet** ; normalisé : minuscules, accents retirés, tout caractère autre qu'une lettre ou un chiffre remplacé par un tiret. « Jeff le barman » → `jeff`, « Émilie » → `emilie`, « bar du Rhône » → `bar-du-rhone`. Recherche d'un élément existant (identique ou proche) : `references/fiche-personnage.md#Dans l'inventaire`.
- Une référence n'est chargée qu'au moment qui la cite. Ne pas tout lire d'avance.
- Outils Higgsfield appelés par leur nom court (`balance`, `show_generation_by_ids`…).
- Photos sur l'ordinateur du client (inspiration, son bureau, un objet) : appeler `media_upload_widget`, **seul outil de ce tour**, avant toute génération. Jamais de chemin local. On reprend après l'envoi, avec les media id.
- Argent : voir `## Règle d'argent`. Aucune vidéo sans prix affiché et « oui » explicite.
- Mémoire : `bibliotheque/preferences.md` est lu au démarrage. `bibliotheque/journal.md` reçoit une ligne par décision (format : `references/apprentissage.md#Format des lignes`) et n'est jamais lu en entier.
- État du projet : `etape:` = dernière étape terminée (0 à 7, ou `abandon`), voir `templates/projet-etat.md`. L'agent principal tient `etat.md`, sauf `## Jobs payants` et `cout.md`, écrits par le sous-agent qui paie. Deux exceptions où l'agent principal écrit dans `## Jobs payants` : le job id gardé après un duel « A ou B ? » (`## Moment 2`, point 5), et les lignes vidéo vidées avant une nouvelle vidéo (`## Moment 3`, point 3).
- Dossiers de la bibliothèque (`personnages/`, `lieux/`, `objets/`, `projets/`) : lister **les noms de dossiers seulement**. On n'ouvre une fiche qu'une fois l'élément choisi.

## Démarrage

À chaque nouvelle conversation, dans cet ordre.

**1. Connexion Higgsfield.** Appeler `balance`. Si l'outil n'existe pas ou renvoie une erreur de connexion, dire ceci et s'arrêter :

> Higgsfield n'est pas connecté. Pour le connecter :
> 1. Dans Claude Desktop, ouvrez Réglages, puis Connecteurs.
> 2. Cliquez sur « Ajouter un connecteur personnalisé ».
> 3. Nom : Higgsfield. URL : https://mcp.higgsfield.ai/mcp
> 4. Cliquez sur Connecter, puis connectez-vous à votre compte Higgsfield.
> Ensuite, ouvrez une nouvelle conversation et redemandez-moi la vidéo.

**2. Bibliothèque** (gratuit, aucune question). Si `bibliotheque/` n'existe pas :

- Créer `bibliotheque/` avec `preferences.md` et `journal.md`, copiés de `templates/preferences.md` et `templates/journal.md`, et les dossiers vides `personnages/`, `lieux/`, `objets/`, `projets/`.
- Donner le solde lu au point 1 en une phrase : « Votre compte Higgsfield a X crédits. »

Si `bibliotheque/` existe mais qu'il y manque `preferences.md`, `journal.md` ou l'un de ces dossiers : créer seulement ce qui manque, de la même façon.

**3. Préférences.** Lire `bibliotheque/preferences.md` (court, fait pour ça). S'il lui manque la section `## Sous-titres`, l'ajouter vide en tête du fichier.

**4. Reprise.** Lister les dossiers de `bibliotheque/projets/`. Dans chaque `etat.md`, lire seulement les lignes `etape:` et `demande:`. Ignorer `abandon` et `etape: 7`. Si un projet a `etape:` de 0 à 6 (le plus récent s'il y en a plusieurs), demander :

« On reprend la vidéo "<demande>" ou on en commence une nouvelle ? »

Le client refuse de reprendre : écrire `etape: abandon` dans ce `etat.md` ; ce projet ne sera plus proposé.

Reprise à l'étape `etape + 1`, en relisant seulement `etat.md`, `vision.md`, `script.md` et les fiches citées dans `## Choix validés` :

| `etape:` | On reprend à |
|---|---|
| 0 | `## Moment 1 — Vision` |
| 1 | `## Coulisse — Images` |
| 2 | `## Moment 2 — Images et découpage` |
| 3 | `## Coulisse — Tournage` (voir plus bas) |
| 4 | `## Moment 3 — Vidéo livrée` |
| 5 | `## Sous-titres` |
| 6 | `## Bilan` |

- Job id dans `## Jobs payants` dont le résultat n'a pas été montré : **d'abord vérifier ce job**, avec `jobs_wait` seulement (rien n'est montré au client hors des trois moments). Terminé → reprendre avec ce résultat, sans rien repayer : le sous-agent qui paie le reprend lui aussi au lieu de le soumettre à nouveau. En cours → attendre. Seulement s'il est connu comme échoué : nouveau prix, et nouveau « oui » s'il s'agit d'une vidéo (`## Règle d'argent`).
- `etape: 3` sans job vidéo : le « oui » d'une conversation précédente ne vaut plus. Redemander le réalisateur pour le prix et réafficher le moment 2 (découpage et ligne de prix).
- Hors de ces cas, un « oui » donné dans une conversation précédente ne vaut plus : toute dépense repart d'un nouveau prix et, pour une vidéo, d'un nouveau « oui ».

**5. Nouveau projet.**
- Slug : `AAAA-MM-JJ-<3 mots>` (date du jour, trois mots de la demande, normalisés selon la règle de slug). Exemple : `2026-10-02-jeff-bar-menu`.
- Créer `bibliotheque/projets/<slug>/etat.md` depuis `templates/projet-etat.md` (`etape: 0`, `demande:` = la demande du client mot pour mot) et `cout.md` depuis `templates/projet-cout.md`. `vision.md` : rien à créer, le concepteur l'écrit.
- Lire `references/cout.md` et suivre `## Avant` : solde de départ dans `cout.md`.

## Sous-agents

| Sous-agent | `subagent_type` | Quand | Coût |
|---|---|---|---|
| concepteur | `video-promo:concepteur` | Moment 1 : carte, correction de carte, autres pistes | gratuit |
| atelier-images | `video-promo:atelier-images` | Après le choix au moment 1 ; vues du lieu au moment 2 ; correction d'image, de vue ou élément nouveau | payant, prix déjà annoncé |
| realisateur | `video-promo:realisateur` | Images prêtes ; correction d'un plan ; correction après la vidéo | gratuit |
| tournage | `video-promo:tournage` | Après le « oui » du moment 2 ; sous-titres | payant, montant autorisé |

- **Appel** : outil Agent, `subagent_type` ci-dessus, toujours avec `run_in_background: false` (l'outil lance sinon le sous-agent en arrière-plan) : attendre le retour avant de répondre au client. La consigne commence par les trois lignes communes, puis les champs de la section `## Consigne reçue` du sous-agent, un par ligne (`Champ : valeur`) :
  ```
  Dossier de travail : <chemin absolu du dossier ouvert>
  Dossier du skill : <dossier de base de ce skill>
  Projet : bibliotheque/projets/<slug>/
  ```
- **Retour** : ne lire que les champs du format (`CARTE`, `INVENTAIRE`, `DÉCOUPAGE`, `PRIX`, `VUES DU LIEU`, `PHRASE`, `DÉFAUTS`, identifiants, dépense). Seuls le texte entre `CARTE` et `FIN CARTE`, celui entre `DÉCOUPAGE` et `FIN DÉCOUPAGE`, la `PHRASE` et les `DÉFAUTS` peuvent être montrés au client. Retour hors format : ne rien montrer, relire `vision.md` ou `script.md` et en tirer le texte.
- **Repli** : retour `HIGGSFIELD_INDISPONIBLE`, outil Agent absent ou sous-agent inconnu → faire soi-même le travail décrit dans `${CLAUDE_PLUGIN_ROOT}/agents/<nom>.md` (consigne, lectures, travail, interdits), appels Higgsfield compris, avec les mêmes règles d'argent et de message.

## Moment 1 — Vision

1. Photos locales dans la demande : `media_upload_widget`, seul outil du tour. On reprend avec les media id.
2. Concepteur : `Demande` (mot pour mot), `Durée` (celle du client, sinon 15), `Photos` (media ids, s'il y en a).
3. Écrire dans `etat.md` : `duree:` et `segments:` (lus sur `DUREE`), `vision:` (lu sur `VISION`). Afficher le texte de la carte tel quel : c'est le seul message de ce moment.
4. Le client change la carte : concepteur avec `Correction` (une seule modification ; plusieurs → l'une après l'autre) ; afficher la carte du retour telle quelle (lignes changées et nouveau prix). Autres pistes demandées : concepteur avec `Autres pistes: oui` ; afficher `Trois versions`, les paragraphes A à C et la question.
5. Le client choisit (A, B ou C). L'accroche choisie porte `(+1 image : …)` : ajouter l'élément à `## Inventaire` de `vision.md` (statut `nouveau`, relevé dans `## Accroches`) et son prix au prix des images ; il est déjà annoncé, pas de question.
6. Journal : ligne `hook` (`references/hooks.md#Ligne de journal`, codes de `## Accroches` de `vision.md`). `etat.md`, `## Choix validés`, tirés de `INVENTAIRE` et de l'élément d'accroche ajouté au point 5 (un lieu dans `lieu:`, un objet dans `objets:`) : `personnages:`, `tenues:`, `lieu:`, `objets:`, `planche commune:` ; `hook:` = lettre, nature, famille, ce qu'on voit, réplique. Puis `etape: 1`.

## Coulisse — Images

1. Relire `## Inventaire` de `vision.md` (jamais `NOUVEAU`, qui ignore l'image d'accroche et manque à la reprise). Aucune ligne `nouveau` ni `existant (element)` : `etape: 2`, passer au moment 2.
2. Une ligne au client : « Images en cours, environ deux minutes. » (elle couvre aussi les vues du lieu du moment 2).
3. Atelier : `Inventaire` (`vision.md`), `Photos`, `Prix annoncé` = `PRIX IMAGES` moins `PRIX VUES DU LIEU`, plus l'image d'accroche éventuelle (à la reprise : le prix de la ligne `Je fixe` de `## Carte` de `vision.md`, moins celui de `## Vues du lieu`, plus le `(+1 image …)` de l'accroche choisie), `Plafond` = prix annoncé + 3 × une image au tarif du modèle d'image le plus cher (prix unitaire lu avec `get_cost:true`, gratuit, pour chaque modèle de `references/choix-modele.md#Modèles image`, avec les paramètres de `references/fiche-personnage.md#Modèle et appel` et `references/fiche-lieu.md#Modèle et appel` ; le plus élevé des deux).
4. Retour :
   - `PRÊT` : `etape: 2`, puis moment 2.
   - `BLOQUÉ` : montrer la `PHRASE` seule, attendre la réponse. Simplifier l'élément → son prix en une ligne `💳 ~X crédits`, puis atelier avec `Correction` (cet élément), nouveau `Prix annoncé` et `Plafond`. Retirer l'élément, ou changer de lieu → concepteur avec `Correction` (`vision.md` mis à jour, carte non réaffichée), puis atelier si un élément nouveau apparaît, son prix en une ligne `💳 ~X crédits`.
   - `PLAFOND` : une phrase avec le nouveau prix : « Une image a demandé plus d'essais : 💳 ~X crédits de plus. » (X = crédits en plus du retour), puis atelier avec `Prix annoncé` = `Plafond` = X.
   - `ARRÊT: solde` : une phrase sur le solde : « Il manque X crédits pour les images. », puis `show_plans_and_credits`. Rien n'est lancé ; atelier à nouveau quand le client a rechargé.
5. Aucune vidéo tant qu'un élément de l'inventaire n'est pas prêt.

## Moment 2 — Images et découpage

1. Réalisateur : `Vision` (`vision.md`, avec `Accroche : <lettre>`), `Durée`, `Segments`, `Route` (`seedance`, sauf `modele: kling` dans `etat.md`).
2. **Vues du lieu** (`references/vues-lieu.md`) : les vues de `VUES DU LIEU` sans job dans `vues du lieu (job ids):` de `etat.md`. S'il y en a : la ligne d'attente si elle n'a pas été dite dans cette conversation (« Images en cours, environ une minute. »), puis atelier avec `Vues du lieu` (`SCRIPT` du retour), `Vues`, `Prix annoncé` (prix d'une vue × nombre de vues, lu dans `## Vues du lieu` de `vision.md`), `Plafond` (même règle qu'en `## Coulisse — Images`, point 3). Retour comme en `## Coulisse — Images`, point 4 ; `BLOQUÉ` sur une vue : montrer la `PHRASE`, puis réalisateur avec `Correction` (la vue simplifiée) et atelier avec `Refaire: oui` pour cette vue.
3. Un seul `show_generation_by_ids` : les vues du lieu (`vues du lieu (job ids):`, vue 1 d'abord), puis toutes les images de l'inventaire, existantes comprises : planches, tenues, lieux, objets, planche commune (job ids de `images (job ids):`, et ceux des fiches pour les éléments existants).
4. Message, sous les images : la `PHRASE` du réalisateur s'il y en a une, le découpage tel que renvoyé (une phrase par plan), puis `On garde ? Si oui, je lance la vidéo : 💳 ~<PRIX> crédits.`
5. Changement demandé : une seule modification à la fois ; plusieurs → l'une après l'autre. Une vue n'est refaite ou ajoutée que si la correction touche ce qu'elle fixe (`references/vues-lieu.md#Corrections`) ; elle compte alors dans la ligne de prix des images, et l'atelier la fait avec `Vues du lieu`, `Vues`, `Refaire: oui`.
   - **Image de l'inventaire** : ligne `image-refus` au journal. Prix en une ligne `💳 ~X crédits` : au premier refus de cet élément, les deux images du duel (`references/choix-modele.md#Duel`, point 2) ; sinon, ou avec un gagnant net dans `preferences.md`, une image ; pour un lieu, plus ses vues (`## Vues du lieu` de `script.md`). Atelier avec `Correction`, `Duel: oui` au premier refus, `Prix annoncé`, `Plafond`. Duel : un seul `show_generation_by_ids` avec les deux rendus, « A ou B ? ». Au choix : l'agent principal met à jour la fiche (`## Enregistrement` du type, ligne `## Historique`), l'entrée de l'élément dans `images (job ids):` de `etat.md`, le statut `pret` dans `## Inventaire` de `vision.md`, et écrit la ligne `duel` au journal (`references/choix-modele.md#Duel`, point 8). L'atelier ne range rien après un duel. Pour un lieu, puis ses vues.
   - **Vue seule** (la vue ne change pas, le rendu déplaît) : ligne `image-refus` au journal ; même règle de duel, type `vue-lieu` ; atelier avec `Vues du lieu`, `Vues` (cette vue), `Duel: oui`. Au choix : l'agent principal remplace l'entrée de la vue dans `vues du lieu (job ids):` et écrit la ligne `duel`.
   - **Plan** (action, position, cadrage, réplique) : ligne `script-correction` au journal ; réalisateur avec `Correction` ; aucune image refaite (`VUES DU LIEU: aucune`), pas de ligne de prix d'image. Si le plan demande une position de caméra qu'aucune vue ne couvre, ou si la lumière change : `VUES DU LIEU` les nomme, leur prix en une ligne, puis atelier.
   - **Plan qui demande un élément nouveau** (« plutôt dans un parc ») : ligne `script-correction` au journal ; ajouter l'élément à `## Inventaire` de `vision.md` (statut `nouveau`), prix de l'image et de ses vues (un lieu) en une ligne, atelier avec `Correction` pour ce seul élément, puis réalisateur avec `Correction`, puis atelier pour les vues. Le nouveau prix de la vidéo ne s'affiche qu'une fois les images prêtes.
   - Réafficher seulement ce qui a changé (images, vues, lignes du découpage), puis la ligne de prix avec le nouveau prix.
   - Durée portée au-delà de 30 s : concepteur avec `Correction`, sa carte annonce les parties et le nouveau prix (afficher ses lignes changées ; des vues de plus seulement si un lieu s'ajoute), puis réalisateur avec les nouveaux `Durée` et `Segments`, puis atelier pour les vues nouvelles.
6. « Oui » explicite (`## Règle d'argent`) : `etat.md` : `script:` (lu sur `SCRIPT`), `modele:` (lu sur `ROUTE`), `prix video:`, `montant autorise:` = ce prix, `etape: 3`.

## Coulisse — Tournage

1. Blocage : relire `## Inventaire` de `vision.md`. Chaque ligne doit être `pret`, `existant` ou `en-mots` (tenue mineure décrite en mots), et chaque vue de `## Vues du lieu` de `script.md` avoir son job dans `vues du lieu (job ids):`. Sinon, aucune vidéo : `## Coulisse — Images`, ou `## Moment 2 — Images et découpage`, point 2, pour ce qui manque.
2. `balance`. Solde sous le prix : « Il manque X crédits. », puis `show_plans_and_credits`. Rien n'est lancé.
3. Une ligne : « Vidéo en cours, environ N minutes. » (N = 5 par segment).
4. Tournage : `Mode: video`, `Script` (`script:` de `etat.md`), `Route` (`modele:`), `Montant autorisé` (`montant autorise:`).
5. Retour :
   - `VIDÉO` : `etape: 4`, puis moment 3.
   - `ARRÊT: solde` : point 2.
   - `ARRÊT: dépassement` : nouveau prix (celui de la phrase du tournage ; à défaut, redemander le réalisateur), ligne `💳 ~X crédits`, nouveau « oui », `prix video:` et `montant autorise:` mis à jour, puis tournage (les jobs déjà soumis sont repris, jamais repayés).
   - `ARRÊT: échec` : `## Erreurs`. Un seul segment échoué : une phrase, son prix en ligne `💳 ~X crédits` (lu dans la phrase du tournage), nouveau « oui », `montant autorise:` = ce prix, puis tournage, qui ne refait que ce segment.

## Moment 3 — Vidéo livrée

1. Afficher la vidéo avec l'outil nommé par `AFFICHAGE`, sur l'identifiant de `VIDÉO`.
2. Message : les défauts renvoyés, un par ligne avec leur instant, ou « Rien à signaler de mon côté. » ; puis « On garde, ou je corrige quelque chose ? ». Journal : ligne `video-avis` à la réponse.
3. Correction : une seule modification ; ligne `script-correction` au journal ; réalisateur avec `Correction` ; `VUES DU LIEU` n'est pas `aucune` (lieu, lumière ou position nouvelle) : leur prix en une ligne, puis atelier avec `Vues du lieu`, `Vues`, `Refaire: oui` ; afficher les vues refaites s'il y en a, les lignes changées et `On garde ? Si oui, je relance la vidéo : 💳 ~<PRIX> crédits.` ; nouveau « oui » ; `etat.md` : `script:`, `prix video:`, `montant autorise:`, `etape: 3`, lignes vidéo de `## Jobs payants` vidées (`video`, `montage` ; images et vues gardées) ; puis `## Coulisse — Tournage`.
4. Voix qui ne va pas (« trop jeune », « trop aiguë ») : réalisateur avec `Correction`, deux ou trois mots de voix dans la présentation du personnage (`references/fiche-personnage.md#Voix`). Visage qui change d'un plan à l'autre : réalisateur avec `Correction` (le plan qui dérive nomme le personnage et sa planche). Même action ratée deux fois : `references/choix-modele.md#Arbre de décision` : 15 s ou moins, la phrase de bascule de `references/choix-modele.md#Phrase au client` (les visages ne seront pas ceux des fiches), puis réalisateur avec `Route: kling` ; au-delà de 15 s, la phrase qui propose de raccourcir. Chaque fois, suite du point 3, avec `modele:` mis à jour.
5. Gardée : `etape: 5`.

## Sous-titres

1. Une question de style (`references/sous-titres.md#Style`) : dernier choix de `preferences.md` proposé s'il existe. Style noté dans `preferences.md`, `## Sous-titres`, à la place de l'ancien.
2. Coût en une ligne, sans « oui » : `💳 ~X crédits` (sans estimation : « estimation inconnue, faible », `references/cout.md#Avant`).
3. Tournage : `Mode: sous-titres`, `Style`, `Montant autorisé` = le prix annoncé des sous-titres.
4. Retour `SOUS-TITRES` : `etape: 6`. `ARRÊT: échec` : une phrase, la vidéo est livrée sans sous-titres, `etape: 6`. `ARRÊT: solde` : « Il manque X crédits. », `show_plans_and_credits`.

## Bilan

1. Afficher la vidéo finale : sous-titrée (`show_medias` sur `video sous-titrée (media id):`), sinon la vidéo gardée.
2. La ligne de coût (`references/cout.md#Bilan`), une seule ligne.
3. Apprentissage : `references/apprentissage.md`, `## Promotion` (au plus une proposition au client), puis `## Retrait`.
4. `etat.md` : `etape: 7`. Le projet est fermé et ne sera plus proposé en reprise.

## Règle d'argent

Détail : `references/cout.md`.

- **Images et vues du lieu** : prix dans la ligne `Je fixe` de la carte, sans question ; une image ou une vue refaite après une correction : prix en une ligne, sans question. **Sous-titres** : prix en une ligne, sans question.
- **Vidéo** : prix affiché au moment 2, puis « oui » explicite. Ce « oui » couvre la vidéo entière, segments et montage compris. Le tournage reçoit ce montant et ne dépense rien au-delà.
- L'atelier reçoit un `Plafond` = prix annoncé + 3 × une image au tarif du modèle d'image le plus cher (relance muette et les deux rendus du duel d'un échec technique). Au-delà : une phrase avec le nouveau prix avant de relancer.
- Ne valent pas accord de dépense : le silence, « ok pour le découpage », « ça me va », un « oui » à une autre question, un « oui » d'une conversation précédente.
- `get_cost:true` avant chaque vidéo, et avant chaque image quand l'outil l'accepte.
- `balance` avant chaque génération payante. Solde insuffisant : s'arrêter et annoncer le montant qui manque.
- Relance payante ou bascule vers un autre modèle, pour une vidéo : nouveau `get_cost`, nouveau prix, nouveau « oui ». Jamais l'ancien prix ni l'ancien accord. Pour une image ou les sous-titres : nouveau prix annoncé en une ligne, sans « oui » (relance muette d'un échec technique : sous le plafond, sans annonce).
- Ne jamais passer `use_unlim`. Si un outil renvoie `unlim_choice`, poser la question au client telle quelle.

## Erreurs

| Cas | Réaction |
|---|---|
| Higgsfield non connecté | Message de `## Démarrage` point 1. Stop. |
| Sous-agent sans Higgsfield (`HIGGSFIELD_INDISPONIBLE`), outil Agent absent | Repli de `## Sous-agents` : l'agent principal fait les appels lui-même, mêmes règles de message. |
| Solde insuffisant | « Il manque X crédits. », `show_plans_and_credits`. Rien n'est lancé. |
| Contenu refusé pour conformité | `references/conformite.md#Reformulations types`, une phrase ; relance = nouvel achat (vidéo : prix + « oui »). |
| Image en échec technique | Géré par l'atelier (`references/choix-modele.md#Échec technique d'une image`) ; au 3e échec, sa `PHRASE`. |
| Image refusée par le client | Ligne `image-refus`, duel (`references/choix-modele.md#Duel`). |
| Vidéo ratée, 1re fois | Une phrase au client ; réalisateur avec `Correction` : reformuler le plan fautif ; nouveau prix, nouveau « oui ». |
| Même action ratée une 2e fois | `references/choix-modele.md#Arbre de décision` ; nouveau prix, nouveau « oui ». |
| Dépassement du montant autorisé | Nouveau prix, nouveau « oui ». |
| Timeout, réponse perdue, conversation coupée | Reprendre le job par son id (`## Jobs payants`), jamais de nouvelle soumission à l'aveugle. |
| Sous-titres en échec | Vidéo livrée sans sous-titres, une phrase. |
| Plus de 30 s demandées | Film en segments, annoncé dans la carte (`references/script.md#Segments`). |
