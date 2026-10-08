# Card — Synthèse

Synthèse d'implémentation du composant `Card` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Wrapper simple qui ajoute padding et bordure. | DOCUMENTÉ |
| Élément rendu | La prop `as` contrôle l'élément HTML sous-jacent. | DOCUMENTÉ / IMPLÉMENTÉ |
| Quand l'utiliser | Non publié au-delà du rôle de wrapper ; vérifier le Zeroheight de l'univers. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Non publié dans les sources lues. | NON_CONFIRMÉ |
| Cardinalité par page | Non publiée dans les sources lues. | NON_CONFIRMÉ |

## Intégration React

- Package Prospect : `@axa-fr/canopee-react/prospect`, export `Card`, `cardVariants`, types `CardProps`, `CardVariants`.
- Package Client : `@axa-fr/canopee-react/client`, mêmes exports publics.
- Fichiers : `CardCommon.tsx`, `CardApollo.tsx`, `CardLF.tsx`.
- Élément racine par défaut : `<div class="af-card">`.
- Dépendances internes : `getClassName`, type utilitaire `PolymorphicComponent`.

```tsx
import { Card, Heading } from "@axa-fr/canopee-react/prospect";

<Card>
  <Heading level={2} className="af-card__title">My card title</Heading>
  <p>My card content</p>
</Card>;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `as` | `ElementType` | `"div"` | Choisit le composant ou élément rendu. | IMPLÉMENTÉ |
| `variant` | `"unstyled"` | `undefined` | Ajoute `af-card--unstyled`. | IMPLÉMENTÉ |
| `className` | `string` | `undefined` | Fusionnée avec `af-card`. | IMPLÉMENTÉ |
| `children` | `ReactNode` | `undefined` | Contenu rendu dans la carte. | IMPLÉMENTÉ |
| Props polymorphes | `PolymorphicComponent<T, ComponentProps<"div"> & { variant?: CardVariants }>` | — | Les props compatibles avec l'élément choisi sont transmises. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Standard | `undefined` | `af-card` | IMPLÉMENTÉ |
| Sans style | `unstyled` | `af-card af-card--unstyled` | IMPLÉMENTÉ / DOCUMENTÉ |

`cardVariants` contient uniquement `{ unstyled: "unstyled" }`, mais le type `CardVariants` est `keyof typeof cardVariants`, donc `"unstyled"`.

## États et comportements

- Si `as="button"`, le CSS applique `cursor: pointer`.
- Pour une carte rendue en `button`, `:hover`, `:focus-visible` et `:focus-within` passent `--card-border-width` de `1px` à `2px`.
- La story `ButtonCard` montre `as: "button"`, `type: "button"` et une action `onClick`.
- La variante `unstyled` retire padding, border, outline et outline-offset.

## Anatomie

- Racine polymorphe : `<Component class="af-card [af-card--unstyled]">`.
- Contenu : `children` libre.
- Les stories utilisent un `Heading` avec `className="af-card__title"`, mais aucune règle CSS `af-card__title` n'est lue dans le CSS Card.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Commun | `padding: var(--rem-16)`. | OBSERVÉ |
| Commun | `border: inherit`, `text-align: inherit`, `background-color: var(--card-background-color)`. | OBSERVÉ |
| Commun | `outline: var(--card-border-width) solid var(--card-border-color)` et offset négatif. | OBSERVÉ |
| Standard | `--card-border-width: 1px`. | OBSERVÉ |
| Unstyled | `padding: 0`, `border: 0`, `outline: 0`, `outline-offset: 0`. | OBSERVÉ |
| Listes | `:is(ol, ul).af-card.af-card--unstyled { padding: 0; }`. | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Radius | `--card-border-radius: var(--radius-8)`. | `--card-border-radius: var(--radius-4)`. | OBSERVÉ |
| Fond | `var(--white-1000)`. | `var(--white-1000)`. | OBSERVÉ |
| Bordure | `--card-border-color: var(--gray-140)`. | `--card-border-color: var(--gray-050)`. | OBSERVÉ |
| MDX | Documente basic, button et unstyled. | Même contenu, plus une note sur `className`. | DOCUMENTÉ |

## Accessibilité

- La sémantique dépend de `as` : par défaut `div`, ou par exemple `button` dans la story.
- Si la carte est interactive, l'exemple documenté utilise `as="button"` et `type="button"`.
- Aucun rôle ARIA n'est ajouté par le composant.
- La conformité WCAG/RGAA n'est pas certifiée par les sources lues.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : critères design de choix entre Card standard, clickable et unstyled.
- `NON_CONFIRMÉ` : cardinalité, hiérarchie de contenu et règles de titre.
- Vérifier dans le Storybook de la version installée : rendu focus/hover réel selon l'élément `as`.
