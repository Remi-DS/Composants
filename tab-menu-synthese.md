# TabMenu — Synthèse

Synthèse d'implémentation du composant `TabMenu` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Navigation de type menu en liste, avec item actif et navigation clavier par flèches. | DOCUMENTÉ / IMPLÉMENTÉ |
| Quand l'utiliser | Le MDX le décrit comme `Horizontal navigation component presented as a list of elements`. Règles design à confirmer. | DOCUMENTÉ / NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Aucune interdiction publiée. | NON_CONFIRMÉ |
| Cardinalité par page | Aucune limite publiée. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `TabMenu`.
- Exports associés : `TabMenuProps` dans les exports publics ; `TabMenuItemProps` documenté dans le MDX mais non exporté publiquement depuis `prospect.ts`/`client.ts`.
- Élément racine rendu et classe CSS de base : `<nav className="af-tab-menu">`.
- Dépendances internes : `ItemMenu`, `getClassName`, `getPosition`.

```tsx
import { TabMenu } from "@axa-fr/canopee-react/prospect";

<TabMenu items={[{ href: "#contracts", label: "Mes contrats", isActive: true }]} />;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `items` | `TabMenuItemProps[]` | `undefined` | Si absent ou vide, le composant retourne `null`. | IMPLÉMENTÉ |
| `items[].label` | `string` | requis par type item | Rendu comme enfant de `ItemMenu`. | IMPLÉMENTÉ |
| Item props | `Omit<ItemMenuProps, "children">` | selon item | Transmises à `ItemMenu`, par exemple `href`, `isActive`. | IMPLÉMENTÉ |
| `initialPosition` | `number` | `0` | Initialise l'index actif/focusable en état local. | IMPLÉMENTÉ / DOCUMENTÉ |
| `className` | `string` | `undefined` | Ajouté à `af-tab-menu` via `getClassName`. | IMPLÉMENTÉ / DOCUMENTÉ |
| Props natives | `Omit<ComponentPropsWithoutRef<"nav">, "children">` | selon HTML | Transmises au `<nav>`. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Aucune variante dédiée | Non exposée | `af-tab-menu` | IMPLÉMENTÉ |
| Actif item | `isActive` ou index courant | classe gérée par `ItemMenu` | IMPLÉMENTÉ |

## États et comportements

- État local `position`, initialisé avec `initialPosition`.
- `ArrowRight` et `ArrowLeft` sont gérés dans le composant ; le helper `getPosition` connaît aussi `ArrowDown` et `ArrowUp`.
- La navigation est cyclique : après le dernier item, retour au premier ; avant le premier, passage au dernier.
- L'élément à la position courante reçoit `isActive=true` et `tabIndex=0`; les autres ont `tabIndex=-1`.
- Si un item non courant a `isActive`, il reste transmis comme fallback via `index === position ? true : item.isActive`.

## Anatomie

- `<nav class="af-tab-menu" tabIndex={0}>`.
- `<ul class="af-tab-menu__list">`.
- Un `<li>` par item.
- Dans chaque `li`, `ItemMenu` avec ref d'ancre, props item, état actif et tabIndex.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Header | `.af-header__tab-menu` : `width:100%`, `padding-bottom:var(--rem-24)`, `padding-inline:var(--rem-16)`. | OBSERVÉ |
| Focus | `.af-tab-menu:focus-visible` : outline `2px solid var(--blue-650)`, offset `3px`. | OBSERVÉ |
| Liste | `display:flex`, `height:100%`, marges/paddings à `0`, `flex-direction:column`, `list-style:none`. | OBSERVÉ |
| Desktop | `@media (--desktop-small)` : nav `height:100%`, `padding:0`; liste en `row`. | OBSERVÉ |

Les valeurs proviennent du CSS source et peuvent évoluer.

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Implémentation | Même `TabMenu.tsx`, import CSS `prospect/TabMenu/TabMenuAll.css`. | Même `TabMenu.tsx`, donc même import CSS prospect dans le fichier source. | IMPLÉMENTÉ |
| Story title | `Components/TabMenu 🚧`. | `Components/TabMenu 🚧`. | DOCUMENTÉ |
| MDX | Même description et mêmes props. | Même description et mêmes props. | DOCUMENTÉ |

## Accessibilité

- Le MDX documente une navigation clavier par flèches et un premier élément focusable par défaut.
- Le `<nav>` est focusable via `tabIndex={0}` et reçoit `onKeyDown`.
- Le code désactive deux règles ESLint a11y autour des interactions sur élément non interactif et du `tabIndex` sur `nav`.
- La conformité WCAG/RGAA n'est pas certifiée.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : différence d'intention entre `TabMenu` et `TabBar`.
- `NON_CONFIRMÉ` : libellés, nombre d'items et présence dans le header.
- Vérifier dans le Storybook de la version installée : comportement réel des flèches verticales, documentées mais non gérées par `handleKeys`.
