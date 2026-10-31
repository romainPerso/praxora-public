# Déclarer un bloc

> **In English** — A block type is declared once: fields marked as _settings_ (for me) or
> _content_ (for the client), layout variants with their mobile behaviour, sample content and a
> version. `definirBloc` checks the declaration as soon as the module loads, so an inconsistent
> block cannot even start.

_Origine : `src/blocs/tarifs/definition.ts` et `src/blocs/noyau/definir.ts`, octobre 2026,
simplifiés._

## Le bloc « Tarifs »

Utilisé par les démos yoga (cours, cartes, abonnements) et par les artisans (contrats
d'entretien, tarifs de dépannage).

```ts
import { champ, optionnel } from "../noyau/champs";
import { definirBloc } from "../noyau/definir";

export const tarifs = definirBloc({
  type: "tarifs",
  version: 1,
  libelle: "Tarifs",
  description: "Formules et prix : séance, carte, abonnement, prestation…",
  emplacements: ["page"],

  // Mises en page prédéfinies, chacune décrite d'abord pour le mobile (D-017).
  variantes: {
    cartes: {
      libelle: "Cartes",
      mobile: "Cartes empilées ; côte à côte à partir des écrans moyens.",
    },
    tableau: {
      libelle: "Tableau",
      mobile: "Chaque ligne devient un petit bloc : pas de défilement horizontal.",
    },
  },

  // Réglages : réservés à l'administrateur de la plateforme. Ici, aucun.
  reglages: {},

  // Contenus : ce que le client modifie lui-même, avec leurs limites.
  contenus: {
    titre: champ.texte({ libelle: "Titre", max: 90 }),
    introduction: optionnel(champ.texte({ libelle: "Introduction", max: 400 })),
    formules: champ.liste(
      {
        nom: champ.texte({ libelle: "Formule", max: 60 }),
        // Le prix est un texte non traduit : « 15 € », « sur devis »…
        prix: champ.texteNonTraduit({ libelle: "Prix (ex. 15 €)", max: 30 }),
        unite: optionnel(champ.texte({ libelle: "Par (séance, mois…)", max: 40 })),
        detail: optionnel(champ.texte({ libelle: "Détail", max: 200 })),
      },
      { libelle: "Formules", min: 1, max: 12 },
    ),
    remarque: optionnel(champ.texte({ libelle: "Remarque sous les tarifs", max: 300 })),
  },

  // Contenus d'exemple : l'aperçu les affiche, et ils remplacent ceux du client
  // quand on enregistre un site comme modèle (rien ne fuit d'un client à l'autre).
  exemple: {
    titre: { fr: "Tarifs", en: "Prices" },
    formules: [
      { nom: { fr: "Cours à l'unité", en: "Single class" }, prix: "15 €" },
      { nom: { fr: "Carte 10 cours", en: "10-class pass" }, prix: "130 €" },
    ],
  },
});
```

`champ.texte` produit un texte **traduisible** (`{ fr, en }`) ; `champ.texteNonTraduit` un texte
unique. Chaque champ porte sa **nature** (réglage ou contenu) : c'est elle qui décide qui peut le
modifier.

## Ce que vérifie `definirBloc`

La déclaration est contrôlée **au chargement du module** : un bloc incohérent fait échouer le
démarrage et les tests, il n'arrive jamais en production.

```ts
export function definirBloc(parametres) {
  const { type, version } = parametres;
  if (!/^[a-z][a-z0-9-]*$/.test(type))
    throw new Error(`Type de bloc « ${type} » : identifiant en minuscules et tirets attendu.`);
  if (Object.keys(parametres.variantes).length === 0)
    throw new Error(`Bloc « ${type} » : au moins une variante est obligatoire.`);

  // Un réglage déclaré parmi les contenus (ou l'inverse) donnerait au client un droit de trop.
  for (const champ of Object.values(parametres.reglages))
    if (champ.nature !== "reglage")
      throw new Error(`Bloc « ${type} » : « ${champ.libelle} » n'est pas un réglage.`);

  // Une version N exige les migrations 2 à N : impossible d'oublier d'en écrire une.
  const migrations = parametres.migrations ?? {};
  for (let v = 2; v <= version; v++)
    if (!migrations[v])
      throw new Error(`Bloc « ${type} » : migration vers la version ${v} manquante.`);

  // Les schémas Zod sont déduits des champs ; les contenus d'exemple doivent les respecter.
  const schemaReglages = schemaDesChamps(parametres.reglages);
  const schemaContenus = schemaDesChamps(parametres.contenus);
  const exemple = schemaContenus.safeParse(parametres.exemple);
  if (!exemple.success)
    throw new Error(
      `Bloc « ${type} » : contenus d'exemple invalides.\n${z.prettifyError(exemple.error)}`,
    );

  return { ...parametres, migrations, schemaReglages, schemaContenus };
}
```

## Ce qui s'en déduit

| À partir de la déclaration   | On obtient                                                               |
| ---------------------------- | ------------------------------------------------------------------------ |
| les champs et leurs limites  | la validation de chaque configuration enregistrée (Zod)                  |
| la nature de chaque champ    | ce que le client a le droit de modifier                                  |
| les variantes                | les composants d'affichage, un par variante                              |
| les contenus d'exemple       | l'aperçu, et des modèles sans données de client                          |
| la version et les migrations | la mise à jour des sites existants ([exemple](faire-evoluer-un-bloc.md)) |

Ajouter un bloc au catalogue, c'est écrire cette déclaration et un composant par variante :
l'administration, la validation et les droits suivent sans autre code.
