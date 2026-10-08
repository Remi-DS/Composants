# SkeletonGrid — Synthèse

Synthèse d'implémentation du composant `SkeletonGrid` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Grille responsive de placeholders `Skeleton`, avec mode wrapper qui rend les enfants quand le chargement est terminé. | IMPLÉMENTÉ |
| Quand l'utiliser | Les MDX documentent une grille de skeletons et un mode wrapper, sans règle de contexte produit. | DOCUMENTÉ |
| Quand ne pas l'utiliser | Non publié ; consulter le Zeroheight de l'univers concerné. | NON_CONFIRMÉ |
| Cardinalité par page | Non publiée ; les exemples de lignes/colonnes ne sont pas des limites design. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `SkeletonGrid`.
- Export associé : `SkeletonGridProps`.
- Élément racine rendu en chargement : `<div className="af-skeleton-grid" role="status">`.
- Dépendances internes : `Skeleton`, `getClassName`, `SkeletonProps`.

```tsx
import { SkeletonGrid } from "@axa-fr/canopee-react/prospect";

<SkeletonGrid grid={[[{ colSize: 12 }], [{ colSize: 6 }]]} />;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `grid` | `SkeletonGridItem[][]` | `[]` | Tableau à deux niveaux : lignes puis cellules `SkeletonProps`. | IMPLÉMENTÉ |
| `isLoading` | `boolean` | `true` | Si `true`, rend la grille ; si `false`, retourne `children`. | IMPLÉMENTÉ |
| `children` | `ReactNode` si `isLoading` fourni | `undefined` | Contenu de remplacement en mode wrapper. | IMPLÉMENTÉ |
| `className` | `string` | `undefined` | Fusionné avec `af-skeleton-grid`. | IMPLÉMENTÉ |
| `aria-busy` | `boolean` | `true` | Posé sur la grille en chargement. | IMPLÉMENTÉ |
| `aria-label` | `string` | `"Chargement"` | Libellé accessible du `role="status"`. | IMPLÉMENTÉ |
| `maxCols` | `number` | `12` | Définit `--max-cols` et le nombre de colonnes CSS. | IMPLÉMENTÉ |
| `colGap` | `number` | `16` | Converti en rem via `calc(${colGap} / var(--font-size-base) * 1rem)`. | IMPLÉMENTÉ |
| `rowGap` | `number` | `8` | Converti en rem via `calc(${rowGap} / var(--font-size-base) * 1rem)`. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Mode chargement | `isLoading=true` | `af-skeleton-grid` | IMPLÉMENTÉ |
| Mode wrapper terminé | `isLoading=false` | aucune grille rendue | IMPLÉMENTÉ |
| Variantes des cellules | Hérite de `SkeletonProps` (`rectangle`, `circle`, `action`) | classes de modificateur `af-skeleton--rectangle`, `af-skeleton--circle`, `af-skeleton--action` | IMPLÉMENTÉ |

## États et comportements

- En chargement, le composant mappe toutes les cellules de `grid` et rend un `Skeleton` par cellule.
- Quand `isLoading` vaut `false`, la fonction retourne directement `children`, sans wrapper.
- Les clés des cellules sont construites avec `${indexRow}-${indexCol}`.
- Les MDX documentent `colSize: 12` pour une ligne pleine largeur avec `maxCols={12}`.
- Les stories montrent `Default`, `Mixed`, `MaxColumns`, `ColumnGap`, `RowGapSpacing` et `WrapperMode`.

## Anatomie

- `div.af-skeleton-grid[role="status"][aria-busy][aria-label]`.
- Styles inline : `--max-cols`, `--col-gap`, `--row-gap`.
- Enfants : plusieurs `Skeleton` sans wrapper par ligne ; la notion de ligne vient de l'ordre du tableau et des spans CSS.
- En mode non chargé : uniquement `children`.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Base | `--max-cols: 12`, `--col-gap: 1rem`, `--row-gap: 0.5rem` | OBSERVÉ |
| Grille | `display: grid`, `width: 100%`, `grid-template-columns: repeat(var(--max-cols), 1fr)` | OBSERVÉ |
| Espacements | `gap: var(--row-gap) var(--col-gap)` | OBSERVÉ |
| Colonne cellule | Via `Skeleton` : `grid-column: span var(--col-size)` | OBSERVÉ |
| Ligne cellule | Via `Skeleton` : `grid-row: span var(--row-size)` | OBSERVÉ |

Aucune media query n'a été relevée dans `SkeletonGridAll.css`.

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Export | Même fichier React `SkeletonGrid.tsx`. | Même fichier React `SkeletonGrid.tsx`. | IMPLÉMENTÉ |
| Import CSS dans React | `@axa-fr/canopee-css/prospect/SkeletonGrid/SkeletonGridAll.css` | Même fichier React, import prospect dans la source commune. | IMPLÉMENTÉ |
| CSS | `SkeletonGridAll.css`, valeurs communes. | `SkeletonGridAll.css`, valeurs communes. | OBSERVÉ |
| Stories | Même exemples, imports adaptés. | Même exemples, imports adaptés. | DOCUMENTÉ |

## Accessibilité

- En chargement, la grille reçoit `role="status"`, `aria-busy` et `aria-label`.
- Le libellé par défaut est `"Chargement"`.
- Les skeletons internes n'ont pas d'attribut ARIA propre.
- En mode `isLoading=false`, aucun wrapper accessible n'est conservé.
- La conformité WCAG/RGAA n'est pas certifiée par les sources.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : règles de choix des formes de skeleton, nombre de lignes, durée de chargement et transition vers le contenu.
- `NON_CONFIRMÉ` : stratégie d'annonce accessible quand le contenu chargé remplace la grille.
- Vérifier dans le Storybook de la version installée les espacements, le mode wrapper et les exemples avec `maxCols`.
