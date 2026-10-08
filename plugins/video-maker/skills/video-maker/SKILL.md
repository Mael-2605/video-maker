---
name: video-maker
description: "Crée une vidéo verticale courte pour les réseaux (reel, TikTok, LinkedIn) avec un personnage IA hyper réaliste qui parle français, via Higgsfield. À utiliser dès que l'utilisateur veut une vidéo, un reel, un TikTok, une pub, ou parle d'un personnage (« Jeff ») à mettre en scène. Quatre moments : carte vision, trame, cuts un par un, vidéo montée."
---

# Vidéo promo

Une vidéo verticale pour les réseaux, sur le sujet que donne le client. Le skill n'apporte aucun savoir métier : le sujet vient toujours de la demande.
Le client intervient à quatre moments : la carte vision, la trame, chaque cut, la vidéo montée.
Tout le reste se fait en coulisse, par quatre sous-agents (`## Sous-agents`).
Ce fichier dit **quand** et **dans quel ordre**. Le **comment** est dans `references/` et dans les fichiers des sous-agents : on ne lit une référence qu'au moment où elle est citée.

## Principes

- Messages : `references/anti-slop.md#Ton du skill` (cinq lignes au plus, hors trame et versions de la carte ; ni modèle, ni identifiant, ni prompt, ni chemin, ni paramètre, ni anglais, ni nom d'étape interne, ni récapitulatif). Une seule question à la fois.
- Le client n'intervient qu'aux quatre moments. Rien du travail de coulisse ne s'affiche : ni retour de sous-agent, ni fichier, ni appel.
- Dire ce qu'on fait, puis ce qu'on attend. Pas de compliment.
- Vouvoiement par défaut. Si le client tutoie, passer au tutoiement pour la suite de la conversation. Aucune question sur le ton.
- Tu ou vous dans les accroches et la réplique : la forme de la demande quand elle en donne une (« t'as goûté le nouveau menu ? »), sinon celle que le client emploie avec le skill.
- Le client ne choisit jamais le modèle. Ne jamais nommer un modèle, sauf s'il le demande (alors une phrase, sans comparaison).
- Aucune mention « généré par IA » dans le flow : le client la coche lui-même à la publication.
- Conformité : chaque texte produit (accroche, réplique, trame, texte fourni par le client) passe par `references/conformite.md` avant d'être montré ou mis dans un prompt. Les sous-agents l'appliquent ; l'agent principal ne montre rien qui ne l'ait pas passée.
- **Règle de slug** (une seule pour tout le skill et les fiches) : personnage = le **prénom seul** ; lieu ou objet = le **nom complet** ; normalisé : minuscules, accents retirés, tout caractère autre qu'une lettre ou un chiffre remplacé par un tiret. « Jeff le barman » → `jeff`, « Émilie » → `emilie`, « bar du Rhône » → `bar-du-rhone`. Recherche d'un élément existant (identique ou proche) : `references/fiche-personnage.md#Dans l'inventaire`.
- Une référence n'est chargée qu'au moment qui la cite. Ne pas tout lire d'avance.
- Outils Higgsfield appelés par leur nom court (`balance`, `show_generation_by_ids`…).
- Photos sur l'ordinateur du client (inspiration, son bureau, un objet) : appeler `media_upload_widget`, **seul outil de ce tour**, avant toute génération. Jamais de chemin local. On reprend après l'envoi, avec les media id.
- Argent : voir `## Règle d'argent`. Aucun cut sans prix affiché et « oui » explicite (celui de la trame vaut pour tous ses cuts).
- Mémoire : `bibliotheque/preferences.md` est lu au démarrage. `bibliotheque/journal.md` reçoit une ligne par décision (format : `references/apprentissage.md#Format des lignes`) et n'est jamais lu en entier.
- État du projet : `etape:` = dernière étape terminée (0 à 7, ou `abandon`), voir `templates/projet-etat.md`. L'agent principal tient `etat.md`, sauf `## Jobs payants`, `## Voix`, les colonnes `brouillon` et `final` de `## Cuts` (statuts `brouillon` et `final` compris) et `cout.md`, écrits par le sous-agent qui paie. L'agent principal écrit les lignes de `## Cuts` et le statut `gardé`. Exceptions : le job id gardé après un duel « A ou B ? » (`## Moment 2 — Trame`) ; la colonne `final` vidée avant un nouvel essai d'un cut gardé ou final (`## Moment 3 — Boucle par cut`, `## Moment 4 — Vidéo montée`), avec `montage (media id):` s'il est rempli ; les colonnes `brouillon` et `final` vidées d'un cut dont le brouillon a plus de sept jours (`## Démarrage`, point 4 ; `## Moment 3 — Boucle par cut`, point 8) ; `montage (media id): échec` après un montage en échec, vidé juste avant le nouvel appel `monter` (`## Coulisse — Montage`). Une voix de `## Voix` n'est remplacée que par une nouvelle consigne `extraire voix`.
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
| 0 | `## Moment 1 — Carte vision` |
| 1 | `## Coulisse — Images` |
| 2 | `## Moment 2 — Trame` |
| 3 | `## Moment 3 — Boucle par cut`, au premier cut qui n'est pas `final` ni `gardé` (`à faire` ou `brouillon`) |
| 4 | `## Moment 4 — Vidéo montée` |
| 5 | `## Sous-titres` |
| 6 | `## Bilan` |

- Jobs de `## Jobs payants` ou de `## Cuts` dont le résultat n'a pas été montré : **d'abord les vérifier**, avec `jobs_wait` seulement. Terminé → reprendre avec ce résultat, sans rien repayer ; le tournage les reprend lui aussi au lieu de les soumettre à nouveau. En cours → attendre. Connu comme échoué → nouveau prix, nouveau « oui ».
- `etape: 3` : d'abord ce qui est repris sans payer. Âge d'un brouillon : la date écrite avec son job dans la colonne `brouillon` (`v<k>=<job id> (<AAAA-MM-JJ>)`). Un cut `brouillon` dont le job est terminé et a sept jours au plus est montré tel quel (`## Moment 3 — Boucle par cut`, point 2). Un cut `gardé` dont la finalisation a un job : repris par le tournage, sans prix. Aucun cut `à faire` ni `brouillon` (tous `gardé` ou `final`, une finalisation peut-être en cours), aucun cut `gardé` sans finalisation dont le brouillon a plus de sept jours, et `montage (media id):` vide : `## Coulisse — Montage` directement, sans question (le tournage attend les finalisations en cours ; le montage est compris dans le prix de la trame). `montage (media id): échec` : `## Coulisse — Montage`, point 3 (prix et « oui » avant tout nouvel appel).
- Ensuite seulement, et avant tout appel payant (celui du point 3 du moment 3 après un « on garde » compris), s'il reste quelque chose à payer (finalisations sans job, cuts `à faire`, un nouveau brouillon pour chaque cut `brouillon` ou `gardé` sans finalisation dont le brouillon a plus de sept jours, sa finalisation comptant déjà parmi les finalisations sans job) : prix lu dans les lignes `Prix :` de `script.md` (réalisateur sans `Correction` pour un `get_cost` à jour), puis `Si oui, je continue : 💳 ~<X> crédits pour la suite.` « Oui » : `montant autorise:` augmenté de X ; chaque cut au brouillon de plus de sept jours : colonnes `brouillon` et `final` vidées, statut `à faire` (le tournage ne reprend jamais ce brouillon). Rien n'est payé sur l'accord de la conversation précédente : son « oui » ne vaut plus. Rien à payer : pas de ligne de prix.
- `etape: 2` avec un `script.md` : réalisateur sans `Correction`, puis réafficher le moment 2.

**5. Nouveau projet.**
- Slug : `AAAA-MM-JJ-<3 mots>` (date du jour, trois mots de la demande, normalisés selon la règle de slug). Exemple : `2026-10-02-jeff-bar-menu`.
- Créer `bibliotheque/projets/<slug>/etat.md` depuis `templates/projet-etat.md` (`etape: 0`, `demande:` = la demande du client mot pour mot) et `cout.md` depuis `templates/projet-cout.md`. `vision.md` : rien à créer, le concepteur l'écrit.
- Lire `references/cout.md` et suivre `## Avant` : solde de départ dans `cout.md`.

## Sous-agents

| Sous-agent | `subagent_type` | Quand | Coût |
|---|---|---|---|
| concepteur | `video-maker:concepteur` | Moment 1 : carte, correction de carte, autres pistes | gratuit |
| atelier-images | `video-maker:atelier-images` | Après le choix au moment 1 ; vues du lieu au moment 2 ; image corrigée ou nouvelle | payant, prix déjà annoncé |
| realisateur | `video-maker:realisateur` | Images prêtes ; correction de la trame ou d'un cut | gratuit |
| tournage | `video-maker:tournage` | Après chaque « oui » sur un cut ou la trame ; « on garde » ; montage ; sous-titres | payant, montant autorisé |

- **Appel** : outil Agent, `subagent_type` ci-dessus, toujours avec `run_in_background: false` (l'outil lance sinon le sous-agent en arrière-plan) : attendre le retour avant de répondre au client. La consigne commence par les trois lignes communes, puis les champs de la section `## Consigne reçue` du sous-agent, un par ligne (`Champ : valeur`) :
  ```
  Dossier de travail : <chemin absolu du dossier ouvert>
  Dossier du skill : <dossier de base de ce skill>
  Projet : bibliotheque/projets/<slug>/
  ```
- **Retour** : ne lire que les champs du format (concepteur : `CARTE`, `VISION`, `INVENTAIRE`, `PRIX IMAGES`, `PRIX VUES DU LIEU`, `DUREE` ; réalisateur : `TRAME` / `FIN TRAME`, `PRIX`, `CUTS`, `SCRIPT`, `ROUTE`, `VUES DU LIEU`, `PHRASE` ; tournage : `CUT`, `DÉFAUTS`, `FINAL`, `VOIX`, `MONTAGE`, `SOUS-TITRES`, `ARRÊT` ; identifiants, dépense). Seuls le texte entre `CARTE` et `FIN CARTE`, celui entre `TRAME` et `FIN TRAME`, la `PHRASE` et les `DÉFAUTS` peuvent être montrés au client. Retour hors format : ne rien montrer, relire `vision.md` ou `script.md` et en tirer le texte.
- **Repli** : retour `HIGGSFIELD_INDISPONIBLE`, outil Agent absent ou sous-agent inconnu → faire soi-même le travail décrit dans `${CLAUDE_PLUGIN_ROOT}/agents/<nom>.md` (consigne, lectures, travail, interdits), appels Higgsfield compris, avec les mêmes règles d'argent et de message.

## Moment 1 — Carte vision

1. Photos locales dans la demande : `media_upload_widget`, seul outil du tour. On reprend avec les media id.
2. Concepteur : `Demande` (mot pour mot), `Durée` (celle du client, sinon 15), `Photos` (media ids, s'il y en a).
3. Écrire dans `etat.md` : `duree:` (lu sur `DUREE`), `vision:` (lu sur `VISION`). Afficher le texte de la carte tel quel : c'est le seul message de ce moment.
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

## Moment 2 — Trame

1. Réalisateur : `Vision` (`vision.md`, avec `Accroche : <lettre>`), `Durée`, `Route` (`seedance`, sauf `modele: kling` dans `etat.md`).
2. **Vues du lieu** (`references/vues-lieu.md`) : les vues de `VUES DU LIEU` sans job dans `vues du lieu (job ids):` de `etat.md`. S'il y en a : la ligne d'attente si elle n'a pas été dite dans cette conversation (« Images en cours, environ une minute. »), puis atelier avec `Vues du lieu` (`SCRIPT` du retour), `Vues`, `Prix annoncé` (prix d'une vue × nombre de vues, lu dans `## Vues du lieu` de `vision.md`), `Plafond` (même règle qu'en `## Coulisse — Images`, point 3). Retour comme en `## Coulisse — Images`, point 4 ; `BLOQUÉ` sur une vue : montrer la `PHRASE`, puis réalisateur avec `Correction : trame : <la vue simplifiée>` et atelier avec `Refaire: oui` pour cette vue.
3. Un seul `show_generation_by_ids`, dans cet ordre : l'image de base de chaque lieu (`bases du lieu (job ids):`, sinon `lieu:<slug>` de `images (job ids):` ou la fiche), les vues (`vues du lieu (job ids):`, vue 1 d'abord), puis planches, tenues, objets, planche commune (existants compris).
4. Message, sous les images : la `PHRASE` s'il y en a une, la trame telle que renvoyée, puis `On garde la trame ? Si oui, je lance le cut 1 : 💳 ~<X> crédits pour l'ensemble.` (X = `PRIX` du réalisateur). Ce « oui » vaut pour tous les cuts de la trame : un brouillon et une finalisation chacun.
5. Changement demandé : une seule modification à la fois ; plusieurs → l'une après l'autre. Une vue n'est refaite ou ajoutée que si la correction touche ce qu'elle fixe (`references/vues-lieu.md#Corrections`) ; elle compte alors dans la ligne de prix des images, et l'atelier la fait avec `Vues du lieu`, `Vues`, `Refaire: oui`.
   - **Image de l'inventaire** : ligne `image-refus` au journal. Prix en une ligne `💳 ~X crédits` : au premier refus de cet élément, les deux images du duel (`references/choix-modele.md#Duel`, point 2) ; sinon, ou avec un gagnant net dans `preferences.md`, une image ; pour un lieu, plus ses vues (`## Vues du lieu` de `script.md`). Atelier avec `Correction`, `Duel: oui` au premier refus, `Prix annoncé`, `Plafond`. Duel : un seul `show_generation_by_ids` avec les deux rendus, « A ou B ? ». Au choix : l'agent principal met à jour la fiche (`## Enregistrement` du type, ligne `## Historique`), l'entrée de l'élément dans `images (job ids):` de `etat.md` (pour un lieu, aussi `bases du lieu (job ids):`), le statut `pret` dans `## Inventaire` de `vision.md`, et écrit la ligne `duel` au journal (`references/choix-modele.md#Duel`, point 8). L'atelier ne range rien après un duel. Pour un lieu, puis ses vues.
   - **Vue seule** (la vue ne change pas, le rendu déplaît) : ligne `image-refus` au journal ; même règle de duel, type `vue-lieu` ; atelier avec `Vues du lieu`, `Vues` (cette vue), `Duel: oui`. Au choix : l'agent principal remplace l'entrée de la vue dans `vues du lieu (job ids):` et écrit la ligne `duel`.
   - **Ligne de trame** (action, position, cadrage, réplique) : ligne `script-correction` au journal ; réalisateur avec `Correction : trame : <la modification>` ; aucune image refaite (`VUES DU LIEU: aucune`), pas de ligne de prix d'image. Si la ligne demande une position de caméra qu'aucune vue ne couvre, ou si la lumière change : `VUES DU LIEU` les nomme, leur prix en une ligne, puis atelier.
   - **Ligne de trame qui demande un élément nouveau** (« plutôt dans un parc ») : ligne `script-correction` au journal ; ajouter l'élément à `## Inventaire` de `vision.md` (statut `nouveau`), prix de l'image et de ses vues (un lieu) en une ligne, atelier avec `Correction` pour ce seul élément, puis réalisateur avec `Correction : trame : <la modification>`, puis atelier pour les vues. Le nouveau prix de la trame ne s'affiche qu'une fois les images prêtes.
   - Réafficher seulement ce qui a changé (images, vues, lignes de la trame), puis `On garde la trame ? Si oui, je lance le cut 1 : 💳 ~<X> crédits pour l'ensemble.` avec le nouveau prix (X = le `Total` de `## Prix` de `script.md`, trame corrigée entière : rien n'est encore accepté, un cut retiré fait baisser le prix).
6. Réponse qui n'est pas un « oui » explicite (« ok pour la trame », « ça me va ») : rien ne part ; une ligne : `Pour lancer le cut 1, il me faut votre « oui » : 💳 ~<X> crédits pour l'ensemble.`
7. « Oui » explicite : `etat.md` : `script:` (lu sur `SCRIPT`), `modele:` (lu sur `ROUTE`), `prix trame:`, `montant autorise:` = ce prix ; une ligne par cut dans `## Cuts`, tirée de `CUTS` (`| <n> | <phrase> | <lieu> | <vue> | <durée> s | à faire | | |`) ; `etape: 3` ; puis `## Moment 3 — Boucle par cut`.

## Moment 3 — Boucle par cut

Le client voit chaque cut en brouillon et le garde ou le corrige ; un cut gardé se finalise pendant que le suivant se tourne. `Prix :` d'un cut = la ligne `Prix :` de son bloc dans `## Prompts des cuts` de `script.md` (dernière version).

1. **Lancer un cut.** Blocage : chaque ligne de `## Inventaire` est `pret`, `existant` ou `en-mots`, l'image de base et la vue du cut ont leur job ; sinon, aucune vidéo (`## Coulisse — Images`, ou `## Moment 2 — Trame`, point 2, pour ce qui manque). `balance` : solde sous le prix → « Il manque X crédits. », `show_plans_and_credits`. Une ligne : `Cut <N> en cours, environ deux minutes.` Tournage : `Consigne : cut <N> brouillon`, `Script` (`script:` de `etat.md`), `Route`, `Montant autorisé` = prix du brouillon de ce cut. `Route` : `seedance`, sauf un cut dont le bloc porte `Prix : kling` (`Route : kling`) ; une seule `Route` par appel : un cut Kling a toujours son propre appel, pour son brouillon comme pour sa finalisation.
2. **Montrer le cut.** `show_generation_by_ids` sur le job de `CUT`. Message : les défauts renvoyés, un par ligne, s'il y en a, puis `Cut <N>/<total> : <phrase du cut>. On garde, ou je corrige ?` (phrase = colonne `phrase` de `## Cuts`).
3. **« On garde ».** Ne vaut que pour ce cut. Statut `gardé` dans `## Cuts`, ligne `cut-garde` au journal (`essais=` le nombre de brouillons du cut). Tournage, en un appel : `Consigne : cut <N> finaliser`, puis, si ce cut est le premier gardé où un personnage parle seul et que ce personnage parle encore dans un cut suivant sans extrait dans `## Voix` : `extraire voix <personnage> cut <N>`, puis `cut <M> brouillon` pour le premier cut `à faire` s'il en reste un (un cut suivant déjà en `brouillon` est montré tel quel, point 2) ; `Montant autorisé` = finalisation de ce cut + brouillon du suivant. Avant l'appel, la ligne d'attente du point 1 pour le cut suivant. Le cut suivant est montré dès son retour (point 2) ; aucune attente pour la finalisation.
   Dernier cut à garder : la ligne `Montage en cours, environ une minute.`, puis `Consigne : cut <N> finaliser ; monter`, `Montant autorisé` = finalisation de ce cut, puis `## Coulisse — Montage`, point 3. Le tournage attend toutes les finalisations ; retour `FINAL: <N>=<job id> (en cours)` sans `MONTAGE` ni `ARRÊT` : nouvel appel `Consigne : monter` (`## Coulisse — Montage`, point 2), sans rien dire de plus au client.
4. **Correction d'un cut.** Une seule modification à la fois (plusieurs → l'une après l'autre). Ligne `cut-correction` au journal. Réalisateur : `Correction : cut <N> : <la modification>`, `Cuts` (statuts) ; seul ce cut est réécrit. Message : la `PHRASE` s'il y en a une, puis `Si oui, je refais le cut <N> : 💳 ~<X> crédits.` (X = `PRIX`, le seul brouillon : la finalisation est déjà comprise dans le « oui » de la trame ; pour un cut déjà `gardé` ou `final`, `PRIX` compte aussi sa finalisation). « Oui » : `montant autorise:` augmenté de X ; cut `gardé` ou `final` : colonne `final` vidée, statut `à faire` ; puis point 1 pour ce cut. Une position de caméra nouvelle : `VUES DU LIEU` la nomme, son prix en une ligne `💳 ~X crédits`, atelier, puis la vue est montrée avec la ligne `Si oui, je refais le cut <N> : 💳 ~<X> crédits.`
5. **Même action ratée deux fois sur un cut.** `references/choix-modele.md#Arbre de décision` : la phrase de bascule de `references/choix-modele.md#Phrase au client`, réalisateur avec `Correction : cut <N> : route kling` et `Route: kling`, puis `Si oui, je refais le cut <N> : 💳 ~<X> crédits.` avec le prix Kling ; ce cut seulement. Le rendu Kling gardé est final tel quel : `Consigne : cut <N> finaliser`, `Route : kling`, seul dans son appel, `Montant autorisé : 0`.
6. **Changement de trame en cours** (ajouter, retirer ou changer un cut pas encore gardé, ou en changer l'ordre) : ligne `script-correction` au journal ; réalisateur `Correction : trame : <la modification>`, `Cuts` ; message : les lignes de trame changées, puis `On garde la trame ? Si oui, je continue : 💳 ~<X> crédits de plus.` (X = `PRIX` : cuts ajoutés ou changés). `VUES DU LIEU` autre que `aucune`, ou élément nouveau (ajouté à `## Inventaire` de `vision.md`, statut `nouveau`, comme au `## Moment 2 — Trame`, point 5) : leur prix en une ligne `💳 ~Y crédits` avant la question, Y compris dans X ; rien ne part sans ce « oui ». « Oui » : atelier d'abord pour ces images et vues (`Prix annoncé` = Y), puis `## Cuts` mis à jour depuis `CUTS` (cuts gardés et non touchés inchangés ; cut retiré `à faire` : ligne supprimée ; cut changé déjà généré : statut `à faire`, colonne `final` vidée), `montant autorise:` augmenté de X ; la boucle reprend au premier cut qui n'est pas `final` ni `gardé`.
7. Ne valent pas « oui » : le silence, « ok pour la trame », « ça me va », « on garde » (qui ne vaut que pour garder le cut montré), un « oui » à une autre question, un « oui » d'une conversation précédente.
8. Retours du tournage :
   - `ARRÊT: solde` → « Il manque X crédits. », `show_plans_and_credits` ; même appel quand le client a rechargé.
   - `ARRÊT: dépassement` → le prix qui manque en une ligne (`💳 ~X crédits de plus`, lu dans la phrase du tournage), nouveau « oui », `montant autorise:` augmenté, puis même appel (les jobs déjà soumis sont repris, jamais repayés).
   - `ARRÊT: échec` sur un brouillon → une phrase au client, puis le prix d'un nouvel essai lu dans la phrase du tournage, avec `Si oui, je refais le cut <N> : 💳 ~<X> crédits.` « Oui » : `montant autorise:` augmenté de X, point 1 pour ce cut.
   - `ARRÊT: échec` sur une finalisation, brouillon de plus de sept jours (la phrase du tournage le dit) → une phrase, puis `Si oui, je refais le cut <N> : 💳 ~<X> crédits.` (X = nouveau brouillon, ligne `Prix :` du bloc ; sa finalisation reste comprise dans le « oui » de la trame). « Oui » : `montant autorise:` augmenté de X, colonnes `brouillon` et `final` vidées, statut `à faire`, puis point 1 pour ce cut : le tournage ne reprend jamais un brouillon de plus de sept jours.
   - `ARRÊT: échec` sur une autre finalisation → une phrase, puis `Si oui, je refais le cut <N> : 💳 ~<X> crédits.` (X = nouvelle finalisation du même brouillon gardé, lue dans la phrase du tournage). « Oui » : `montant autorise:` augmenté de X, puis `Consigne : cut <N> finaliser`, suivi de `cut <N+1> brouillon` si l'appel en échec l'avait enchaîné (le tournage reprend ce job déjà soumis et le rend, sans le repayer), ou de `monter` si plus aucun cut ne reste à garder (avec sa ligne d'attente), `Montant autorisé` = X ; le cut suivant est montré au retour (point 2).
   - `VOIX: aucune — <slug> sans extrait` (extraction en échec, brouillon qui refuse l'extrait, ou `voix:<slug>` sans ligne dans `## Voix`) : le tournage a tiré le cut sans l'extrait, sans `ARRÊT`. Rien de plus au client : le cut est montré comme d'habitude (point 2). Avant le prochain appel au tournage : réalisateur avec `Correction : trame : sans extrait de voix pour <personnage>` (deux ou trois mots de voix dans les cuts encore `à faire`, `references/montage.md#Échec`), sans prix ni question. Le « oui » de la trame le couvre.

## Coulisse — Montage

1. Avant chaque appel `monter` (ou `cut <N> finaliser ; monter`), une ligne : `Montage en cours, environ une minute.`
2. Tournage : `Consigne : monter`, `Montant autorisé : compris dans la trame` (l'assemblage n'a pas de `get_cost` ; son coût, faible, est compris dans le « oui » de la trame). Retour `FINAL: <N>=<job id> (en cours)` sans `MONTAGE` : appeler à nouveau `monter`.
3. Retour `MONTAGE` : `etape: 4`, puis moment 4. `ARRÊT: échec` venu d'une finalisation : `## Moment 3 — Boucle par cut`, point 8 (finalisation en échec). `ARRÊT: échec` du montage lui-même : `montage (media id): échec` ; une phrase, aucun cut n'est refait, puis `Si oui, je refais le montage : 💳 ~<X> crédits.` (estimation faible, sinon « estimation inconnue, faible », `references/cout.md#Avant`) ; « oui » : `montant autorise:` augmenté de X, `montage (media id):` vidé, puis `Consigne : monter`, `Montant autorisé` = X. Jamais de nouvel appel sans ce « oui » (`references/montage.md#Échec`), à la reprise comme ici.

## Moment 4 — Vidéo montée

1. `show_medias` sur le media id de `MONTAGE` (à la reprise : `montage (media id):`).
2. Message : les défauts renvoyés, au plus deux, un par ligne avec leur instant, ou `Rien à signaler de mon côté.` ; puis `On garde, ou je corrige ?` Journal : ligne `video-avis` à la réponse.
3. Correction : une seule modification, sur le cut qu'elle vise (cut ambigu : une question, « Quel moment : <phrase du cut a> ou <phrase du cut b> ? ») ; ligne `cut-correction` ; réalisateur `Correction : cut <N> : <la modification>`, `Cuts` ; message `Si oui, je refais le cut <N> : 💳 ~<X> crédits.` (X = nouveau brouillon + nouvelle finalisation, montage refait compris) ; « oui » : colonne `final` de ce cut vidée, statut `à faire`, `montage (media id):` vidé, `montant autorise:` augmenté, `etape: 3`, puis `## Moment 3 — Boucle par cut`, point 1, pour ce cut seulement ; une fois gardé : `cut <N> finaliser ; monter`. Plusieurs cuts refaits l'un après l'autre : `cut <N> finaliser` seul pour chacun, et `monter` une seule fois, après le dernier gardé.
4. Voix qui ne va pas (« trop jeune », « trop aiguë ») : la correction vise le premier cut où ce personnage parle ; réalisateur avec `Correction : cut <N> : <deux ou trois mots de voix>` (`references/fiche-personnage.md#Voix`) ; suite du point 3, sauf l'appel une fois ce cut gardé : `cut <N> finaliser ; extraire voix <personnage> cut <N>`, sans `monter` (le nouvel extrait remplace l'ancien dans `## Voix`). Les autres cuts où il parle gardent l'ancienne voix : le dire en une phrase et proposer de les refaire, l'un après l'autre (réalisateur avec `Correction : cut <M> : nouvel extrait de voix`, puis `Si oui, je refais le cut <M> : 💳 ~<X> crédits.` pour chacun ; chaque cut gardé : `cut <M> finaliser` seul). Refus, ou dernier cut refait et gardé : `## Coulisse — Montage`, une seule fois.
5. Gardée : `etape: 5`.

## Sous-titres

1. Une question de style (`references/sous-titres.md#Style`) : dernier choix de `preferences.md` proposé s'il existe. Style noté dans `preferences.md`, `## Sous-titres`, à la place de l'ancien.
2. Coût en une ligne, sans « oui » : `💳 ~X crédits` (sans estimation : « estimation inconnue, faible », `references/cout.md#Avant`).
3. Tournage : `Consigne : sous-titres`, `Style`, `Montant autorisé` = le prix annoncé des sous-titres.
4. Retour `SOUS-TITRES` : `etape: 6`. `ARRÊT: échec` : une phrase, la vidéo est livrée sans sous-titres, `etape: 6`. `ARRÊT: solde` : « Il manque X crédits. », `show_plans_and_credits`.

## Bilan

1. Afficher la vidéo finale : sous-titrée (`show_medias` sur `video sous-titrée (media id):`), sinon la vidéo gardée.
2. La ligne de coût (`references/cout.md#Bilan`), une seule ligne.
3. Apprentissage : `references/apprentissage.md`, `## Promotion` (au plus une proposition au client), puis `## Retrait`.
4. `etat.md` : `etape: 7`. Le projet est fermé et ne sera plus proposé en reprise.

## Règle d'argent

Détail : `references/cout.md`.

- **Images et vues du lieu** : prix dans la ligne `Je fixe` de la carte, sans question ; une image ou une vue refaite après une correction : prix en une ligne, sans question. **Sous-titres** : prix en une ligne, sans question.
- **Trame** : prix global affiché au moment 2 (un brouillon et une finalisation par cut, extraction des voix et montage compris), puis « oui » explicite ; ce « oui » vaut pour tous les cuts de la trame. **Nouvel essai d'un cut** : son prix, puis « oui ». **Cuts ajoutés ou changés** : leur prix, puis « oui ». **Reprise** : ce qui reste à payer, puis « oui ». Le tournage reçoit, à chaque appel, le prix des consignes de cet appel, et ne dépense rien au-delà.
- L'atelier reçoit un `Plafond` = prix annoncé + 3 × une image au tarif du modèle d'image le plus cher (relance muette et les deux rendus du duel d'un échec technique). Au-delà : une phrase avec le nouveau prix avant de relancer.
- Ne valent pas accord de dépense : le silence, « ok pour la trame », « ça me va », « on garde » (qui ne vaut que pour garder le cut montré), un « oui » à une autre question, un « oui » d'une conversation précédente.
- `get_cost:true` avant chaque brouillon, chaque finalisation et chaque image quand l'outil l'accepte.
- `balance` avant chaque génération payante. Solde insuffisant : s'arrêter et annoncer le montant qui manque.
- Bascule d'un cut vers Kling : nouveau `get_cost`, nouveau prix, nouveau « oui ». Jamais l'ancien prix ni l'ancien accord. Pour une image ou les sous-titres : nouveau prix annoncé en une ligne, sans « oui » (relance muette d'un échec technique : sous le plafond, sans annonce).
- Ne jamais passer `use_unlim`. Si un outil renvoie `unlim_choice`, poser la question au client telle quelle.

## Erreurs

| Cas | Réaction |
|---|---|
| Higgsfield non connecté | Message de `## Démarrage` point 1. Stop. |
| Sous-agent sans Higgsfield (`HIGGSFIELD_INDISPONIBLE`), outil Agent absent | Repli de `## Sous-agents` : l'agent principal fait les appels lui-même, mêmes règles de message. |
| Solde insuffisant | « Il manque X crédits. », `show_plans_and_credits`. Rien n'est lancé. |
| Contenu refusé pour conformité | `references/conformite.md#Reformulations types`, une phrase ; relance = nouvel achat (cut : prix + « oui »). |
| Image en échec technique | Géré par l'atelier (`references/choix-modele.md#Échec technique d'une image`) ; au 3e échec, sa `PHRASE`. |
| Image refusée par le client | Ligne `image-refus`, duel (`references/choix-modele.md#Duel`). |
| Cut raté, 1re fois | Une phrase ; réalisateur `Correction : cut <N>` ; M3, nouveau « oui ». |
| Même action ratée une 2e fois sur un cut | `references/choix-modele.md#Arbre de décision`, Kling pour ce cut ; M3. |
| Brouillon de plus de sept jours (date de la colonne `brouillon`) | Jamais finalisé ni repris : nouveau brouillon, M3 (à la reprise : la ligne de prix de `## Démarrage`, point 4), nouveau « oui », colonnes `brouillon` et `final` vidées, statut `à faire`. |
| Extrait de voix en échec, refusé ou absent | Le tournage tire le cut sans extrait (`VOIX: aucune — <slug> sans extrait`) ; réalisateur pour les mots de voix des cuts `à faire` ; aucun prix, aucune question. |
| Dépassement du montant autorisé | Nouveau prix, nouveau « oui ». |
| Timeout, réponse perdue, conversation coupée | Reprendre le job par son id (`## Jobs payants`, `## Cuts`), jamais de nouvelle soumission à l'aveugle. |
| Sous-titres en échec | Vidéo livrée sans sous-titres, une phrase. |

M3 : `Si oui, je refais le cut <N> : 💳 ~<X> crédits.`
