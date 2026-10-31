# Gestion de projet sur GitHub

> **In English** — How I run praxora's projects with plain GitHub features: a ticket before any
> work, each with a milestone, a priority, a size and its dependencies; sub-issues for large
> tickets; a backlog label so that nothing postponed gets lost; a project board with roadmap
> views; one pull request per coherent batch, merged by me; and lessons learned along the way
> ("Closes" only works in English, stacked pull requests need care).

_État au 31 octobre 2026._

## Pourquoi GitHub seul

Les outils standard et gratuits suffisent : le code, les tickets, les décisions et le suivi sont
**au même endroit**, reliés entre eux, et lisibles par n'importe quel développeur qui
rejoindrait le projet. Pas d'outil de gestion de projet à part, pas de double saisie.

## Un ticket avant chaque travail

Tout travail commence par un ticket (une _issue_), même une correction d'une ligne. Il est
**commenté à chaque étape** : début, livraison (la PR), mise en production. Les commentaires de fin
de session servent de **notes de passage** : la session suivante (ou quelqu'un d'autre) reprend
sans rien reconstituer.

Chaque ticket reçoit **dès sa création** :

| Champ            | Valeurs                                 | Sert à                                      |
| ---------------- | --------------------------------------- | ------------------------------------------- |
| **Jalon**        | Démos en ligne, Premier client, V1, V2… | savoir ce qui reste avant chaque étape      |
| **Priorité**     | `priorite:haute`, `moyenne`, `basse`    | choisir le prochain travail                 |
| **Taille**       | `taille:S`, `M`, `L`                    | estimer ; un ticket L est découpé           |
| **Dépendances**  | « bloqué par » d'autres tickets         | ne pas commencer ce qui ne peut pas aboutir |
| **Sous-tickets** | pour les tickets de taille L            | avancer et livrer par morceaux              |

**Le prochain travail** se choisit ainsi : le ticket de priorité haute du jalon le plus proche
dont rien ne bloque.

## Rien de ce qui est reporté ne se perd

Quand une décision remet quelque chose à plus tard (« en V2 », « quand un deuxième métier en aura
besoin »), un ticket est créé **tout de suite**, avec l'étiquette `backlog` (et `v2` si c'est
après la V1), et il **cite la décision** qui l'a reporté. Six mois plus tard, on sait ce qui a été
écarté, pourquoi, et où le reprendre.

## Le tableau de projet

Un tableau GitHub Projects, alimenté automatiquement par les tickets, avec plusieurs vues :

| Vue                  | Pour                                         |
| -------------------- | -------------------------------------------- |
| **Tableau de bord**  | à faire, en cours, terminé                   |
| **Tous les tickets** | trier et filtrer par jalon, priorité, taille |
| **Feuille de route** | les jalons dans le temps                     |
| **Backlog**, **V2**  | ce qui attend, sans encombrer le reste       |

Le ticket se ferme à la fusion de sa PR, et le tableau suit.

## Les pull requests

- **Une PR par lot cohérent**, pas une énorme PR en fin de chantier : chacune se relit en
  quelques minutes.
- Branches nommées d'après le ticket (`186-cartes`, `158-theme-cuivre`).
- Une PR qui termine un ticket écrit **« Closes #N »** : le ticket se ferme à la fusion.
- La CI doit être verte ; **je relis et je fusionne** moi-même
  ([la vérification](integration-continue.md)).
- La description dit **quoi relire en priorité** et donne les **commandes de mise en production**
  prêtes à copier.

## Plusieurs dépôts, une entreprise

Chaque projet a son dépôt, ses tickets, ses décisions et son journal. Quand une décision touche
un autre projet (serveur, domaine, marque, textes juridiques), un **ticket est ouvert dans le
dépôt concerné**, qui cite le ticket d'origine. Même règle pour ce book : une étape d'un projet
privé qui mérite d'être montrée devient un ticket ici.

## Ce que j'ai appris

- **« Closes » plutôt que « Ferme ».** GitHub ne reconnaît que les mots-clés anglais. Au début,
  mes PR écrivaient « Ferme #N » et les tickets restaient ouverts ; il a fallu les fermer à la
  main, puis corriger la règle.
- **Les PR empilées demandent de la rigueur.** Une PR ouverte sur la branche d'une autre a été
  fusionnée après elle, donc dans une branche déjà fusionnée : son contenu n'est jamais arrivé sur
  `main`. Règle depuis : toute PR vise `main` ; si elle dépend d'une autre, elle part de sa
  branche et les contient toutes les deux, si bien que l'ordre de fusion ne compte plus.
- **Le tableau ne se tient pas tout seul.** Les jalons, étiquettes et dépendances posés dès la
  création du ticket le remplissent ; un ticket créé « vite fait » sans eux disparaît des vues.
