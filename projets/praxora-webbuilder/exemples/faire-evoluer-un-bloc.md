# Faire évoluer un bloc

> **In English** — When a block's shape changes, it gets a new version and a pure, tested
> migration function. Stored configurations (drafts, published versions, templates) are migrated
> at startup before any visitor is served, and on read as a safety net. Adding an optional field
> or a setting with a default needs no new version.

_Origine : `src/blocs/heros/definition.ts`, `docs/blocs.md` et les tests unitaires, octobre 2026,
simplifiés._

## La règle

| Changement                                                            | Nouvelle version ?          |
| --------------------------------------------------------------------- | --------------------------- |
| Ajouter un champ **facultatif**, ou un réglage avec valeur par défaut | non                         |
| Renommer, supprimer, changer de type, rendre obligatoire              | **oui**, avec une migration |

Une migration est une **fonction pure** : elle reçoit les données de la version N-1 et rend
celles de la version N. Les configurations enregistrées sont mises à jour **au démarrage de
l'application, avant qu'elle accepte des visiteurs** ; toute configuration lue est aussi mise à jour
en mémoire si besoin. Aucun visiteur ne voit une page à qui il manquerait un bloc.

## Un cas réel : « accroche » devient « sousTitre »

Le bloc « Héros » (le grand visuel d'ouverture) avait un champ `accroche`. Pour aligner le
vocabulaire avec les autres blocs, il est devenu `sousTitre` : version 2.

```ts
export const heros = definirBloc({
  type: "heros",
  version: 2,
  // … variantes, réglages, contenus (avec sousTitre) …
  migrations: {
    // Version 2 : le champ « accroche » est renommé « sousTitre ».
    2: ({ variante, reglages, contenus }) => {
      const { accroche, ...reste } = contenus;
      return {
        variante,
        reglages,
        contenus: accroche === undefined ? reste : { ...reste, sousTitre: accroche },
      };
    },
  },
});
```

Sans la migration, `definirBloc` refuse de démarrer ([voir pourquoi](declarer-un-bloc.md)).

## Un cas réel sans version : une carte par lieu

Le 28 octobre 2026, les blocs « Lieux » et « Tournée » ont reçu un réglage `carte` (afficher la
carte de chaque lieu), activé par défaut. Réglage avec valeur par défaut : **pas de nouvelle
version**, les sites existants l'ont reçu tels quels ([l'exemple](une-carte-par-lieu.md)).

## Le test

Les migrations se testent comme n'importe quelle fonction : on part d'une configuration
enregistrée à l'ancienne version, on vérifie le résultat.

```ts
import { catalogueGenerique } from "@/blocs/catalogue";
import { migrerStructure, type Structure } from "@/configuration/structure";

it("migre un héros de la version 1 : « accroche » devient « sousTitre »", () => {
  const ancienne: Structure = {
    zones: {},
    pages: [
      {
        id: "p-accueil",
        slug: "",
        titre: { fr: "Accueil" },
        blocs: [
          {
            id: "b-heros",
            type: "heros",
            version: 1,
            variante: "image-fond",
            reglages: {},
            contenus: {
              titre: { fr: "Bienvenue" },
              accroche: { fr: "Cours de yoga à Lyon" },
              image: { media: "media:exemple", alt: { fr: "Une salle de yoga" } },
            },
          },
        ],
      },
    ],
  };

  const migree = migrerStructure(catalogueGenerique, ancienne);

  const heros = migree!.pages[0]!.blocs[0]!;
  expect(heros.version).toBe(2);
  expect(heros.contenus).toEqual({
    titre: { fr: "Bienvenue" },
    sousTitre: { fr: "Cours de yoga à Lyon" },
    image: { media: "media:exemple", alt: { fr: "Une salle de yoga" } },
  });
  // Déjà à jour : rien à réécrire, et on le sait (null).
  expect(migrerStructure(catalogueGenerique, migree!)).toBeNull();
});
```

_Test écrit pour cette page sur le modèle de ceux du projet, et vérifié dans le projet le
31 octobre 2026._
