# L'administration des clients

> **In English** — The admin interface where each client manages their own website, and the
> platform layer where I prepare sites and invite clients. Designed from user journeys and
> mock-ups before any code, with one promise: anyone can run their website. Desktop first for real
> work, with a mobile core for daily tasks (answering a request, announcing a closure, marking a
> product as sold out). Requests follow each trade's own steps. Edits are saved field by field to a
> draft, previewed with the real site rendering, and published with a computed summary; short
> tasks go live at once without exposing the draft. New sites start from the demo of their trade,
> but publication is blocked while invented facts remain. Accounts exist only by invitation; the
> platform account requires an authenticator app. Principles only: no security code is shown.

_État au 12 novembre 2026. Les écrans décrits au présent sont en place ; le reste est repris en
fin de page. Les exemples viennent des démos (contenus fictifs)._

## La promesse

Mes futurs clients ont une crainte commune : « je n'y arriverai pas ». Une restauratrice ou un
chocolatier n'a ni le temps ni l'envie d'apprendre un logiciel. Avant d'écrire le code, j'ai
donc posé une promesse, que chaque écran doit tenir : en 2026, n'importe qui peut gérer son site.

Concrètement, le client voit ce qu'il change (sur la page elle-même), il ne peut rien casser (la
mise en page est tenue par les blocs, rien n'est visible avant « Publier »), on lui parle avec ses
mots (« Mes produits », « Demandes », jamais « bloc » ni « configuration »), et l'accueil lui
propose des actions plutôt qu'un arbre de pages.

J'ai conçu les parcours du client et les miens, puis des maquettes, relues et validées avant le
premier écran. Chaque lot de développement part d'une maquette validée.

## Ordinateur d'abord, avec un noyau mobile

Les sites publics sont conçus pour le téléphone d'abord. L'administration, non : le vrai travail
(rédiger, traduire, recadrer une photo) se fait assis, et un aperçu à côté du formulaire rassure
un client qui n'est pas technicien. Je n'ai donc pas bridé les écrans pour qu'ils tiennent sur un
téléphone. _(D-086)_

En revanche, les gestes du quotidien se font entre deux clients, téléphone en main. Ceux-là sont
conçus pour le téléphone avec le même soin que les sites : répondre à une demande, faire une
annonce, déclarer une fermeture exceptionnelle, marquer un produit épuisé.

Chaque utilisateur choisit l'apparence de son administration : claire, sombre, ou celle de son
appareil. L'aperçu du site, lui, garde toujours les couleurs du site.

## Les demandes, avec les mots du métier

Commandes, réservations, demandes de devis et messages arrivent dans une même liste. Ma première
version marquait une demande comme traitée dès qu'on l'ouvrait. En l'essayant sur mon téléphone,
je me suis rendu compte que c'était faux : une demande n'est traitée que quand le commerçant le
dit, et ses étapes dépendent du métier. _(D-089, D-090)_

Chaque site a donc ses **circuits** : pour un chocolatier, une commande passe par « nouvelle »,
« en préparation », « prête », puis « retirée par le client ». Le logiciel, lui, ne connaît que
quatre rôles (à traiter, en cours, terminée, abandonnée) : les compteurs et les filtres restent
simples, quel que soit le vocabulaire du métier.

Pour répondre, l'écran prépare un message (« Bonjour Marie, votre commande sera prête samedi à
partir de 10 h ») et l'ouvre dans WhatsApp, les SMS ou l'e-mail **de l'appareil du client**. La
réponse part de son numéro et de son adresse, pas d'un robot de la plateforme.

D'autres retours d'usage ont suivi : sur ordinateur, la première demande à traiter s'ouvre
d'elle-même ; changer de demande ne recharge que le détail, avec une silhouette de chargement au
lieu d'un écran blanc.

## Publier sans surprise

Tout ce que le client modifie va dans un **brouillon**. Le bouton « Publier », toujours visible,
indique le nombre de modifications en attente. _(D-093)_

- **L'aperçu ne ment pas.** La page du brouillon s'affiche dans un cadre, avec le vrai rendu du
  site. En mode « téléphone », le cadre a la largeur d'un téléphone : ce sont les vraies règles
  d'affichage mobiles qui s'appliquent, pas une imitation.
- **Chaque champ s'enregistre seul**, après une courte pause de saisie. L'écran envoie l'ancienne
  et la nouvelle valeur du champ ; si quelqu'un d'autre a changé ce même champ entre-temps, rien
  n'est écrasé et le texte saisi reste à l'écran. Deux personnes peuvent donc travailler sur le
  même site, sur deux parties différentes, sans se gêner.
- **Le récapitulatif est calculé**, pas tenu à jour : c'est la différence entre le brouillon et
  la version en ligne. Une modification défaite à la main disparaît d'elle-même de la liste.
- **On peut tout abandonner** : le brouillon reprend la version en ligne, et ce qui a été
  abandonné reste récupérable.

Les tâches courtes du téléphone ne passent pas par là. Une annonce ou une fermeture
exceptionnelle part en ligne tout de suite, **sans emporter le brouillon** en cours : la
modification est appliquée à la fois au brouillon et à une copie de la version en ligne, qui
devient une nouvelle version. Un commerçant qui annonce sa fermeture du lundi ne publie pas par
accident la page qu'il réécrivait la veille. _(D-091)_

