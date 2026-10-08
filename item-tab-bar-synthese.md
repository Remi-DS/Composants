# ItemTabBar — Synthèse

Synthèse d'implémentation du composant `ItemTabBar` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Élément rendu comme un bouton avec `role="tab"` et un état sélectionné via `aria-selected`. | IMPLÉMENTÉ |
| Intention design | Aucune règle d'usage détaillée n'est formulée dans les MDX/stories consultés. | NON_CONFIRMÉ |
| Quand l'utiliser | À confirmer dans le Zeroheight de l'univers ; les stories montrent seulement un libellé d'onglet. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Non présent dans les sources techniques. | NON_CONFIRMÉ |
| Cardinalité par page | Non présent dans les sources techniques. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `ItemTabBar`.
- Exports associés : `ItemTabBarProps`.
- Exports publics : `prospect.ts` réexporte `ItemTabBarApollo`, `client.ts` réexporte `ItemTabBarLF`.
- Élément racine rendu et classe CSS de base : `<button type="button" role="tab" class="af-item-tab-bar">`.
- Dépendances internes : `getClassName`.

```tsx
import { ItemTabBar } from "@axa-fr/canopee-react/prospect";

<ItemTabBar title="This is a title" />;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `title` | `string` | Aucun | Texte rendu dans le bouton. | IMPLÉMENTÉ |
| `isActive` | `boolean` | `false` | Définit `aria-selected` et l'état visuel actif. | IMPLÉMENTÉ |
| `className` | via `ComponentPropsWithRef<"button">` | Aucun | Ajouté aux classes calculées. | IMPLÉMENTÉ |
| Props natives | `ComponentPropsWithRef<"button">` | Selon React/HTML | Transmises au bouton via `{...props}`. | IMPLÉMENTÉ |

L'héritage des props natives vient de `ComponentPropsWithRef<"button">`.

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| État actif | `isActive=true` | `aria-selected="true"` puis styles `[aria-selected="true"]` | IMPLÉMENTÉ |
| État inactif | `isActive=false` | `aria-selected="false"` | IMPLÉMENTÉ |

Aucun objet de variantes public n'est exporté pour ce composant.

## États et comportements

- Le composant ne maintient pas d'état interne : l'appelant pilote `isActive`.
- `aria-selected` vaut la valeur de `isActive`.
- Les événements natifs de bouton, dont `onClick`, sont transmis.
- Les styles implémentent les états actif, survol et focus visible.

## Anatomie

```html
<button type="button" role="tab" aria-selected="false|true" class="af-item-tab-bar">
  title
</button>
```

Classes et sélecteurs relevés : `.af-item-tab-bar`, `.af-item-tab-bar:hover`,
`.af-item-tab-bar:focus-visible`, `.af-item-tab-bar[aria-selected="true"]`.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Base commune | `font-size: var(--rem-14)`, `min-height: var(--rem-48)`, `padding: var(--rem-12) var(--rem-8)` | OBSERVÉ |
| Ligne basse | Ombre interne avec `var(--blue-200)`, renforcée en actif avec `var(--blue-1000)` | OBSERVÉ |
| Focus | `outline: 2px solid var(--item-tab-bar-outline-color)`, `outline-offset: 2px` | OBSERVÉ |
| Responsive | `@media (--desktop-small)` : taille 16 px tokenisée, padding `var(--rem-16)`, hauteur minimale `var(--rem-52)` | OBSERVÉ |
| Prospect | Texte inactif en `var(--blue-1000)` | OBSERVÉ |
| Client | Texte inactif en `var(--gray-1000)`, actif en `var(--blue-1000)` | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Implémentation React | Même `ItemTabBarCommon` | Même `ItemTabBarCommon` | IMPLÉMENTÉ |
| Feuille CSS | `ItemTabBarApollo.css` | `ItemTabBarLF.css` | IMPLÉMENTÉ |
| Couleur inactive | `var(--blue-1000)` | `var(--gray-1000)` | OBSERVÉ |
| Titre Storybook | `Components/ItemTabBar` | `Components/TabBar/ItemTabBar` | DOCUMENTÉ |

## Accessibilité

- `role="tab"` est implémenté.
- `aria-selected` reflète `isActive`.
- Le focus visible est stylé en CSS.
- Le bouton natif conserve les interactions clavier natives.
- `aria-controls`, `aria-labelledby`, relation avec une `tablist` et navigation clavier entre onglets ne sont pas implémentés dans ce composant.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : règles d'usage, cardinalité, libellés et relation attendue avec un composant parent de type tablist.
- `NON_CONFIRMÉ` : conformité WCAG/RGAA de l'ensemble d'onglets complet.
- Vérifier dans le Storybook de la version installée : rendu exact des couleurs d'univers et intégration dans une barre d'onglets.
