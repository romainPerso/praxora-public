# CLAUDE.md — praxora-public

Contexte à lire au début de chaque session de travail avec Claude sur ce dépôt.

## Le projet

**praxora-public** est la vitrine publique du travail de Romain Moussac autour de praxora : un
« book », comme celui d'un graphiste ou d'un photographe, mais pour un développeur. Il montre la
conception, les choix techniques, des exemples de code et la méthode, à partir des projets privés.

- **Public visé** : entreprises et recruteurs (éditeurs de logiciels, ESN, en Aveyron ou à
  distance), prospects curieux de la technique. Un lecteur pressé doit comprendre en deux minutes
  qui est Romain et ce qu'il sait faire ; un lecteur technique doit pouvoir creuser.
- **Dépôt public** : tout ce qui est poussé ici est visible de tous, pour toujours (l'historique
  git garde ce qu'on supprime). Dans le doute, on ne publie pas.
- Évolue **en parallèle** des projets privés : on y reporte ce qui mérite d'être montré, quand
  c'est stable.

## Langue et format (choix de Romain, 2026-10-31)

- Contenu en **français**. Accueil (`README.en.md`) et résumé de chaque page aussi en **anglais**.
- **Markdown lisible directement sur GitHub** : titres courts, paragraphes brefs, tableaux, schémas
  Mermaid (GitHub les affiche). Un petit site GitHub Pages viendra plus tard (ticket).
- Ton : précis, sobre, à la première personne pour Romain (« je »). Pas de superlatifs ni de
  chiffres invérifiables ; on montre plutôt qu'on affirme.

## Confidentialité : ce qu'on publie, ce qu'on ne publie jamais

Les dépôts d'origine sont **privés**. Rien n'est copié automatiquement : chaque page est **écrite
pour ce dépôt**, et chaque extrait de code est **choisi, simplifié et relu**.

**Jamais** :

- identifiants, jetons, clés, mots de passe, même factices ressemblant à des vrais ;
- adresses IP, noms de machines, ports, chemins du serveur ou du poste de Romain, configuration
  du serveur (Caddy, Docker de production, DNS), procédures d'administration ;
- code de sécurité tel quel : authentification, cloisonnement (requêtes et politiques exactes),
  contrôles d'accès — on en décrit le **principe**, pas l'implémentation ;
- données ou noms de clients, prospects, personnes réelles (sauf Romain), échanges privés ;
- tarifs, offres et éléments commerciaux non publiés sur www.praxora.fr ;
- textes juridiques en brouillon (`docs/juridique/` du webbuilder).

**Oui** :

- principes d'architecture, schémas, décisions et leurs raisons (le « pourquoi ») ;
- extraits de code **non sensibles** et autonomes (déclaration d'un bloc, migration, test,
  composant d'affichage), avec leur origine (dépôt, fichier, date) ;
- méthode de travail, y compris la façon d'utiliser l'IA et ses limites ;
- démos publiques (`*.praxora.fr`) et le site www.praxora.fr ;
- captures d'écran des démos (contenus fictifs).

**Avant chaque PR** : relire le diff avec la liste « Jamais » ci-dessus ; un doute = retirer, ou
demander à Romain. Romain relit et fusionne lui-même chaque PR.

## Sources

| Projet             | Dépôt (privé)                    | Ce qu'on y trouve pour le book                         |
| ------------------ | -------------------------------- | ------------------------------------------------------ |
| praxoraXwebbuilder | `romainPerso/praxora-webbuilder` | `docs/decisions.md`, `docs/blocs.md`, code des blocs   |
| praxora-compta     | `romainPerso/praxora-compta`     | `docs/` (étude), `docs/ecosysteme.md` (vue d'ensemble) |

Chaque page indique **à quelle date** elle reflète le projet (« État au … ») : le book montre une
étape, pas la vérité du jour.

## Organisation

| Dossier ou fichier          | Contenu                                                                            |
| --------------------------- | ---------------------------------------------------------------------------------- |
| `README.md`, `README.en.md` | Accueil (français, anglais)                                                        |
| `profil.md`                 | Parcours, compétences, façon de travailler                                         |
| `praxora/`                  | L'entreprise et l'écosystème de projets                                            |
| `projets/<projet>/`         | Une étude de cas par projet : présentation, conception, choix techniques, exemples |
| `methode/`                  | Méthode transverse : IA, qualité, sécurité et RGPD                                 |

## Méthode

- Même méthode que les projets privés : **un ticket** (issue de ce dépôt) avant chaque travail,
  commenté au début et à la livraison ; **une PR par lot cohérent**, fusionnée par Romain ; une PR
  qui termine un ticket écrit « Closes #N ».
- Les tickets de ce dépôt sont **publics** : pas de détail confidentiel dedans non plus.
- Langue des commits, tickets et PR : français.
- Quand un projet privé franchit une étape (nouvelle décision importante, démo, mise en ligne),
  ouvrir un ticket ici pour la reporter.
- Fin de session : note de passage dans le ticket en cours (fait, reste, questions).