## Contenus, photos et cartes

- **Les contenus du métier** (produits, œuvres, personnes, lieux) se listent, s'ajoutent en
  commençant par la photo, et se retirent après une confirmation qui dit où le contenu est montré
  sur le site. _(D-095)_
- **La médiathèque** suit chaque photo envoyée, signale celles qui ne servent nulle part, et
  laisse choisir d'un clic le point qui doit rester visible quand la photo est recadrée.
  _(D-094)_
- **La carte d'un lieu** est cherchée une fois à partir de l'adresse, ajustée si besoin, puis
  dessinée **dans le navigateur** de la personne qui la règle et enregistrée comme une image. Le
  site n'appelle ensuite aucun service de carte : pas de traceur pour les visiteurs, et la carte
  part avec l'export comme n'importe quelle photo. _(D-096)_

## Du client au site, dans l'ordre

Après une journée de développement, l'outil savait inviter un client, mais pas encore le créer.
Moi qui l'utilise, je m'y perdais. J'ai donc adopté une règle simple : construire les
écrans dans l'ordre de la vie d'un client. On crée le client avec son premier site, on prépare le
site, on invite le client quand le site est prêt, puis on suit depuis le tableau de bord.

- **Un assistant de création** enchaîne le client, le métier, le modèle de pages et le style,
  avec un aperçu en direct : la vraie page d'accueil change à chaque choix.
- **Avant que le client ait un compte**, je peux préparer son site ; dès sa première connexion,
  c'est lui qui décide si je peux intervenir, et il retrouve dans l'historique ce qui a été fait.
  _(D-101)_

## Partir d'une démo, sans jamais publier de faux

Un nouveau site part de la démo de son métier : textes, structure, style et photos libres de
droits. Le client découvre un site complet et crédible dès le premier jour, au lieu d'une page
vide. Mais une démo contient des faits inventés : une adresse, des prix, des horaires.
_(D-102)_

- **Chaque contenu repris est marqué « exemple ».** Une pastille l'indique dans l'éditeur, et
  l'accueil affiche une liste « Reste à remplacer ».
- **Un texte d'exemple peut être publié**, avec un avertissement. **Une information d'exemple
  bloque la publication** (adresse, téléphone, horaires, prix, allergènes) : un vrai site ne doit
  jamais afficher un fait inventé au nom du client.
- **Une information déjà juste se confirme d'un clic**, une à la fois, jamais en bloc ; un bouton
  « Garder ce texte » évite de forcer une fausse retouche.
- **Les images générées par IA ne passent jamais dans un modèle** : seules les photos libres de
  droits suivent, avec leur licence.

## Des comptes sûrs, sans effrayer personne

Je décris ici les principes, pas le code : un dépôt public n'est pas l'endroit pour montrer une
implémentation de sécurité.

- **Pas d'inscription ouverte.** Un compte ne naît que d'une invitation. Le lien vaut sept jours
  et ne sert qu'une fois.
- **Ouvrir le lien ne suffit pas, il faut un clic.** Certaines messageries ouvrent les liens des
  e-mails pour les analyser ; sans ce clic, elles consommeraient l'invitation à la place du
  client. La même règle vaut pour le lien de connexion par e-mail.
- **La base de données vérifie elle-même** qu'un nouveau compte correspond à une invitation
  valide : une erreur dans le code ne suffit pas à créer un compte.
- **La page de connexion ne dit jamais** si une adresse a un compte.

**Double authentification.** Mon compte ouvre tous les sites de tous les clients : un mot de passe
volé ne doit pas suffire. Il exige donc un code d'application (norme ouverte TOTP), plutôt qu'un
SMS : gratuit, sans réseau, sans opérateur ni service tiers. Deux détails m'ont occupé plus que
prévu. _(D-103)_

- Ce compte ne reçoit **pas de lien de connexion par e-mail** : le lien aurait contourné le second
  facteur, et la boîte e-mail serait devenue la seule clé.
- **Les codes de secours se recopient sans erreur** : minuscules et chiffres, sans les caractères
  qui se confondent (0 et o, 1, l et i).

Pour les clients, la double authentification sera facultative. Ajouter une étape de plus à des
utilisateurs qui craignent déjà la technique irait contre la promesse du début.

## La méthode

- **Maquettes d'abord**, validées avant chaque écran.
- **Relecture en captures** (ordinateur et téléphone) avant chaque PR. Elle a trouvé un défaut
  que les tests automatiques ne voyaient pas : juste après l'activation de la double
  authentification, les codes de secours ne s'affichaient pas.
- **Essais sur téléphone**, puis un petit lot par remarque, comme pour les
  [commerces](commerces.md).

## En étude, bientôt

| Quoi                                                           | Quand (visé)              |
| -------------------------------------------------------------- | ------------------------- |
| Actualités, langues et traductions dans l'administration       | premier client, déc. 2026 |
| Historique, remise en ligne d'une version, inviter un collègue | premier client, déc. 2026 |
| Autorisation d'aide donnée par le client depuis ses réglages   | premier client, déc. 2026 |
| Double authentification facultative pour les clients           | version 1, janv. 2027     |
| Paquet d'export et aides IA                                    | version 1, janv. 2027     |
