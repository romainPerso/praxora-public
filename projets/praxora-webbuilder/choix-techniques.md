# Choix techniques

> **In English** — The stack (TypeScript, Next.js, PostgreSQL, Prisma, Zod, Better Auth behind
> an in-house module, S3-compatible storage, sharp, Docker Compose, Caddy, Vitest, Playwright) and
> the reasoning behind it. The rule: modern but proven tools with a real community and a
> maintenance track record. Each choice lists what was ruled out — for instance Drizzle (still
> pre-1.0) and MinIO (community edition abandoned).

_État au 31 octobre 2026 ; choix validés le 16 octobre 2026 (décision D-009)._

## La règle

**Moderne mais éprouvé.** Un outil entre dans la pile s'il est porté par une vraie communauté et
a un historique de maintenance. Pas d'outil à la mode dont la pérennité est incertaine : les sites
de mes clients doivent tourner dans cinq ans, et leur export aussi. Tout ajout de dépendance
structurante passe par une décision écrite.

## La pile

| Couche            | Choix                                                                               | Écarté, et pourquoi                                                                                 |
| ----------------- | ----------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| Langage           | TypeScript, Node.js LTS                                                             | —                                                                                                   |
| Application       | **Next.js** (App Router), auto-hébergé                                              | Astro + administration séparée : deux applications, contraire au « un seul code »                   |
| Base de données   | **PostgreSQL**                                                                      | —                                                                                                   |
| Accès aux données | **Prisma**                                                                          | Drizzle : encore en 0.x, outil de migration qui peut confondre un renommage avec une suppression    |
| Validation        | **Zod**                                                                             | —                                                                                                   |
| Authentification  | **Better Auth**, appelé seulement via notre module                                  | Keycloak : serveur Java séparé, trop lourd pour le VPS et l'export ; Auth.js : passé en maintenance |
| Médias            | stockage **S3** (OVH en production, SeaweedFS en développement, disque en autonome) | MinIO : version communautaire abandonnée                                                            |
| Images            | **sharp** (libvips)                                                                 | —                                                                                                   |
| E-mails           | Nodemailer + relais SMTP                                                            | —                                                                                                   |
| Conteneurs        | Docker, Docker Compose                                                              | —                                                                                                   |
| HTTPS             | **Caddy** (certificats à la demande)                                                | Traefik, Nginx + Certbot                                                                            |
| Tests             | **Vitest** (unitaires, intégration), **Playwright** (bout en bout)                  | —                                                                                                   |

## Les arguments

### Next.js, une seule application

Une base de code pour les sites publics (le site est choisi selon le nom de domaine reçu), les
administrations et le mode autonome. Rendu côté serveur pour le référencement.

**Vigilance** : les conventions changent entre versions majeures, et certaines fonctions sont
pensées d'abord pour l'hébergeur Vercel. **Parade** : version majeure figée, montées de version
dans des tickets dédiés, tests de bout en bout sur les démos, et aucune fonction qui dépende de
Vercel.

### Prisma plutôt que Drizzle

Ma première proposition était Drizzle ; je l'ai **révisée** en appliquant la règle de maturité.
Prisma a une version stable depuis 2019, des migrations mûres et une très large communauté ; son
cœur est open source, et ses services payants ne sont pas utilisés. Le cloisonnement passe par une
extension du client Prisma, doublée de la sécurité au niveau des lignes de PostgreSQL.

### Better Auth, avec un garde-fou

C'est la référence actuelle pour l'authentification auto-hébergée en TypeScript, mais elle a moins
de deux ans de version stable : **le seul composant qui ne remplit pas pleinement la règle**.
Garde-fou : elle n'est appelée qu'à travers notre propre module d'authentification, et ses tables
sont dans notre base avec des empreintes de mot de passe standard. Si elle déclinait, on la
remplacerait sans toucher au reste ni perdre les comptes.

### SeaweedFS en développement, pas MinIO

MinIO était le choix évident il y a deux ans ; sa version communautaire est aujourd'hui abandonnée.
C'est exactement le risque que la règle veut éviter. Le code ne parle que le protocole S3
standard : le fournisseur reste interchangeable.

### Caddy

Un certificat HTTPS est délivré automatiquement quand un client fait pointer son domaine vers la
plateforme, après vérification auprès de l'application que le domaine est bien connu. Pas de
certificat « joker », qui aurait exigé de confier une clé d'API DNS au serveur web. Caddy est aussi
fourni dans le paquet d'export.

## Ce que je retiens de l'exercice

- Écrire **ce qu'on écarte** est aussi utile que ce qu'on choisit : six mois plus tard, c'est la
  seule trace de pourquoi on n'a pas pris l'outil « évident ».
- Un composant moins éprouvé peut être accepté s'il est **isolé derrière une interface à soi** :
  on achète le droit de changer d'avis.
