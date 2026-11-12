# Commerces : vendre sans paiement en ligne

> **In English** — How the platform serves small shops without online payment. Generic building
> blocks (a Product record, a Products block, a year calendar computed in the browser) were
> designed for three fictional shops: a chocolatier, a craft shop and a second-hand clothes shop.
> Visitors can order for in-store pickup: the cart lives in the browser, the server recomputes
> every price, and no email is ever sent to the address typed by the visitor. The shop can be
> filtered through the URL (aisle, theme, size), so a link from the home page opens it already
> filtered, without JavaScript. Each choice came from a remark made while testing on a phone.

_État au 12 novembre 2026. Démos : [commerce.praxora.fr](https://commerce.praxora.fr) (chocolatier),
[commerce2.praxora.fr](https://commerce2.praxora.fr) (loisirs créatifs),
[commerce3.praxora.fr](https://commerce3.praxora.fr) (seconde main). Entreprises et personnes
fictives._

## Le point de départ

Début novembre, j'ai ajouté une catégorie de modèles « Commerces » aux démos déjà en ligne (yoga,
restaurants, artisans). Un petit commerce n'a pas les mêmes besoins qu'un restaurant : il montre
des produits avec leur prix, il vit au rythme des fêtes, et il reçoit des commandes, surtout en
décembre. Il n'a en revanche ni boutique en ligne ni envie d'en gérer une.

Trois boutiques fictives ont servi de banc d'essai, chacune ajoutant ce qui manquait à la
précédente :

| Démo                       | Commerce                           | Ce qu'elle a ajouté                                      |
| -------------------------- | ---------------------------------- | -------------------------------------------------------- |
| **Lauch & Cacao**, Colmar  | chocolaterie artisanale            | fiche Produit, calendrier de l'année, commande à retirer |
| **Pelote & Papier**, Tours | loisirs créatifs, ateliers et kits | boutique filtrable par rayon et par thème                |
| **Seconde Étoffe**, Rennes | friperie et dépôt-vente            | pièce unique, pièce « réservée », filtre par taille      |

## Des briques pour tous les commerces, pas pour un métier

La règle du projet est de ne pas créer de bloc propre à un métier (voir
[Conception](conception.md), § 5). Les briques des commerces servent donc aussi bien un
chocolatier qu'une mercerie ou une friperie.

- **La fiche Produit** : nom, collection, photos, prix, disponibilité dans une liste fermée (toute
  l'année, de saison, sur commande, pièce unique, épuisé), caractéristiques et allergènes. Le prix
  s'écrit en texte libre, parce qu'un commerçant dit « les 100 g » ou « à partir de 12 € », et
  qu'un champ numérique l'aurait forcé à mentir. La mention « fait maison » n'y est pas proposée :
  la loi la réserve à la restauration. _(décision D-069)_
- **Le calendrier de l'année** : des périodes par mois (Noël, Pâques, la rentrée), sans dates
  exactes à ressaisir chaque année. L'encart « en ce moment » de l'accueil est calculé dans le
  navigateur du visiteur, à l'heure de Paris : la page publiée reste en cache, et sans JavaScript
  la frise complète reste lisible. _(D-070)_

## La commande à retirer

Le besoin est venu de la chocolaterie : prendre les commandes de fêtes sans paiement en ligne.
Le visiteur prépare un panier et envoie une demande ; le commerçant confirme, et le client paie en
boutique en venant chercher sa commande. _(D-072)_

Ce que j'ai choisi, et pourquoi :

- **Le panier reste dans le navigateur**, sans compte ni cookie. Tant que rien n'est payé, rien ne
  justifie de le garder sur le serveur.
- **Le serveur seul connaît les prix.** La demande ne transmet que des identifiants de produits et
  des quantités. Le serveur relit les fiches publiées, recalcule le total et le recopie dans la
  commande : un prix modifié dans le navigateur ne sert à rien, et un prix changé plus tard ne
  modifie pas une commande déjà passée.
- **Un seul jour de retrait**, choisi parmi les jours d'ouverture, après le plus long délai de
  préparation du panier. Si un produit retarde tout, le panier le nomme et propose de faire deux
  commandes.
- **Un numéro de commande lisible au téléphone**, et un récapitulatif à dicter pour qui préfère
  appeler.
- **Aucun e-mail envoyé au visiteur.** C'était ma première idée (« votre commande est bien
  reçue »), et je l'ai abandonnée : un formulaire qui écrit à l'adresse qu'on y tape peut servir à
  envoyer des messages à n'importe qui. Le visiteur voit son numéro à l'écran ; le commerçant
  reçoit l'e-mail et confirme lui-même, par e-mail, SMS ou téléphone.

Le paiement en ligne viendra plus tard, comme un module qu'on branche ou non, sur ces mêmes
commandes.

## Une boutique qu'on filtre par l'adresse

Pour la boutique de loisirs créatifs, je voulais qu'un lien de l'accueil (« Préparer Noël »)
ouvre la boutique déjà filtrée. Le filtre s'écrit donc dans l'adresse :
`/boutique?rayon=laine-et-tricot&theme=noel`. Le serveur l'applique, l'adresse se partage et se
garde en favori, le bouton retour fonctionne, et tout marche sans JavaScript. _(D-076, D-077)_

Les choix suivants sont venus de mes essais sur téléphone, une remarque à la fois :

- **Rayons et thèmes se combinent.** Avec un thème choisi, les rayons qui n'ont aucun produit pour
  ce thème disparaissent.
- **Le nombre de produits** s'affiche à côté du titre de chaque rayon.
- **Pas de pastille « × »** pour retirer un filtre. Je l'avais demandée, puis retirée : elle
  doublait les boutons de filtre, visibles juste au-dessus. Un lien « Effacer les filtres »
  n'apparaît que quand deux filtres sont combinés.
- **Changer de filtre ne recharge pas la page** : seul le contenu change, l'en-tête et la position
  restent en place. Sans JavaScript, c'est un lien ordinaire.

La friperie a ajouté un troisième filtre, sur une caractéristique au choix du commerçant (la
taille, ici ; ce pourrait être une pointure ou l'âge d'un jouet), avec un ordre de tri qui place
« XS » avant « M » et « 38 » avant « 40 ». _(D-079)_ Elle a aussi demandé deux disponibilités :
une **pièce unique** ne se commande qu'en un exemplaire, et une pièce **réservée** reste visible
mais ne se met pas au panier, le temps que la cliente vienne l'essayer. _(D-078)_

## La méthode

La commande à retirer a été livrée en quatre lots, chacun fusionné et vérifié avant le suivant :
les prix de vente, le panier, la demande de commande, puis les cas limites relevés à l'usage. La
boutique filtrable a suivi le même rythme : une remarque sur téléphone, un petit lot, une
vérification, puis la remarque suivante.

Les démos servent aussi de tests. Chaque numéro de téléphone d'une démo doit appartenir aux
tranches que l'Arcep réserve à la fiction : un numéro inventé au hasard pourrait être celui d'un
vrai abonné, qui recevrait les appels des visiteurs. Le test a trouvé un numéro oublié dès sa
première exécution.

## En étude, bientôt

- **Paiement en ligne, créneaux de retrait et stocks**, en module facultatif (version 2, visée en
  février 2027).
- **Recherche, tri et pages** pour les catalogues de plus de quelques dizaines de produits, quand
  un vrai client en aura besoin.
