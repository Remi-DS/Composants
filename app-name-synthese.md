# AppName — Synthèse

Synthèse d'implémentation du composant `AppName` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Affiche le nom de l'application à côté du logo AXA. | DOCUMENTÉ |
| Placement | Le MDX indique qu'il est typiquement placé dans le composant `Header`. | DOCUMENTÉ |
| Quand l'utiliser | Usage précis par contexte non publié ; vérifier le Zeroheight de l'univers. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Non publié dans les sources lues. | NON_CONFIRMÉ |
| Cardinalité par page | Non publiée dans les sources lues. | NON_CONFIRMÉ |

## Intégration React

- Package Prospect : `@axa-fr/canopee-react/prospect`, export `AppName`, type `AppNameProps`.
- Package Client : `@axa-fr/canopee-react/client`, export `AppName`, type `AppNameProps`.
- Fichier React commun : `AppName.tsx`.
- CSS commun importé depuis le fichier React : `@axa-fr/canopee-css/prospect/AppName/AppNameAll.css`.
- Élément racine : `<div class="af-app-name">`.

```tsx
import { AppName } from "@axa-fr/canopee-react/prospect";

<AppName label="Mon application" logoLinkProps={{ href: "/" }} />;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `label` | `string` | — | Affiché dans `<span class="af-app-name__label">`. | IMPLÉMENTÉ |
| `logoAlt` | `string` | `"Logo AXA"` | Alt de l'image du logo AXA. | IMPLÉMENTÉ |
| `logoLinkProps` | `Record<string, unknown>` | `undefined` | Props transmises au composant qui enveloppe le logo. | IMPLÉMENTÉ |
| `LogoLinkComponent` | `ElementType` | `"a"` | Élément/composant utilisé pour envelopper le logo. | IMPLÉMENTÉ |
| `className` | `string` | `undefined` | Fusionné avec `af-app-name` via `getClassName`. | IMPLÉMENTÉ |
| Props natives | `ComponentPropsWithoutRef<"div">` | — | Les autres props sont transmises au `<div>` racine. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Aucune variante exposée | — | `af-app-name` | IMPLÉMENTÉ |

Le composant n'expose pas d'objet de variantes.

## États et comportements

- Le logo est l'asset `logo-axa.svg`.
- Le code crée toujours un wrapper si `LogoLinkComponent` est truthy ; comme la valeur par défaut est `"a"`, un `<a class="af-app-name__logo-link">` est rendu par défaut.
- Les MDX parlent de "Logo only (no link)" avec `<AppName label="My application" />`, et leur tableau indique `LogoLinkComponent` défaut `undefined`. C'est contradictoire avec le code, qui met `"a"` par défaut.
- `logoLinkProps` permet par exemple `href` pour un lien natif ou `to` pour un composant React Router.

## Anatomie

- Racine : `<div class="af-app-name">`.
- Wrapper logo : `<LogoLinkComponent class="af-app-name__logo-link">` recevant les props de `logoLinkProps`.
- Image : `<img src={logoAxa} alt={logoAlt} class="af-app-name__logo">`.
- Libellé : `<span class="af-app-name__label">{label}</span>`.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Racine | `display: inline-flex`, `align-items: center`, `gap: var(--rem-16)`. | OBSERVÉ |
| Logo mobile | `width` et `height` à `var(--rem-40)`, `margin-block: var(--rem-8)`. | OBSERVÉ |
| Label mobile | `display: none`, `max-inline-size: var(--rem-96)`, taille `var(--rem-16)`. | OBSERVÉ |
| Label | Graisse `var(--font-weight-semibold)`, `line-height: var(--rem-16)`, couleur `var(--blue-1000)`. | OBSERVÉ |
| Desktop | À `@media (--desktop-small)`, logo `var(--rem-56)`, `margin-block: var(--rem-16)`, label `display: inline`. | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Export public | `@axa-fr/canopee-react/prospect`. | `@axa-fr/canopee-react/client`. | IMPLÉMENTÉ |
| Code React | Même fichier `AppName.tsx`. | Même fichier `AppName.tsx`. | IMPLÉMENTÉ |
| CSS | Le fichier React importe le CSS prospect `AppNameAll.css`. | Même composant commun ; pas de CSS LF distinct lu. | OBSERVÉ |
| Documentation | Import prospect dans MDX Apollo. | Import client dans MDX Look & Feel. | DOCUMENTÉ |

## Accessibilité

- L'image du logo reçoit un `alt`, par défaut `"Logo AXA"`.
- Si un lien est rendu, son accessibilité dépend des props passées au `LogoLinkComponent`.
- Le libellé visible est masqué en CSS avant `--desktop-small`, mais reste dans le DOM.
- La conformité WCAG/RGAA n'est pas certifiée par les sources lues.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : règle de placement obligatoire ou non dans `Header`, au-delà du "typically placed".
- `NON_CONFIRMÉ` : cardinalité, libellé attendu et comportement de navigation du logo.
- `NON_CONFIRMÉ` : contradiction entre le défaut MDX de `LogoLinkComponent` et le défaut réel `"a"` dans le code.
- Vérifier dans le Storybook de la version installée : rendu d'un `<a>` sans `href` quand aucune prop de lien n'est fournie.
