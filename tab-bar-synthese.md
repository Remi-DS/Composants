# TabBar — Synthèse

Synthèse d'implémentation du composant `TabBar` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Afficher une liste d'onglets et les panneaux associés, avec sélection interne. | IMPLÉMENTÉ |
| Quand l'utiliser | Le MDX montre des contenus par onglet, pré-sélection, alignement et callback. Règles design à confirmer. | DOCUMENTÉ / NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Aucune contre-indication publiée. | NON_CONFIRMÉ |
| Cardinalité par page | Aucune limite publiée ; un test vérifie seulement l'unicité d'IDs entre plusieurs instances. | IMPLÉMENTÉ / NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `TabBar`.
- Exports associés : `tabBarDirection`, `TabBarDirection`, `TabBarProps`.
- Élément racine rendu et classe CSS de base : `<div className="af-tabbar">`.
- Dépendances internes : `ItemTabBar` Apollo ou LF, `classnames`, hooks React.

```tsx
import { TabBar, tabBarDirection } from "@axa-fr/canopee-react/prospect";

<TabBar items={[{ title: "Tab 1", content: "Content 1" }]} direction={tabBarDirection.center} />;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `items` | `({ content: ReactNode; handleSelectTab?: () => void } & Omit<ItemTabBarProps, "content">)[]` | requis | Alimente les tabs et panels ; `title` sert aussi aux clés React. | IMPLÉMENTÉ |
| `items[].content` | `ReactNode` | requis | Rendu dans le `tabpanel` correspondant. | IMPLÉMENTÉ |
| `items[].handleSelectTab` | `() => void` | `undefined` | Appelé au clic et à la navigation clavier qui sélectionne l'onglet. | IMPLÉMENTÉ |
| `preSelectedTabIndex` | `number` | `0` via `preSelectedTabIndex || 0` | Initialise l'onglet sélectionné ; une valeur falsy revient à `0`. | IMPLÉMENTÉ |
| `direction` | `"center" \| "left"` | `"left"` | Ajoute `af-tabbar--center` si `center`. | IMPLÉMENTÉ |

Aucune prop native de wrapper n'est exposée dans `TabBarProps`.

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Alignement | `left` | `af-tabbar` | IMPLÉMENTÉ |
| Alignement | `center` | `af-tabbar af-tabbar--center` | IMPLÉMENTÉ |

## États et comportements

- `selectedTabIndex` est géré en local par `useState`.
- Clic sur un onglet : met à jour l'index sélectionné et appelle `handleSelectTab`.
- Clavier : `ArrowRight`, `ArrowLeft`, `Home`, `End` sélectionnent et focalisent l'onglet cible.
- Les flèches bouclent entre premier et dernier onglet.
- Les panels inactifs restent dans le DOM avec `aria-hidden="true"` et sont masqués en CSS.

## Anatomie

- `div.af-tabbar` éventuellement `af-tabbar--center`.
- `div[role="tablist"]` contenant les `ItemTabBarComponent`.
- Chaque tab reçoit `id`, `aria-selected`, `aria-controls`, `tabIndex`, `onKeyDown`, `onClick`.
- Chaque panel est un `div[role="tabpanel"]` avec `id`, `aria-labelledby`, `aria-hidden`.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Commun | Wrapper en `display:flex`, `flex-direction:column`. | OBSERVÉ |
| Commun | `tablist` en `display:flex`, `width:100%`, `box-shadow: inset 0 -1px 0 0 var(--tabbar-box-shadow-color)`. | OBSERVÉ |
| Commun | Panel avec `aria-hidden="true"` masqué par `display:none`. | OBSERVÉ |
| Center | `justify-content:center`; tabs avec `flex-grow:1`. | OBSERVÉ |
| Prospect/Client | `--tabbar-box-shadow-color: var(--blue-200)`. | OBSERVÉ |

Aucune media query dédiée n'a été relevée.

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Sous-composant tab | `ItemTabBarApollo`. | `ItemTabBarLF`. | IMPLÉMENTÉ |
| CSS | `TabBarApollo.css`. | `TabBarLF.css`. | IMPLÉMENTÉ |
| Token ombre | `var(--blue-200)`. | `var(--blue-200)`. | OBSERVÉ |

## Accessibilité

- Rôles `tablist`, `tab`, `tabpanel` implémentés via `TabBar` et `ItemTabBar`.
- Liens ARIA entre tabs et panels via `aria-controls` / `aria-labelledby`.
- Navigation clavier flèches, `Home`, `End` implémentée.
- Un test `jest-axe` vérifie l'absence de violations dans un cas de base ; cela ne certifie pas WCAG/RGAA.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : nombre recommandé d'onglets et règles de libellés.
- `NON_CONFIRMÉ` : quand choisir `TabBar` plutôt que `TabMenu`.
- Vérifier dans le Storybook de la version installée : rendu exact de `ItemTabBar` par univers.
