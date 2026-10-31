# Une carte par lieu

> **In English** — A design review showed that maps differed across demo sites (full interactive
> map, static map only, or no map at all), because each block had been built when one demo needed
> it. The fix: one shared component used by four blocks, one map per location (the product owner's
> choice over a single multi-marker map), and a layout fix for small maps found while checking
> screenshots.

_Origine : `src/blocs/carte-geo/CarteLieu.tsx`, décision D-061, 28 octobre 2026, simplifié._

## Le constat

En relisant les démos, j'ai repéré une incohérence : les restaurants avaient une carte statique
qui devenait interactive au clic, les artisans une carte statique seule, et les démos yoga aucune
carte. Rien ne le justifiait : chaque bloc avait été écrit au moment où une démo en avait besoin.

Deux options pour les sites à plusieurs lieux : une carte commune avec un repère par lieu, ou une
carte par lieu. **Une carte par lieu** l'a emporté : une carte commune oblige à dézoomer, demande
un système pour viser un lieu, et sépare chaque description de son repère.

## Le composant

Un seul composant, utilisé par les blocs Carte, Zone d'intervention, Lieux et Tournée.

```tsx
/**
 * Carte d'un lieu, la même partout où un lieu est montré (D-061) : la carte statique de la fiche,
 * remplacée au clic par la carte interactive (D-052), et l'attribution.
 */
export function CarteLieu({
  lieu,
  contexte,
  interactive,
  sizes,
  className,
  toujours = false,
}: {
  lieu: {
    nom: string;
    carte?: { media: string; alt: ValeurTraduisible }; // image dessinée une fois, exportée avec le site
    position?: { lat: number; lon: number };
  };
  contexte: ContexteRendu;
  interactive: boolean;
  sizes: string;
  className?: string;
  /** Afficher le cadre même sans carte ni position (bloc Carte, dont c'est l'objet même). */
  toujours?: boolean;
}) {
  const { langue } = contexte;
  if (!toujours && !lieu.carte && !lieu.position) return null;

  const statique = lieu.carte ? (
    <ImageMedia image={lieu.carte} contexte={contexte} className={styles.image} sizes={sizes} />
  ) : (
    <div className={`${commun.imageAbsente} ${styles.image}`}>{TEXTES[langue].carteAbsente}</div>
  );

  return (
    <div className={className}>
      {interactive && lieu.position ? (
        // Rien n'est demandé au fournisseur de tuiles avant le clic du visiteur.
        <CarteInteractive position={lieu.position} langue={langue} nom={lieu.nom}>
          {statique}
        </CarteInteractive>
      ) : (
        <div className={styles.cadre}>{statique}</div>
      )}
      <Attribution />
    </div>
  );
}
```

Dans le bloc « Lieux », chaque lieu reçoit sa carte, derrière un nouveau **réglage** activé par
défaut : pas de nouvelle version du bloc, les sites existants en profitent tels quels
([pourquoi](faire-evoluer-un-bloc.md)).

```tsx
{
  reglages.carte && (
    <CarteLieu
      lieu={f}
      contexte={contexte}
      interactive
      sizes="(min-width: 48rem) 45vw, 100vw"
      className={styles.carte}
    />
  );
}
```

## Ce que les captures ont montré

Une fois les cartes placées dans des colonnes de quatre, le message d'information (« la carte
interactive est fournie par OpenFreeMap… ») recouvrait presque toute la carte. Deux corrections :

- le message passe **sous** la carte ; seul le bouton reste dessus ;
- dans un cadre étroit, le bouton se compacte grâce à une **requête de conteneur** : c'est la
  largeur de la carte qui compte, pas celle de l'écran.

```css
.cadre {
  position: relative;
  container-type: inline-size;
  aspect-ratio: 4 / 3;
}

/* Petite carte (une carte par lieu, dans une colonne étroite) : bouton plus compact. */
@container (max-width: 18rem) {
  .boutonCarte {
    padding: 0.375rem 0.625rem;
    font-size: 0.75rem;
    white-space: nowrap;
  }
}
```

## Les tests

```ts
it("lieux : une carte par lieu qui a une position ou une carte, aucune sinon", () => {
  const h = html(blocLieux({})); // trois lieux, dont un sans position ni carte
  expect(compter(h, "Afficher la carte interactive")).toBe(2);
  expect(compter(h, "openstreetmap.org/copyright")).toBe(2);
  expect(html(blocLieux({ carte: false }))).not.toContain("carte interactive");
});
```

Résultat : la règle est écrite dans la documentation des blocs, pour que les prochaines démos ne
recréent pas l'écart.
