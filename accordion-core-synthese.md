# AccordionCore — Synthèse

Synthèse d'implémentation du composant `AccordionCore` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Noyau d'accordéon basé sur `<details>` / `<summary>`, avec résumé personnalisable et flèche. | IMPLÉMENTÉ |
| Documentation | Le MDX reprend la description de l'Accordion complet et précise l'usage de `open` et `arrowClickIconVariant`. | DOCUMENTÉ |
| Quand l'utiliser | Usage design du noyau seul non précisé ; vérifier le Zeroheight de l'univers. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Non publié dans les sources lues. | NON_CONFIRMÉ |
| Cardinalité par page | Non publiée dans les sources lues. | NON_CONFIRMÉ |

## Intégration React

- Package Prospect : `@axa-fr/canopee-react/prospect`, export `AccordionCore`.
- Package Client : `@axa-fr/canopee-react/client`, export `AccordionCore`.
- Fichiers : `AccordionCoreCommon.tsx`, `AccordionCoreApollo.tsx`, `AccordionCoreLF.tsx`.
- Élément racine : `<details class="af-apollo-accordion">`.
- Dépendances internes : `Icon`, `getClassName`, icône `keyboard_arrow_down-fill.svg`.

```tsx
import { AccordionCore } from "@axa-fr/canopee-react/prospect";

<AccordionCore summary="Titre onglet">
  Lorem ipsum
</AccordionCore>;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `summary` | `ReactNode` | — | Contenu placé dans le `<summary>` avant la flèche. | IMPLÉMENTÉ |
| `summaryProps` | `Omit<ComponentProps<"summary">, "onClick">` | `undefined` | Props transmises au `<summary>`, avec fusion de `className`. | IMPLÉMENTÉ |
| `onClick` | `MouseEventHandler<HTMLElement>` | `undefined` | Si fourni, `event.preventDefault()` puis appel du handler. | IMPLÉMENTÉ |
| `showArrowAsClickIcon` | `boolean` | `true` | Ajoute la classe `af-click-icon` à la flèche si vrai. | IMPLÉMENTÉ |
| `arrowClickIconVariant` | `ClickIconVariant` | `"default"` | Ajoute le modificateur `ghost` à `af-click-icon` quand la valeur est `"ghost"`. | IMPLÉMENTÉ |
| `arrowIconVariant` | `IconProps["variant"]` | `undefined` | Transmis au composant `Icon` de flèche. | IMPLÉMENTÉ |
| Props natives | `ComponentProps<"details">` | — | Inclut `open`, `children`, `className` et les attributs HTML de `<details>`. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Click icon par défaut | `showArrowAsClickIcon=true` | `af-click-icon` | IMPLÉMENTÉ |
| Click icon ghost | `arrowClickIconVariant="ghost"` | `af-click-icon af-click-icon--ghost` | IMPLÉMENTÉ |
| Plain | `className="af-apollo-accordion--plain"` | `af-apollo-accordion--plain` | DOCUMENTÉ / OBSERVÉ |

Le composant n'expose pas d'objet de variantes propre ; `plain` est documenté comme classe passée manuellement.

## États et comportements

- État fermé/ouvert : géré par le `<details>` natif et la prop `open`.
- Quand `[open]`, le résumé reçoit une bordure basse et la flèche est transformée par `rotate(180deg)`.
- Avec `onClick`, l'ouverture native est empêchée ; l'intégrateur doit gérer l'état s'il veut modifier `open`.
- `prefers-reduced-motion: reduce` annule seulement le délai de transition de la flèche, pas la transition elle-même.

## Anatomie

- Racine : `<details>` avec `af-apollo-accordion` et les classes additionnelles éventuelles.
- Résumé : `<summary class="af-apollo-accordion__summary" tabIndex={0}>`.
- Flèche : `<div class="af-accordion__arrow">` enrichi éventuellement par `af-click-icon`, contenant l'icône `keyboardDown`.
- Contenu : `children` après le résumé.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Commun | `--spacing-summary-inline: var(--rem-16)`, `--spacing-summary-block: var(--rem-16)`. | OBSERVÉ |
| Commun desktop | `--spacing-summary-inline: var(--rem-24)` à `@media (--desktop-small)`. | OBSERVÉ |
| Résumé | `display: grid`, `column-gap: 16px`, `font-weight: 600`, `cursor: pointer`. | OBSERVÉ |
| Marqueur natif | `&::-webkit-details-marker { display: none; }`. | OBSERVÉ |
| Plain | Bordure `1px solid var(--accordion-border-color)`, radius `var(--radius-8)`. | OBSERVÉ |
| Contenu plain ouvert | `display: grid`, `padding` et `row-gap` à `var(--spacing-summary-inline)`. | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Implémentation React | Injecte `IconApollo`. | Injecte `IconLF`. | IMPLÉMENTÉ |
| CSS univers | Importe `AccordionCoreApollo.css`. | Importe `AccordionCoreLF.css`. | IMPLÉMENTÉ |
| Bordure | `--accordion-border-color: var(--blue-200)`. | Même valeur `var(--blue-200)`. | OBSERVÉ |

## Accessibilité

- Utilise les éléments natifs `<details>` et `<summary>`.
- Le résumé reçoit `tabIndex={0}`.
- L'icône de flèche reçoit `role="presentation"`.
- Aucun attribut `aria-expanded` manuel n'est ajouté ; la sémantique native est utilisée. Conformité WCAG/RGAA non certifiée.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : recommandations d'usage du composant core seul plutôt que des variantes métier.
- `NON_CONFIRMÉ` : règles de cardinalité et libellés de résumé.
- Vérifier dans le Storybook de la version installée : comportement attendu quand `onClick` empêche l'ouverture native.
