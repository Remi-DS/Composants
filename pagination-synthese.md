# Pagination — Synthèse

Synthèse d'implémentation du composant `Pagination` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Navigation paginée rendue dans un `<nav>` avec boutons précédent/suivant et items de page. | IMPLÉMENTÉ |
| Quand l'utiliser | Les MDX documentent l'import, l'état contrôlé et l'usage de `asItem`/`hidePrevNext`, sans règle métier. | DOCUMENTÉ |
| Quand ne pas l'utiliser | Non publié ; consulter le Zeroheight Prospect ou Client. | NON_CONFIRMÉ |
| Cardinalité par page | Non publiée ; ne pas déduire de règle de l'API. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `Pagination`.
- Export associé : `PaginationProps`.
- Élément racine rendu : `<nav className="af-pagination">`.
- Dépendances internes : `ItemPagination`, `ClickIcon`, `getItems`, icônes Material `chevron_backward` et `chevron_forward`.

```tsx
import { Pagination } from "@axa-fr/canopee-react/prospect";

<Pagination
  numberPages={10}
  currentPage={1}
  asItem="button"
  onChangePage={(page) => console.log(page)}
/>;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `numberPages` | `number` | `1` dans `PaginationCommon` | Nombre total de pages et longueur de la liste calculée. | IMPLÉMENTÉ |
| `currentPage` | `number` | `1` | Page active ; désactive précédent si `1`, suivant si `numberPages`. | IMPLÉMENTÉ |
| `onChangePage` | `(page: number) => void` | requis par le type helper | Appelé avec `currentPage - 1`, `currentPage + 1` ou une page d'item. | IMPLÉMENTÉ |
| `asItem` | `ElementType` | `undefined` | Remplace l'élément des pages ; `button` reçoit `type="button"`, `a` reçoit un `href`. | IMPLÉMENTÉ |
| `hidePrevNext` | `boolean` | `false` | Ajoute le modificateur `af-pagination--hide-prev-next`; masquage CSS seulement desktop. | IMPLÉMENTÉ |
| `prevButtonProps` / `nextButtonProps` | `Partial<ComponentProps<typeof ClickIcon>>` | `undefined` | Fusionnés dans les `ClickIcon` précédent/suivant. | IMPLÉMENTÉ |
| Props natives nav | `ComponentPropsWithoutRef<"nav">` | selon React | `aria-label` par défaut à `"Pagination"` ; `className` propagée. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Masquage précédent/suivant | `hidePrevNext=true` | `af-pagination--hide-prev-next` | IMPLÉMENTÉ |
| Item courant | `isCurrentPage=true` | `af-item-pagination--current` | IMPLÉMENTÉ |
| Ellipse | page égale à la constante `ELLIPSIS` | `af-item-pagination` sur un `<span>` | IMPLÉMENTÉ |

## États et comportements

- `getItems` affiche toutes les pages si `numberPages <= 7`.
- Au-delà de 7 pages, l'algorithme insère une ou deux ellipses selon la position de `currentPage`.
- Les items sont calculés avec `page`, `isCurrentPage`, `as` et `onClick`.
- `ItemPagination` rend un `<span>` pour la constante `ELLIPSIS` (valeur composée de trois points), sinon `as || "a"`.
- Pour un lien `<a>`, `href` vaut `/${page}` et `aria-current="page"` est posé sur l'item courant.
- Le compteur texte est toujours rendu : `Page ${currentPage} sur ${numberPages}`.

## Anatomie

- `nav.af-pagination` avec `aria-label`.
- `ol.af-pagination-list`.
- Premier `li` : `ClickIcon` précédent, icône `chevron_backward`, `aria-label="Page précédente"`.
- Deuxième `li` : `span.af-pagination-counter`.
- `li` intermédiaires : `ItemPaginationComponent` pour pages et ellipses.
- Dernier `li` : `ClickIcon` suivant, icône `chevron_forward`, `aria-label="Page suivante"`.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Liste | `display: flex`, `padding: var(--rem-8)`, `border: 1px solid var(--pagination-list-border-color)`, `border-radius: 100vmax`, `gap: var(--rem-16)` | OBSERVÉ |
| Mobile par défaut | Les `li` contenant `.af-item-pagination` sont masqués ; le compteur reste visible. | OBSERVÉ |
| `@media (--desktop-small)` | Le compteur est masqué et les items de page deviennent visibles. | OBSERVÉ |
| `hidePrevNext` desktop | Premier et dernier `li` masqués uniquement dans `@media (--desktop-small)`. | DOCUMENTÉ / OBSERVÉ |
| Item | `all: unset`, `display: grid`, `width: 40px`, `height: 40px`, `border-radius: var(--radius-100)`, `font-weight: 600` | OBSERVÉ |
| Focus item | `outline: 2px solid var(--item-pagination-outline-color)` en `:focus-visible` | OBSERVÉ |
| Prospect | Bordure `var(--blue-200)`, fond liste `var(--white-1000)`. | OBSERVÉ |
| Client | Bordure `var(--gray-140)`, fond liste `var(--white-1000)`. | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Import CSS | `PaginationApollo.css` | `PaginationLF.css` | IMPLÉMENTÉ |
| Item utilisé | `ItemPaginationApollo` | `ItemPaginationLF` | IMPLÉMENTÉ |
| Bordure de liste | `var(--blue-200)` | `var(--gray-140)` | OBSERVÉ |
| MDX | Même contenu, import package Prospect. | Même contenu, import package Client. | DOCUMENTÉ |

## Accessibilité

- Le composant rend un `<nav>` avec `aria-label`, par défaut `"Pagination"`.
- Les boutons précédent/suivant ont des `aria-label` explicites et sont désactivés aux extrémités.
- L'item courant en lien reçoit `aria-current="page"` ; en `asItem="button"`, le code ne pose pas `aria-current`.
- Les MDX indiquent que les boutons précédent/suivant restent affichés sur mobile pour la navigation sur petits écrans.
- La conformité WCAG/RGAA n'est pas certifiée par les sources lues.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : règles de placement dans la page, nombre minimal de pages avant affichage, libellés recommandés hors valeurs par défaut.
- `NON_CONFIRMÉ` : comportement attendu si `currentPage` sort de `[1, numberPages]`.
- Vérifier dans le Storybook de la version installée les breakpoints et le rendu `asItem` utilisé par l'application.
