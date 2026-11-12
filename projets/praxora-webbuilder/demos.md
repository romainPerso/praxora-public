# Démos

> **In English** — Thirteen live demo websites built with the platform, for five kinds of
> business: yoga, restaurants, tradespeople, shops and an artist. Every business, person and
> review in them is fictional; places are real. Photos come from free stock libraries (Pexels).
> AI-generated images (open-licence models, run on my own machine) are used only where no free
> photo can be used honestly: the "before" of some before/after comparisons, and the artworks of
> the fictional artist, so that no real painter's work is attributed to an invented character.

_Entreprises, personnes et avis fictifs ; lieux réels. Les démos ne sont pas référencées par les
moteurs de recherche. État au 12 novembre 2026._

| Démo                                                 | Métier                                 | Ce qu'elle montre                                                            |
| ---------------------------------------------------- | -------------------------------------- | ---------------------------------------------------------------------------- |
| [yoga.praxora.fr](https://yoga.praxora.fr)           | association de yoga, une salle         | planning de la semaine, tarifs, équipe, carte d'accès                        |
| [yoga2.praxora.fr](https://yoga2.praxora.fr)         | collectif de professeurs, deux lieux   | fiches partagées (professeurs, lieux), une page par professeur               |
| [yoga3.praxora.fr](https://yoga3.praxora.fr)         | praticien indépendant, plusieurs lieux | cours, séances sur mesure, entreprises, une carte par lieu                   |
| [restau.praxora.fr](https://restau.praxora.fr)       | bistrot                                | carte du restaurant, ardoise du jour, horaires structurés                    |
| [restau2.praxora.fr](https://restau2.praxora.fr)     | table gastronomique                    | menus dégustation, distinctions, thème sombre                                |
| [restau3.praxora.fr](https://restau3.praxora.fr)     | food truck                             | tournée de la semaine, « où est le camion maintenant », demandes d'événement |
| [artisan.praxora.fr](https://artisan.praxora.fr)     | menuisier-ébéniste                     | réalisations filtrables, avant / après, zone d'intervention, devis           |
| [artisan2.praxora.fr](https://artisan2.praxora.fr)   | paysagiste                             | contrats d'entretien, saisons, avant / après                                 |
| [artisan3.praxora.fr](https://artisan3.praxora.fr)   | plombier-chauffagiste                  | dépannage aux prix affichés, chauffage, salles de bains                      |
| [commerce.praxora.fr](https://commerce.praxora.fr)   | chocolatier                            | produits avec prix et allergènes, calendrier des fêtes, commande à retirer   |
| [commerce2.praxora.fr](https://commerce2.praxora.fr) | loisirs créatifs                       | boutique filtrable par rayon et par thème, ateliers et kits à retirer        |
| [commerce3.praxora.fr](https://commerce3.praxora.fr) | friperie et dépôt-vente                | pièces uniques, filtre par taille, mise de côté pour essayer                 |
| [artiste.praxora.fr](https://artiste.praxora.fr)     | peintre et sculpteur                   | mur d'œuvres filtrable, agenda des expositions, commandes, cours d'atelier   |

Les trois commerces ont leur page : [Commerces : vendre sans paiement en ligne](commerces.md).

## Ce qu'il faut regarder

- **Sur téléphone** d'abord : chaque variante de bloc est conçue pour le petit écran, avec un
  bouton d'action rapide (appeler, réserver, devis, panier).
- **Le même moteur, des sites différents** : les treize démos partagent les mêmes blocs ; seuls la
  structure, le thème et les contenus changent.
- **Le passage en anglais** (bouton « EN ») : tout est traduit, adresses comprises.

## Le sujet décide de la forme

La démo artiste est la dernière de la série. Elle montre une règle que j'applique à chaque démo :
la mise en page suit le sujet, jamais le hasard. Ici, l'accueil s'ouvre sur une œuvre en plein
écran, parce que c'est ce qu'un visiteur vient voir chez un peintre.

Le sujet : Camille Darras, peintre et sculpteur fictif à Honfleur, qui vit de trois activités.
Ses œuvres, des commandes sur mesure, et les cours de son atelier, où chaque élève vient avec son
propre projet. Le fil rouge du site tient en une phrase : « Venez avec une idée, repartez avec
votre œuvre ».

Cette démo a ajouté trois briques au catalogue, génériques comme les autres :

- **une fiche Œuvre et un bloc Œuvres** : un mur en colonnes où chaque image garde son format
  (une œuvre ne se recadre pas), un cartel sous chaque œuvre, une pastille « Vendue » ou
  « Réservée », et un filtre par technique dans l'adresse (`?technique=fusain`). Une œuvre n'est
  pas un produit : elle ne va pas au panier, son prix est souvent « sur demande », et elle reste
  montrée une fois vendue ;
- **un bloc Agenda** : expositions, vernissages, portes ouvertes. Un événement passé se range de
  lui-même dans la liste des événements passés, à la date du jour, même sur une page en cache ;
- **une demande de projet**, troisième forme de la demande de devis, pour une commande sur mesure
  (un portrait, un tableau, un faire-part) : sujet, technique, format, échéance.

Le thème est sombre, en alternance de noir et de blanc cassé, avec une seule couleur, le
vermillon. Ses sections sombres portent une texture de coups de pinceau
([voir Conception](conception.md), § 11).

## Les images

Ordre de priorité : photo libre de droits ; à défaut, photo libre modifiée par une IA
générative ; en dernier recours, image créée par l'IA. Les modèles d'IA sont à licence ouverte et
tournent sur ma propre machine.

Deux cas ont demandé l'IA :

- **Les comparaisons avant / après des artisans** : les « avant » sont générés sur la géométrie de
  la vraie photo, et signalés comme tels sur les sites.
- **Les œuvres de l'artiste et de ses élèves.** Les banques libres ont des photos de tableaux, mais
  ce sont les tableaux de vrais peintres : les attribuer à un artiste inventé aurait été faux.
  Les œuvres sont donc générées. Les photos d'atelier, elles, viennent d'une banque libre et ne
  montrent aucun tableau de tiers.

Jamais d'image générée pour un vrai client : ses réalisations sont ses photos.

## Ce qui ne se voit pas

Les numéros de téléphone des démos appartiennent tous aux tranches que l'Arcep réserve à la
fiction, et un test le vérifie : un numéro inventé au hasard pourrait sonner chez un vrai abonné.

Les démos servent aussi de point de départ aux sites des clients : un nouveau site part de la
démo de son métier, et la publication reste bloquée tant qu'un fait inventé y figure (voir
[l'administration](administration.md)).

## En étude, bientôt

La série est complète pour le moment. Une nouvelle démo viendra quand un métier le demandera.
