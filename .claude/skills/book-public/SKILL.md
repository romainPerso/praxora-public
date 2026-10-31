---
name: book-public
description: Reporter dans le book public (praxora-public) une étape d'un projet privé de praxora — choisir ce qui mérite d'être montré, le réécrire pour un lecteur extérieur, relire la liste de confidentialité, mettre à jour les dates et les sommaires. À utiliser quand un ticket de praxora-public demande un report, quand Romain veut montrer quelque chose, ou quand la fin de session d'un projet privé signale une étape à publier.
---

# Book public

Objectif : le book reflète le travail de Romain **sans retard ni fuite**. Il montre une étape,
réécrite pour être lue par une entreprise ou un recruteur ; il ne recopie jamais un projet privé.

Règles complètes : `CLAUDE.md` de ce dépôt (public visé, langue, confidentialité, organisation).

## 1. Repérer ce qui mérite d'être montré

Bon candidat :

- une **décision** structurante, avec ses raisons et ce qui a été écarté ;
- une **démo** ou une mise en ligne ;
- un **problème résolu** qui montre une façon de penser (constat → options → choix → vérification) ;
- un **extrait de code** court, autonome, non sensible, qui illustre une idée de conception ;
- une évolution de la **méthode** (nouvelle règle, erreur devenue règle).

Pas candidat : le détail d'une correction mineure, ce qui n'est pas encore stable, tout ce qui
tombe dans la liste « Jamais » de `CLAUDE.md`.

## 2. Le ticket

- Un ticket dans **praxora-public** avant le travail (titre et texte **publics** : rien de
  confidentiel, pas de numéro de ticket privé détaillé ; « D-061 du webbuilder » suffit).
- Les projets privés ouvrent ce ticket depuis leur fin de session ; on peut aussi l'ouvrir ici à
  la demande de Romain.

## 3. Écrire

1. **Lire la source** dans le dépôt privé (décision, documentation, code), pas seulement le
   journal.
2. **Choisir la page** : `projets/<projet>/conception.md` ou `choix-techniques.md`, un nouvel
   exemple dans `projets/<projet>/exemples/`, `methode/`, `profil.md`… Une page nouvelle s'ajoute
   au sommaire de son dossier **et** aux deux accueils (`README.md`, `README.en.md`).
3. **Réécrire** pour un lecteur qui ne connaît pas le projet : le problème, le choix, la raison,
   le résultat. Phrases courtes, « je » pour Romain, pas de superlatif ni de chiffre invérifiable.
4. **Résumé anglais** en tête de toute page nouvelle (`> **In English** — …`).
5. **Extrait de code** :
   - court, autonome, simplifié si besoin, avec l'origine (fichier, date) et « simplifié » ;
   - **vérifié** : s'il est présenté comme du code qui tourne (un test, par exemple), le faire
     tourner dans le projet d'origine avant de le publier ; sinon dire qu'il est simplifié ;
   - jamais de code de sécurité tel quel (authentification, cloisonnement, contrôles d'accès) :
     on en décrit le principe.
6. **Le fait et le prévu** : vérifier dans le code (branche `main` du dépôt privé) que ce qui est
   écrit au présent existe. Ce qui est seulement décidé va au futur et dans la section
   **« En étude, bientôt »** en fin de page (« Coming next » en anglais), avec la date visée.
   Quand une étape est livrée, la retirer de cette section et la décrire au présent, sur **toutes**
   les pages qui l'annonçaient (`grep` sur le sujet).
7. **Dates** : mettre à jour « État au … » de chaque page touchée, et les chiffres (nombre de
   décisions, de tests, de démos) seulement s'ils ont été recomptés.
8. **Liens** : vérifier que les démos et pages citées répondent.

## 4. Relire avant la PR

- Relire **tout le diff** avec la liste « Jamais » de `CLAUDE.md` : identifiants, adresses IP,
  ports, chemins, configuration serveur, code de sécurité, noms de clients ou de personnes,
  tarifs non publiés, brouillons juridiques. **Un doute = retirer**, ou demander à Romain.
- Vérifier qu'aucune page ne promet ce qui n'est pas fait (« prévu », « en étude » quand c'est le
  cas).
- `npx --yes prettier@3 --check .` ; la CI refait la mise en forme et la recherche de secrets.

## 5. Livrer

- Une PR par report (ou par lot cohérent), « Closes #N » ; Romain relit et fusionne.
- Dans le message à Romain : ce qui est publié, **les passages à relire en priorité** (tout ce qui
  parle de lui, de clients ou de la collaboration avec l'IA).
- Commenter le ticket : « **Livré** ».
