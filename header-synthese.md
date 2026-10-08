# Header — Synthèse

Synthèse d'implémentation du composant `Header` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | La MDX décrit un en-tête responsive composé de `AppName`, d'un `TabMenu` optionnel, d'un `MenuBurger` optionnel et d'enfants d'action optionnels. | DOCUMENTÉ |
| Quand l'utiliser | Non documenté comme règle d'usage ; vérifier le Zeroheight Prospect ou Client. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Non documenté dans les sources techniques lues. | NON_CONFIRMÉ |
| Cardinalité par page | Non documentée ; le rendu `<header>` ne suffit pas à établir une règle design. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `Header` ; export associé : `HeaderProps`.
- Élément racine rendu et classe CSS de base : `<header className="af-header">`, avec les attributs natifs restants transmis au `header`.
- Props natives : `HeaderProps = Omit<ComponentPropsWithoutRef<"header">, "title">` complété par les props spécifiques listées dans la section Propriétés.
- Dépendances internes communes : `TabMenu`, `ClickIcon`, `AppName`, `Heading`, `MenuBurger`, `getClassName`, `useIsSmallScreen(BREAKPOINT.MD)`.
- Prospect injecte `HeadingApollo`, `MenuBurgerApollo`, `ClickIconApollo`; Client injecte `HeadingLF`, `MenuBurgerLF`, `ClickIconLF`.

