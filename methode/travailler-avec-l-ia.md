# Travailler avec l'IA

> **In English** — How I work with an AI (Claude) on praxora: it acts as a technical architect
> who proposes, implements and tests, while I decide, review and merge. The collaboration is
> framed by written context (a CLAUDE.md per repository), a ticket for every piece of work,
> written decisions, a session journal, reusable procedures ("skills") and hard rules (no
> secrets, no AI-generated images for real clients, legal texts reviewed by a lawyer). Mistakes
> become rules.

_État au 31 octobre 2026._

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
| `docs/decisions.md`        | chaque choix structurant, avec ses raisons et ce qui a été écarté (près de 70 à ce jour)                                                       |
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
- Les **aides IA** proposées aux clients de praxora restent facultatives, mesurées et plafonnées.

## Les erreurs deviennent des règles

L'IA se trompe, comme un collègue. Ce qui compte, c'est qu'une erreur ne se reproduise pas :

- Le 28 octobre 2026, une PR « empilée » sur une autre a été fusionnée dans la mauvaise branche :
  la démo n'est pas arrivée en production. Correction immédiate, puis une règle écrite dans
  `CLAUDE.md` sur la façon de lier deux PR.
- Le même jour, des labels annoncés dans une proposition avaient été oubliés à l'implémentation.
  Je l'ai vu en relisant le site ; Claude a corrigé, puis vérifié les autres démos dans la
  foulée et trouvé le même oubli sur deux d'entre elles.

## Ce que j'en retire

- L'IA ne remplace pas la conception : elle **rend visible** le manque de conception. Un projet
  sans décisions écrites dérive plus vite avec une IA qu'avec un humain.
- Le meilleur réglage est **la confiance encadrée** : beaucoup d'autonomie dans le cadre, aucune
  sur ce qui est irréversible.
- Tout ce qui est écrit pour l'IA (contexte, décisions, procédures) **sert aussi aux humains** :
  un nouveau développeur trouverait le même chemin.
