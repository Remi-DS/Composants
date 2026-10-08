# AccordionContextual — Synthèse

Synthèse d'implémentation du composant `AccordionContextual` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Composant interactif secondaire permettant d'afficher ou masquer un contenu additionnel sans surcharger la page. | DOCUMENTÉ |
| Interaction | Le clic sur le résumé peut déclencher `onClick` avec l'événement natif. | DOCUMENTÉ / IMPLÉMENTÉ |
| Quand l'utiliser | Les sources parlent d'information non essentielle au parcours ; les règles design détaillées restent à vérifier dans Zeroheight. | DOCUMENTÉ / NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Non publié dans les sources lues. | NON_CONFIRMÉ |
| Cardinalité par page | Non publiée dans les sources lues. | NON_CONFIRMÉ |

## Intégration React

- Package Prospect : `@axa-fr/canopee-react/prospect`, export `AccordionContextual`, `accordionContextualVariants`, type `AccordionContextualVariants`.
- Package Client : `@axa-fr/canopee-react/client`, mêmes exports publics.
- Fichiers : `AccordionContextualCommon.tsx`, `AccordionContextualApollo.tsx`, `AccordionContextualLF.tsx`.
- Élément racine rendu : `AccordionCore`, donc `<details>` avec `af-apollo-accordion`, `af-apollo-accordion-contextual` et le modificateur de variante.
- Dépendances internes : `AccordionCore`, `Icon`, `getClassName`.

```tsx
import { AccordionContextual } from "@axa-fr/canopee-react/prospect";

<AccordionContextual icon={bankIcon} title="Titre onglet">
  Contenu additionnel
</AccordionContextual>;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `variant` | `"info" \| "warning" \| "reverse"` | `"info"` | Ajoute `af-apollo-accordion-contextual--{variant}` et pilote la couleur de l'icône/flèche. | IMPLÉMENTÉ |
| `title` | `string` | — | Rend `<p class="af-accordion__title">`. | IMPLÉMENTÉ |
| `icon` | `string` | `undefined` | Rend un `Icon` décoratif `role="presentation"`, `size="S"`, si fourni. | IMPLÉMENTÉ |
| Props héritées | `Omit<AccordionCoreProps, "summary">` | — | Inclut `children`, `open`, `onClick`, `summaryProps`, `arrowIconVariant`, etc. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Information | `info` | `af-apollo-accordion-contextual--info` | IMPLÉMENTÉ |
| Avertissement | `warning` | `af-apollo-accordion-contextual--warning` | IMPLÉMENTÉ |
| Inversée | `reverse` | `af-apollo-accordion-contextual--reverse` | IMPLÉMENTÉ |

`getIconVariant` retourne `primary` pour `info`, `error` pour `warning` et `secondary` pour `reverse`.

## États et comportements

- `showArrowAsClickIcon` est forcé à `false` dans `AccordionContextualCommon`.
- `arrowIconVariant` est dérivé de `variant`.
- Quand `open`, le CSS supprime la bordure basse du résumé contextual et affiche le contenu via `::details-content` avec `display: grid` et `padding: var(--rem-16)`.
- Sans icône, les zones CSS passent de `"icon title arrow"` à `"title arrow"`.
- Le variant `reverse` force le titre puis tous les descendants à `white`.

## Anatomie

- Racine : `<details class="af-apollo-accordion af-apollo-accordion-contextual af-apollo-accordion-contextual--{variant}">`.
- Résumé : classe `af-apollo-accordion__summary`.
- Contenu du résumé : icône optionnelle `af-accordion__icon`, titre `af-accordion__title`, flèche `af-accordion__arrow`.
- Contenu déplié : `children` rendu par `AccordionCore`.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Commun | `--accordion-contextual-column-gap: var(--rem-16)`, `--accordion-contextual-gap: var(--rem-4)`, `--spacing-summary-inline: var(--rem-12)`. | OBSERVÉ |
| Commun | Résumé en grille, `padding: var(--spacing-summary-block) var(--spacing-summary-inline)`, `line-height: var(--rem-20)`. | OBSERVÉ |
| Prospect | `--accordion-contextual-title-font-size: var(--rem-16)`. | OBSERVÉ |
| Prospect info | Couleur titre `var(--blue-1000)`. | OBSERVÉ |
| Prospect warning | Couleur titre `var(--red-alert-1000)`. | OBSERVÉ |
| Client | Taille titre `var(--rem-16)`, puis `var(--rem-18)` à `@media (--desktop-small)`. | OBSERVÉ |
| Client warning | Couleur titre `var(--red-alert-1200)`. | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Implémentation React | Utilise `IconApollo`. | Utilise `IconCommon`. | IMPLÉMENTÉ |
| Couleur warning | `var(--red-alert-1000)`. | `var(--red-alert-1200)`. | OBSERVÉ |
| Taille desktop | Pas de media query spécifique dans le CSS contextual Apollo. | Titre à `var(--rem-18)` sur `--desktop-small`. | OBSERVÉ |

## Accessibilité

- S'appuie sur `<details>` / `<summary>` via `AccordionCore`.
- L'icône optionnelle est décorative avec `role="presentation"`.
- Le résumé reçoit `tabIndex={0}` par `AccordionCore`.
- Aucun rôle ARIA spécifique n'est ajouté par `AccordionContextualCommon`; conformité WCAG/RGAA non certifiée.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : règle d'usage exacte de chaque variante (`info`, `warning`, `reverse`) dans les parcours.
- `NON_CONFIRMÉ` : cardinalité, microcopy et critères d'exclusion.
- Vérifier dans le Storybook de la version installée : contraste réel du variant `reverse` sur le fond utilisé.