```tsx
import { Header } from "@axa-fr/canopee-react/prospect";

<Header
  appNameProps={{ label: "Mon application", logoLinkProps: { href: "/" } }}
  tabMenuProps={tabMenuProps}
  menuBurgerProps={menuBurgerProps}
/>;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `appNameProps` | `AppNameProps` | — | Obligatoire ; enrichi avec la classe `af-header__app-name` et transmis à `AppName`. | IMPLÉMENTÉ / DOCUMENTÉ |
| `menuBurgerProps` | `MenuBurgerProps` | `undefined` | Si présent, enrichi avec `af-header__menu-burger` puis rendu dans `.af-header__actions`. | IMPLÉMENTÉ |
| `tabMenuProps` | `TabMenuProps` | `undefined` | Si présent, enrichi avec `af-header__tab-menu` puis transmis à `TabMenu`. | IMPLÉMENTÉ |
| `clickIconProps` | `ClickIconProps` | `undefined` | Étendu sur le déclencheur menu ; `aria-label` vaut celui fourni ou `Ouvrir le menu`. | IMPLÉMENTÉ |
| `title` | `string` | `undefined` | Rendu via `Heading level={1}` dans les actions ; la condition masque le titre seul en mobile. | IMPLÉMENTÉ |
| `actionChildren` | `ReactNode` | `undefined` | Rendu dans `.af-header__actions-children`. | IMPLÉMENTÉ |
| `className` | `string` | `undefined` | Ajouté à la classe racine via `getClassName`. | IMPLÉMENTÉ |
| Props `<header>` | `ComponentPropsWithoutRef<"header">` sauf `title` | — | Propagées sur l'élément `<header>`. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Univers Prospect | `HeaderApollo` | Importe `@axa-fr/canopee-css/prospect/Header/HeaderApollo.css`. | IMPLÉMENTÉ |
| Univers Client | `HeaderLF` | Importe `@axa-fr/canopee-css/client/Header/HeaderLF.css`. | IMPLÉMENTÉ |
| Menu mobile ouvert | Popover natif | `.af-header__menu:popover-open`. | IMPLÉMENTÉ / OBSERVÉ |

Le composant n'expose pas de prop `variant`.

## États et comportements

- Si `menuBurgerProps`, `tabMenuProps` ou `actionChildren` existe, un `ClickIcon` menu est rendu avec `src={menu}`, `size="S"`, `variant="ghost"`, classe `af-header__menu-icon`. | IMPLÉMENTÉ |
- En petit écran, le déclencheur reçoit `aria-haspopup="menu"`, `popoverTarget="af-header-menu"` et `popoverTargetAction="toggle"`. | IMPLÉMENTÉ |
- `.af-header__menu` reçoit `id="af-header-menu"` et `popover="auto"` seulement en petit écran. | IMPLÉMENTÉ |
- En desktop, la MDX indique que le menu est visible en ligne et l'icône cachée ; le CSS applique `.af-header__menu-icon { display: none; }`. | DOCUMENTÉ / OBSERVÉ |
- `MenuBurger` utilise la Popover API selon la MDX ; cette synthèse ne déduit pas d'autre règle de navigation. | DOCUMENTÉ |

## Anatomie

- Racine `.af-header` contenant `AppNameComponent`, puis éventuellement `ClickIconComponent`, puis `.af-header__menu`. | IMPLÉMENTÉ |
- `.af-header__menu` contient éventuellement `TabMenu`, puis `.af-header__actions`. | IMPLÉMENTÉ |
- `.af-header__actions` peut contenir `.af-header__title` (`Heading level=1`), `.af-header__actions-children`, et `MenuBurgerComponent`. | IMPLÉMENTÉ |
- Les classes ajoutées aux sous-composants sont `af-header__app-name`, `af-header__menu-burger`, `af-header__tab-menu`, `af-header__menu-icon`. | IMPLÉMENTÉ |

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Racine mobile | `position: sticky`, `z-index: 10`, `top: 0`, `display: flex`, `padding: var(--rem-8) var(--rem-24)`, `gap: var(--rem-16)`, `background: var(--white)`, `anchor-name: --af-header`. | OBSERVÉ |
| Heading dans header | `.af-header .af-heading { display: none; }`, puis visible en desktop via `display: inline-block`. | OBSERVÉ |
| Menu mobile | `position: absolute`, `top: calc(100% + 1px)`, `width: 100%`, `padding-block: var(--rem-24)`, `opacity: 0`, `visibility: hidden`, `transform: translateY(-0.25rem)`. | OBSERVÉ |
| Anchor positioning | `@supports (top: anchor(--af-header bottom))` définit `top: anchor(--af-header bottom)`, `position-anchor: --af-header`. | OBSERVÉ |
| Popover ouvert | `.af-header__menu:popover-open` : `height: 100svh`, `opacity: 1`, `visibility: visible`, `pointer-events: auto`. | OBSERVÉ |
| Starting style | `@starting-style` remet `opacity: 0` et `translateY(-0.25rem)` au départ. | OBSERVÉ |
| Actions mobile | `.af-header__actions` en colonne ; `.af-header__actions-children` avec `padding-inline: var(--rem-16)` et `gap: var(--rem-16)`. | OBSERVÉ |
| Desktop | `@media (--desktop-small)` : `.af-header` en grid, `padding: 0 var(--rem-32)`, `grid-template-columns: auto 1fr`. | OBSERVÉ |
| Overlay desktop | `.af-header::after` couvre `100vw/100vh`; `:has(:popover-open)` applique `background-color: var(--blue-1000-48)`. | OBSERVÉ |
| Client | Bord haut `2px solid var(--blue-1000)` et bord bas `2px solid var(--blue-200)`. | OBSERVÉ |
| Prospect | Bord bas `1px solid var(--blue-200)`. | OBSERVÉ |

Les valeurs proviennent du CSS source et peuvent évoluer.

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Sous-composants | `HeadingApollo`, `MenuBurgerApollo`, `ClickIconApollo`. | `HeadingLF`, `MenuBurgerLF`, `ClickIconLF`. | IMPLÉMENTÉ |
| Bordures | `border-bottom: 1px solid var(--blue-200)`. | `border-top: 2px solid var(--blue-1000)` et `border-bottom: 2px solid var(--blue-200)`. | OBSERVÉ |
| Documentation | Import `@axa-fr/canopee-react/prospect`. | Import `@axa-fr/canopee-react/client`. | DOCUMENTÉ |
| Stories | Jeux d'args équivalents avec `Default`, `Logo`, `LogoAndMenu`, `LogoAndActionChildren`, `LogoAndActionChildrenAndMenuBurger`, `LogoAndMenuBurger`, `LogoAndMenuWithTitle`, `LogoWithTitle`. | Même structure. | DOCUMENTÉ |

## Accessibilité

- Le bouton menu mobile est un `ClickIcon` avec `aria-haspopup="menu"` en petit écran et `aria-label` par défaut `Ouvrir le menu`. | IMPLÉMENTÉ |
- Le panneau est contrôlé par attributs Popover (`popoverTarget`, `popoverTargetAction`, `popover="auto"`) en petit écran. | IMPLÉMENTÉ |
- `title` est rendu comme `Heading level={1}` quand affiché. | IMPLÉMENTÉ |
- Aucune gestion clavier spécifique n'est codée dans `HeaderCommon`; elle dépend des éléments natifs/sous-composants. | OBSERVÉ |
- Conformité WCAG/RGAA non certifiée par les sources lues. | NON_CONFIRMÉ |

## Limites et points à confirmer

- `NON_CONFIRMÉ` : règles d'usage, cas d'exclusion, cardinalité par page, nombre maximal d'items de menu et microcopy ; vérifier le Zeroheight Prospect ou Client.
- Vérifier dans le Storybook de la version installée : comportement réel de la Popover API, focus dans le panneau mobile, rendu des bordures par univers et interactions du `MenuBurger`.
