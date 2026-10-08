# Skeleton — Synthèse

Synthèse d'implémentation du composant `Skeleton` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Bloc placeholder visuel, base utilisée par `SkeletonGrid`, `SkeletonList` et `ExitLayoutSkeleton` selon les commentaires JSDoc. | IMPLÉMENTÉ |
| Quand l'utiliser | Les MDX documentent l'import, `colSize`, `rowSize` et des exemples de variantes, sans règle de chargement produit. | DOCUMENTÉ |
| Quand ne pas l'utiliser | Non publié ; consulter le Zeroheight de l'univers concerné. | NON_CONFIRMÉ |
| Cardinalité par page | Non publiée dans les sources lues. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `Skeleton`.
- Exports associés : `skeletonVariants`, `skeletonSizeVariants`, `SkeletonProps`, `SkeletonVariant`, `SkeletonSizeVariant`, `SkeletonCircleSizeVariant`, `SkeletonActionSizeVariant`.
- Élément racine rendu : `<div>` avec classe de base `af-skeleton` et modificateurs de variante/taille.
- Dépendances internes : `getClassName`.

```tsx
import { Skeleton } from "@axa-fr/canopee-react/prospect";

<Skeleton variant="rectangle" size="M" colSize={12} rowSize={1} />;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `variant` | `"rectangle" \| "circle" \| "action"` | `"rectangle"` | Ajoute `af-skeleton--rectangle`, `--circle` ou `--action`. | IMPLÉMENTÉ |
| `size` | dépend de `variant` | `"M"` | Mappe vers les classes de taille de `skeletonSizeVariants`. | IMPLÉMENTÉ |
| `colSize` | `number` | `1` | Définit la variable inline `--col-size`. | IMPLÉMENTÉ |
| `rowSize` | `number` | `1` | Définit la variable inline `--row-size`. | IMPLÉMENTÉ |
| `className` | `string` | `undefined` | Fusionné avec `af-skeleton`. | IMPLÉMENTÉ |
| `style` | `CSSProperties` | `undefined` | Fusionné après `--col-size` et `--row-size`, peut donc les surcharger. | IMPLÉMENTÉ |
| Props natives div | `ComponentPropsWithoutRef<"div">` | selon React | Propagées au `<div>`. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Rectangle | `rectangle` | `af-skeleton--rectangle` | IMPLÉMENTÉ |
| Cercle | `circle` | `af-skeleton--circle` | IMPLÉMENTÉ |
| Action | `action` | `af-skeleton--action` | IMPLÉMENTÉ |
| Tailles | `XS`, `S`, `M`, `L`, `XL`, `XXL` | `extra-small`, `small`, `medium`, `large`, `extra-large`, `extra-extra-large` | IMPLÉMENTÉ |

Types restrictifs : `circle` accepte `S | M | L`, `action` accepte `M`, `rectangle` accepte `XS | S | M | L | XL | XXL`.

## États et comportements

- Le composant est purement présentational et ne gère pas d'état de chargement.
- Les stories montrent `Circle`, `Action`, `Rectangle` et un `Playground`.
- L'animation CSS `pulse` n'est active que sous `@media (prefers-reduced-motion: no-preference)`.
- La largeur de grille vient de `grid-column: span var(--col-size)` et `grid-row: span var(--row-size)`.
- Aucune valeur ARIA par défaut n'est posée par `Skeleton`.

## Anatomie

- Un seul `div.af-skeleton`.
- Modificateurs : variante et taille.
- Styles inline : `--col-size`, `--row-size`, puis `style` fourni.
- Le visuel est produit par un `linear-gradient` de fond et l'animation CSS.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Base | `--skeleton-height: 56px`, `--skeleton-width: 100%`, `--skeleton-radius: var(--radius-8)` | OBSERVÉ |
| Couleurs | `--skeleton-color-odd: var(--gray-140)`, `--skeleton-color-even: var(--gray-050)` | OBSERVÉ |
| Fond | `linear-gradient(-45deg, odd 40%, even 50%, odd 60%)`, `background-size: 400% 400%` | OBSERVÉ |
| Action / cercle | `--skeleton-radius: var(--radius-100)` | OBSERVÉ |
| Cercle | `--skeleton-width: var(--skeleton-height)`, `aspect-ratio: 1 / 1` | OBSERVÉ |
| Tailles rectangle | `XS 24px`, `S 40px`, `M 56px`, `L 80px`, `XL 150px`, `XXL 400px` | OBSERVÉ |
| Tailles cercle | `S 24px`, `M 48px`, `L 64px` | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Export | Même fichier React `Skeleton.tsx`. | Même fichier React `Skeleton.tsx`. | IMPLÉMENTÉ |
| Import CSS dans React | `@axa-fr/canopee-css/client/Skeleton/SkeletonAll.css` dans le fichier commun. | Identique, malgré l'export Prospect. | IMPLÉMENTÉ |
| CSS | `SkeletonAll.css`, valeurs communes. | `SkeletonAll.css`, valeurs communes. | OBSERVÉ |
| Stories | Même contenu, imports adaptés. | Même contenu, imports adaptés. | DOCUMENTÉ |

## Accessibilité

- `Skeleton` ne rend pas de rôle, `aria-busy` ou libellé par défaut.
- L'animation respecte `prefers-reduced-motion: no-preference`.
- Pour une zone de chargement annoncée, les sources montrent plutôt `SkeletonGrid` avec `role="status"` et `aria-busy`.
- La conformité WCAG/RGAA n'est pas certifiée.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : contextes où préférer skeleton, spinner ou contenu vide ; durée maximale d'affichage.
- `NON_CONFIRMÉ` : règles de composition visuelle et nombre de placeholders par écran.
- Vérifier dans le Storybook de la version installée les tailles et l'animation.
