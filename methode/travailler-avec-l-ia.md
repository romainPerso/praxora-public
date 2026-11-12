# Travailler avec l'IA

> **In English** — How I work with an AI (Claude) on praxora: it acts as a technical architect
> who proposes, implements and tests, while I decide, review and merge. The collaboration is
> framed by written context (a CLAUDE.md per repository), a ticket for every piece of work,
> written decisions, a session journal, reusable procedures ("skills") and hard rules (no
> secrets, no AI-generated images for real clients, legal texts reviewed by a lawyer). Mistakes
> become rules. Since November 2026, every text written for humans (websites, leaflets, client
> documents) goes once through a "humanizer" skill that removes AI writing tics, without changing
> any fact.

_État au 12 novembre 2026._

## Le principe

Je travaille avec **Claude** (Anthropic) comme avec un **collègue architecte** : il propose, il
code, il teste et il documente ; **je décide, je relis et je fusionne**. Je lui demande de me
contredire quand un choix technique est mauvais, et de proposer des idées fonctionnelles pour mes
clients. L'IA va vite ; le cadre ci-dessous sert à ce qu'elle aille vite **dans la bonne
direction**, et à ce que tout reste compréhensible sans elle.

## Le cadre écrit

| Outil                      | Rôle                                                                                                                                           |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| `CLAUDE.md` (un par dépôt) | le contexte lu au début de chaque session : principes non négociables, méthode, pile, où trouver quoi                                          |
| **Tickets** GitHub         | un ticket avant chaque travail, commenté au début, à la livraison et à la mise en ligne ; tout ce qui est reporté reçoit un ticket « backlog » |
| `docs/decisions.md`        | chaque choix structurant, avec ses raisons et ce qui a été écarté (plus de 100 à ce jour)                                                      |
| `docs/glossaire.md`        | le vocabulaire : tout nouveau concept y entre avant le code                                                                                    |
| `docs/journal.md`          | une entrée par session, écrite pour un humain : décisions, difficultés, progrès, suite                                                         |
| **Note de passage**        | en fin de session, dans le ticket en cours : ce qui est fait, ce qui reste, les questions                                                      |
| **Skills**                 | les procédures qui reviennent (créer une démo, ajouter un bloc, déployer, clore une session), écrites une fois et suivies à chaque fois        |

## Le déroulé type

1. Je décris un besoin ou un retour (souvent après avoir testé sur mon téléphone).
2. Claude lit le contexte, crée ou reprend le ticket, et **propose** : concept, options,
   recommandation. Pour tout ce qui engage (concept d'une démo, nouvelle brique, choix de pile),
   rien n'est codé avant mon accord.
3. Il implémente par **lots cohérents**, chacun dans une PR avec ses tests, et vérifie le rendu
   sur des captures (ordinateur et mobile) avant de me le présenter.
4. Je relis et je fusionne. Il me donne les commandes exactes pour le serveur ; c'est moi qui
   les lance.
5. Fin de session : journal, note de passage, préparation de la suivante.

## Les règles qui ne se discutent pas

- **Aucun secret** (mot de passe, jeton, clé) n'est répété ni écrit, ni dans le code, ni dans les
  tickets.
- **Je fusionne** chaque PR et **je lance** chaque commande sur le serveur.
- Les **textes juridiques** rédigés avec l'IA sont des brouillons, à faire valider par un juriste
  avant toute facturation.
- **Images générées** : modèles à licence ouverte seulement, sur ma propre machine, toujours
  signalées ; et **jamais** pour un vrai client, dont les réalisations sont ses photos.
- Les **aides IA** qui seront proposées aux clients de praxora resteront facultatives, mesurées et
  plafonnées (prévues pour la version 1).

## Relire ce que lisent les humains

Un texte écrit avec une IA se reconnaît : « crucial », les énumérations par trois, la phrase-chute
qui conclut chaque paragraphe. Un lecteur qui repère ces tics doute du reste, même quand le fond
est juste. Or mes sites, mes dépliants et les documents que je remets à mes clients sont lus par
des humains qui ne me connaissent pas encore.

Depuis le 11 novembre 2026, tout texte destiné à des humains passe donc **une fois** par une
skill de relecture qui retire ces tics : `humaniseur-fr` pour le français, `humanizer` pour
l'anglais, deux skills libres (licence MIT). Mon cadrage passe avant le leur : registre courant
soigné, vouvoiement, guillemets « », et **aucun fait ajouté, retiré ou modifié**. Une seule
passe : chaque passe supplémentaire aplatit le texte un peu plus.

Trois retouches faites sur [www.praxora.fr](https://www.praxora.fr) :

| Avant                                                                                         | Après                                                                                |
| --------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| Votre site s'exporte en entier, prêt à être installé ailleurs. Vous n'êtes jamais prisonnier. | Votre site s'exporte en entier : vous pouvez l'installer ailleurs quand vous voulez. |
| Le programme part de votre niveau et de vos besoins, pas d'un catalogue tout fait.            | Le programme part de votre niveau et de vos besoins.                                 |
| … une base technique irréprochable …                                                          | … une base technique solide …                                                        |

La phrase-chute disparaît, l'opposition inutile aussi, et le superlatif devient un mot qu'on peut
tenir. Le code, les tickets et la documentation interne ne passent pas par cette relecture : ils
s'adressent à des développeurs, qui ont besoin de précision plus que de style.

## Les erreurs deviennent des règles

L'IA se trompe, comme un collègue. Ce qui compte, c'est qu'une erreur ne se reproduise pas :

- Le 28 octobre 2026, une PR « empilée » sur une autre a été fusionnée dans la mauvaise branche :
  la démo n'est pas arrivée en production. Correction immédiate, puis une règle écrite dans
  `CLAUDE.md` sur la façon de lier deux PR.
- Le même jour, des labels annoncés dans une proposition avaient été oubliés à l'implémentation.
  Je l'ai vu en relisant le site ; Claude a corrigé, puis vérifié les autres démos dans la
  foulée et trouvé le même oubli sur deux d'entre elles.
- Le 11 novembre 2026, la CI avait épuisé son quota mensuel de minutes en onze jours : personne ne
  surveillait sa consommation. Le lendemain : mesure, CI allégée, et une branche `main` que GitHub
  protège lui-même
  ([détail](integration-continue.md#une-ci-plus-sobre)).

Voir aussi : [intégration continue et tests](integration-continue.md),
[gestion de projet sur GitHub](gestion-de-projet-github.md).

## Ce que j'en retire

- L'IA ne remplace pas la conception : elle **rend visible** le manque de conception. Un projet
  sans décisions écrites dérive plus vite avec une IA qu'avec un humain.
- Le meilleur réglage est **la confiance encadrée** : beaucoup d'autonomie dans le cadre, aucune
  sur ce qui est irréversible.
- Tout ce qui est écrit pour l'IA (contexte, décisions, procédures) **sert aussi aux humains** :
  un nouveau développeur trouverait le même chemin.

## En étude, bientôt

- **Les aides IA pour les clients** de praxora (version 1, janvier 2027) : rédaction et
  traduction dans l'administration, débranchables, mesurées et plafonnées par client. Ce sera
  l'IA **dans** le produit, à distinguer de l'IA qui m'aide à le construire, décrite ici.
