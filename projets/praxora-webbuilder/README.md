# praxoraXwebbuilder

> **In English** — A multi-site platform that builds and manages showcase websites for
> freelancers and small businesses (yoga teachers, restaurants, tradespeople, shops). Sites are
> assembled from a catalogue of typed blocks; clients edit only their content in a restricted
> admin; any site can be exported as a standalone application with its data. One Next.js codebase,
> PostgreSQL with row-level security, self-hosted on a European VPS. Built by one person, with an
> AI used as an architect colleague under explicit rules.

## Le problème

Un professeur de yoga, un restaurant ou un plombier a besoin d'un site **beau, rapide, à jour et
lisible sur téléphone**. Les solutions courantes le laissent face à un choix ingrat : un éditeur
« glisser-déposer » où il passe ses soirées sans résultat professionnel, ou un site sur mesure
qu'il ne sait plus modifier ni récupérer.

## La réponse

Une plateforme où **je compose** le site à partir de blocs éprouvés, et où **le client ne touche
qu'à ses contenus** : textes, photos, horaires, planning, carte du restaurant, actualités.

| Pour le client                                                   | Pour moi                                                |
| ---------------------------------------------------------------- | ------------------------------------------------------- |
| Un site professionnel, mobile d'abord, en français et en anglais | Un site neuf à partir de modèles, sans repartir de zéro |
| Une administration simple, impossible à « casser »               | Un seul code à maintenir pour tous les sites            |
| Ses données lui appartiennent : export complet s'il part         | Des blocs testés une fois, réutilisés partout           |
| Des aides IA facultatives (rédaction, traduction)                | Une IA mesurée et plafonnée par client                  |

## En chiffres (31 octobre 2026)

- **Une seule application** Next.js pour les sites publics, les administrations et le mode
  autonome (l'export).
- **Une trentaine de types de blocs** (héros, planning de cours, carte du restaurant, horaires,
  réalisations avec avant / après, demande de devis, carte géographique…) et des **fiches**
  partagées (personnes, lieux, réalisations).
- **Neuf sites de démonstration** en ligne, sur trois métiers : [voir les démos](demos.md).
- **Près de 70 décisions** écrites, de la pile technique au placement d'une carte ; **environ 400
  tests** automatiques (unitaires, intégration sur PostgreSQL, bout en bout dans un navigateur).

## Pour aller plus loin

| Page                                    | Contenu                                    |
| --------------------------------------- | ------------------------------------------ |
| [Conception](conception.md)             | les idées qui structurent la plateforme    |
| [Choix techniques](choix-techniques.md) | la pile, ce qui a été écarté et pourquoi   |
| [Exemples de code](exemples/)           | extraits commentés                         |
| [Démos](demos.md)                       | les sites en ligne et ce que chacun montre |

## Calendrier

| Étape                                               | Date visée          |
| --------------------------------------------------- | ------------------- |
| Premières démos en ligne                            | octobre 2026 (fait) |
| Premier client en ligne                             | décembre 2026       |
| Version 1 (administration client, export, aides IA) | janvier 2027        |
