# Table — Synthèse

Synthèse d'implémentation du composant `Table` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Composant composé pour rendre une table HTML et ses sous-composants typés. | DOCUMENTÉ / IMPLÉMENTÉ |
| Quand l'utiliser | Le MDX montre des tables basiques, alternées, avec tags, boutons, tailles et alignements. Règles design à confirmer. | DOCUMENTÉ / NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Aucune interdiction publiée. | NON_CONFIRMÉ |
| Cardinalité par page | Aucune limite publiée. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `Table`.
- Exports associés : `TableProps`, `HeadColorVariants`, `BodyColorVariants`, `RowSizeVariants`.
- Sous-composants : `Table.THead`, `Table.TBody`, `Table.Tr`, `Table.Th`, `Table.Td`.
- Élément racine rendu et classe CSS de base : `<table className="af-table">`.
- Dépendances internes : `Checkbox`, `ClickIcon`, icône `unfold_more-fill.svg`, `getClassName`.

```tsx
import { Table } from "@axa-fr/canopee-react/prospect";

<Table><Table.THead><Table.Tr><Table.Th>Nom</Table.Th></Table.Tr></Table.THead></Table>;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `Table` props | `ComponentPropsWithRef<"table">` | selon HTML | Transmises au `<table>`, avec classe `af-table`. | IMPLÉMENTÉ |
| `THead.variant` | `"gray" \| "blue"` | `"blue"` | Ajoute `af-table__thead--gray` ou `--blue`. | IMPLÉMENTÉ |
| `TBody.variant` | `"white" \| "blue" \| "alternate"` | `"white"` | Contrôle le fond des lignes/cellules. | IMPLÉMENTÉ |
| `Tr.size` | `"L" \| "M" \| "S"` | `"S"` | Ajoute `af-table__tr--large`, `--medium`, `--small`. | IMPLÉMENTÉ |
| `Th.position` | `"left" \| "center" \| "right"` | `"left"` | Aligne le contenu d'en-tête. | IMPLÉMENTÉ |
| `Th.checkboxPosition` | `"left" \| "center" \| "right"` | `"left"` | Ajoute `af-table__th--checkbox-${value}` ; seul `checkbox-right` a une règle CSS dédiée. | IMPLÉMENTÉ |
| `Th.onCheck` | `() => void` | `undefined` | Rend une `Checkbox` avec `onChange={onCheck}`. | IMPLÉMENTÉ |
| `Th.onSort` | `() => void` | `undefined` | Rend un `ClickIcon` ghost avec icône de tri. | IMPLÉMENTÉ |
| `Td.position` | `"left" \| "center" \| "right"` | `"left"` | Aligne le contenu cellule. | IMPLÉMENTÉ |
| `Td.verticalAlign` | `"top" \| "middle"` | `"middle"` | Ajoute une classe verticale. | IMPLÉMENTÉ |
| `Td.size` | `"L" \| "M" \| "S"` | `undefined` | Ajoute une taille propre à la cellule si fournie. | IMPLÉMENTÉ |
| `Td.variant` | `"white" \| "blue"` | `undefined` | Applique un fond cellule. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| En-tête | `gray`, `blue` | `af-table__thead--gray`, `--blue` | IMPLÉMENTÉ |
| Corps | `white`, `blue`, `alternate` | `af-table__tbody--white`, `--blue`, `--alternate` | IMPLÉMENTÉ |
| Ligne | `S`, `M`, `L` | `af-table__tr--small`, `--medium`, `--large` | IMPLÉMENTÉ |
| Cellule | `left`, `center`, `right`, `top`, `middle`, `white`, `blue` | classes `af-table__td--left`, `--center`, `--right`, `--top`, `--middle`, `--white`, `--blue` | IMPLÉMENTÉ |

## États et comportements

- `Table.Th` ajoute une checkbox uniquement si `onCheck` est fourni.
- `Table.Th` ajoute un bouton icône de tri uniquement si `onSort` est fourni.
- Aucune logique de tri, sélection de lignes ou pagination n'est implémentée par `Table`.
- Les stories utilisent `action(...)` pour simuler tri et sélection.

## Anatomie

- `table.af-table`.
- `thead.af-table__thead--variant` > `tr.af-table__tr--size` > `th.af-table__th--position`.
- Dans `th` : `div.af-table__th-wrapper`, checkbox optionnelle, `span.af-table__th-content`, icône tri optionnelle.
- `tbody.af-table__tbody--variant` > `tr` > `td.af-table__td--modifier`.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Table | `all: unset`, `display:table`, `width:100%`, `border-collapse:collapse`, `line-height:var(--rem-20)`. | OBSERVÉ |
| En-tête | Padding wrapper `var(--rem-16)`, label `font-size:var(--rem-16ptable)`, `font-weight:600`. | OBSERVÉ |
| Ligne | `border-bottom:1px solid var(--blue-200)`. | OBSERVÉ |
| Tailles | Cellules `10`, `18`, `30` / `var(--font-size-base)` en rem pour S/M/L. | OBSERVÉ |
| Fonds | Gris `var(--gray-050)`, bleu body `var(--blue-040)`, blanc `var(--white-1000)`. | OBSERVÉ |
| Prospect/Client | Header bleu `var(--blue-080)` en Prospect, `var(--blue-100)` en Client. | OBSERVÉ |

Aucune media query dédiée à `Table` n'a été relevée.

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| CSS importé | `TableApollo.css`. | `TableLF.css`. | IMPLÉMENTÉ |
| Header bleu | `--table-header-bg-blue: var(--blue-080)`. | `--table-header-bg-blue: var(--blue-100)`. | OBSERVÉ |
| Stories | Importent `Button`, `Tag` prospect. | Même scénarios avec imports client. | DOCUMENTÉ |

## Accessibilité

- Le composant conserve les balises HTML natives `table`, `thead`, `tbody`, `tr`, `th`, `td`.
- Aucun `scope`, `caption`, `aria-sort` ou état de tri n'est ajouté automatiquement.
- `onSort` rend un contrôle cliquable via `ClickIcon`, sans logique de tri accessible déclarée dans `Table`.
- La conformité WCAG/RGAA n'est pas certifiée.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : règles de tables responsives et relation avec `TableMobileCard`.
- `NON_CONFIRMÉ` : nombre de colonnes, tri accessible, sélection globale et libellés.
- Vérifier dans le Storybook de la version installée : rendu des sous-composants `Checkbox`, `ClickIcon`, `Button`, `Tag`.
