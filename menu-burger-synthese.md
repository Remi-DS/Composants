# MenuBurger — Synthèse

Synthèse d'implémentation du composant `MenuBurger` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Menu burger avec bouton déclencheur et panneau contenant des éléments de navigation et du contenu personnalisé. | DOCUMENTÉ |
| Quand l'utiliser | Le MDX le décrit comme un menu de compte contenant navigation et actions personnalisées. | DOCUMENTÉ |
| Quand ne pas l'utiliser | Non présent dans les sources techniques. | NON_CONFIRMÉ |
| Cardinalité par page | Non présent dans les sources techniques. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `MenuBurger`.
- Exports associés publics : `MenuBurgerProps`.
- Exports internes non réexportés par `prospect.ts`/`client.ts` : `menuBurgerVariants`, `MenuBurgerVariants`, `MenuBurgerClickItemProps`.
- Élément racine rendu et classe CSS de base : `<div class="af-menu-burger">`.
- Dépendances internes : `Button`, `Icon`, `List`, `ClickItem`, `useIsSmallScreen(BREAKPOINT.MD)`, `useId`.

```tsx
import { MenuBurger } from "@axa-fr/canopee-react/prospect";

<MenuBurger buttonLabel="Mon compte" clickItems={[{ children: "Profil" }]} />;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `buttonLabel` | `string` | Aucun | Libellé du bouton déclencheur desktop. | IMPLÉMENTÉ |
| `icon` | `IconProps["src"]` | Icône `person` | Icône gauche du bouton. | IMPLÉMENTÉ |
| `variant` | `"primary" \| "secondary"` | `"primary"` | Variante du bouton déclencheur. | IMPLÉMENTÉ |
| `clickItems` | `MenuBurgerClickItemProps[]` | `undefined` | Liste de `ClickItem` rendue dans le panneau. | IMPLÉMENTÉ |
| `children` | `ReactNode` | `undefined` | Contenu additionnel sous la liste. | IMPLÉMENTÉ |
| `className` | `string` | `undefined` | Classe additionnelle sur la racine. | IMPLÉMENTÉ |
| Props natives | `Omit<ComponentPropsWithoutRef<"div">, "children">` | Selon React/HTML | Transmises à la racine. | IMPLÉMENTÉ |

`MenuBurgerClickItemProps` correspond à `Omit<ClickItemProps, "variant"> & { variant?: ClickItemProps["variant"] }`.

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Bouton | `primary` | Classes du `Button` injecté | IMPLÉMENTÉ |
| Bouton | `secondary` | Classes du `Button` injecté | IMPLÉMENTÉ |
| `ClickItem` par défaut | `small` | Classes de `ClickItem` | IMPLÉMENTÉ |
| Icône | `person` par défaut ou `icon` fourni | Classes de `Icon` | IMPLÉMENTÉ |

Objet interne : `menuBurgerVariants = { primary: "primary", secondary: "secondary" }`.

## États et comportements

- Desktop : le bouton est affiché et cible le panneau via l'API native Popover.
- Desktop : `popoverTarget`, `popoverTargetAction="toggle"` et `aria-haspopup="true"` sont posés sur le bouton.
- Desktop : le panneau reçoit `aria-labelledby` pointant vers le bouton.
- Mobile : `useIsSmallScreen(BREAKPOINT.MD)` masque le bouton et rend le panneau directement dans la page, sans attribut `popover`.
- Les `clickItems` ne sont rendus que si le tableau existe et contient au moins un élément.
- Chaque `clickItem` reçoit `variant="small"` par défaut.

## Anatomie

```html
<div class="af-menu-burger">
  <button class="af-menu-burger__button">Mon compte</button>
  <section class="af-menu-burger__panel">
    <div class="af-menu-burger__section af-menu-burger__section--actions">
      <List class="af-menu-burger__click-items">items de navigation</List>
    </div>
    <div class="af-menu-burger__section af-menu-burger__content">contenu complémentaire</div>
  </section>
</div>
```

Classes relevées : `.af-menu-burger`, `__button`, `__button-icon--suffix`, `__panel`,
`__section`, `__section--actions`, `__click-items`, `__content`.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Racine/panneau | Marge supérieure `var(--rem-24)`, padding `var(--rem-16)` | OBSERVÉ |
| Séparation | Bordure supérieure `1px solid var(--blue-200)` | OBSERVÉ |
| Espacement | `var(--rem-12)` entre éléments | OBSERVÉ |
| Rayon | `var(--radius-8)` | OBSERVÉ |
| Positionnement | Décalage supérieur `var(--rem-24)` | OBSERVÉ |
| Transition | `var(--transition-duration)`, animation `overlay` avec `allow-discrete forwards`, transition d'opacité | OBSERVÉ |
| Responsive | Piloté en React par `BREAKPOINT.MD`; pas de media query CSS spécifique relevée pour ce basculement | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Implémentation | `MenuBurgerApollo` | `MenuBurgerLF` | IMPLÉMENTÉ |
| Composants injectés | `Button`, `Icon`, `List`, `ClickItem` Apollo | Équivalents LF | IMPLÉMENTÉ |
| API | `MenuBurgerProps` commune | `MenuBurgerProps` commune | IMPLÉMENTÉ |
| CSS | `MenuBurgerAll.css` | `MenuBurgerAll.css` | OBSERVÉ |
| Différence fonctionnelle | Aucune différence documentée | Aucune différence documentée | NON_CONFIRMÉ |

## Accessibilité

- Bouton natif pour le déclenchement desktop.
- `aria-haspopup="true"` sur le bouton desktop.
- `aria-labelledby` sur le panneau desktop.
- Icônes décoratives avec `alt=""` et `role="presentation"`.
- Identifiants générés avec `useId`.
- Aucun `role="menu"` ou `role="menuitem"` n'est ajouté par `MenuBurgerCommon`.
- Aucun focus initial, retour du focus ou comportement clavier personnalisé n'est visible.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : nombre maximal d'éléments, règles de contenu et usage d'un menu de compte.
- `NON_CONFIRMÉ` : stratégie de focus, fermeture clavier et modèle ARIA attendu.
- `NON_CONFIRMÉ` : comportement si l'API Popover n'est pas disponible.
- `NON_CONFIRMÉ` : conformité WCAG/RGAA.
- Vérifier dans le Storybook de la version installée : comportement mobile/desktop et animation du panneau.
