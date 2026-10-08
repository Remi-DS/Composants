# ItemLabel — Synthèse

Synthèse d'implémentation du composant `ItemLabel` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Rend un label de formulaire accessible avec description optionnelle et boutons d'action optionnels. | DOCUMENTÉ |
| Quand l'utiliser | Le MDX le présente comme label proche des sémantiques natives `<label>`, sans règle d'arbitrage. | DOCUMENTÉ |
| Quand ne pas l'utiliser | Non publié. | NON_CONFIRMÉ |
| Cardinalité par page | Non publiée. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `ItemLabel`.
- Exports associés : aucun type public dans les fichiers d'exports.
- Élément racine rendu et classe CSS de base : `<div className="af-item-label">` contenant `<label className="af-item-label__label">`.
- Dépendances internes : `Button` de l'univers, `Svg`, icône `info.svg`.

```tsx
import { ItemLabel } from "@axa-fr/canopee-react/prospect";

<ItemLabel htmlFor="file-input" description="Description Text" required>
  Label Text
</ItemLabel>
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| props natives | `ComponentProps<"label">` | natif | Transmises au `<label>` interne, sauf `className` et `style` appliqués au wrapper. | IMPLÉMENTÉ |
| `children` | `ReactNode` | - | Contenu du label ; si absent, le composant retourne `null`. | IMPLÉMENTÉ |
| `description` | `ReactNode` | - | Rendu dans `.af-item-label__description` et ajouté à `aria-describedby`. | IMPLÉMENTÉ |
| `required` | `boolean` | `false` documenté | Ajoute un `*` visuel avec `aria-hidden="true"`. | IMPLÉMENTÉ |
| `sideButtonLabel` | `ReactNode` | - | Rend un bouton `variant="ghost"` en zone sidebutton. | IMPLÉMENTÉ |
| `onSideButtonClick` | `MouseEventHandler<HTMLButtonElement>` | - | Handler du bouton latéral. | IMPLÉMENTÉ |
| `sideButtonProps` | props partielles de `ButtonProps` sans `children/onClick/variant/className` | - | Étend le bouton latéral. | IMPLÉMENTÉ |
| `moreButtonLabel`, `onMoreButtonClick`, `moreButtonProps` | idem bouton more | - | Rend un bouton ghost avec icône info à gauche. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Avec description | `description` renseigné | `af-item-label__description` | IMPLÉMENTÉ |
| Avec bouton latéral | `sideButtonLabel` renseigné | `af-item-label__sidebutton` | IMPLÉMENTÉ |
| Avec bouton more | `moreButtonLabel` renseigné | `af-item-label__more` | IMPLÉMENTÉ |
| Variante design nommée | Aucune prop `variant`. | - | IMPLÉMENTÉ |

## États et comportements

`aria-describedby` combine l'id généré de la description et l'éventuel `aria-describedby` reçu. Les boutons utilisent toujours `variant="ghost"` et des classes fixes. `moreButton` ajoute `iconLeft` avec un `Svg role="presentation"`.

## Anatomie

Structure DOM : wrapper `.af-item-label`, label `.af-item-label__label`, astérisque éventuel, bouton `.af-item-label__sidebutton`, description `.af-item-label__description`, bouton `.af-item-label__more`.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Wrapper | grille `label sidebutton`, `description sidebutton`, `more more`, colonnes `1fr auto`, `row-gap: var(--rem-4)` | OBSERVÉ |
| Sidebutton | `column-gap: var(--rem-12)` si présent, `place-self:center end` | OBSERVÉ |
| Label | `display:flex`, `gap: var(--rem-4)`, `font-size: var(--rem-16)`, poids `600`, line-height `var(--rem-20)` | OBSERVÉ |
| Label desktop | `font-size: var(--rem-18)`, `line-height: var(--rem-24)` | OBSERVÉ |
| Description | `font-size: var(--rem-14)`, poids `400`, `line-height:1.25em`; desktop `var(--rem-16)` | OBSERVÉ |
| Couleurs | Prospect et Client : `--label-color: var(--gray-1000)`, `--label-description-color: var(--gray-800)` | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Button injecté | `ButtonApollo`. | `ButtonLF`. | IMPLÉMENTÉ |
| CSS | mêmes tokens dans `ItemLabelApollo.css`. | mêmes tokens dans `ItemLabelLF.css`. | OBSERVÉ |
| MDX | import Prospect. | import Client. | DOCUMENTÉ |

## Accessibilité

Le MDX recommande de fournir des noms accessibles aux boutons, d'utiliser `aria-haspopup`, `aria-expanded`, `aria-controls` si un bouton contrôle une popup, et d'éviter `tabIndex` sur des éléments non interactifs. Le code associe la description par `aria-describedby`; il ne définit pas `aria-required`. Aucune conformité WCAG/RGAA n'est certifiée.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : formulation du label, longueur, usage simultané des deux boutons, cardinalité.
- `DOCUMENTÉ` : `className` et `style` s'appliquent au wrapper, pas au `<label>` interne.
- Vérifier dans le Storybook de la version installée : rendu avec descriptions multi-lignes et boutons d'action.
