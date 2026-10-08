# TableMobileCard — Synthèse

Synthèse d'implémentation du composant `TableMobileCard` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Rendre une carte mobile sous forme de liste de définitions `dl/dt/dd`, avec lignes configurables. | IMPLÉMENTÉ |
| Quand l'utiliser | Le MDX indique explicitement : `It is the mobile version for a table.` | DOCUMENTÉ |
| Quand ne pas l'utiliser | Le MDX indique : `This component should not be used alone.` | DOCUMENTÉ |
| Cardinalité par page | Aucune limite publiée. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `TableMobileCard`.
- Exports associés : `TableMobileCardProps` dans `client.ts`; non ré-exporté dans `prospect.ts` lu.
- Sous-composants : `TableMobileCard.DRow`, `TableMobileCard.Dt`, `TableMobileCard.Dd`.
- Élément racine rendu et classe CSS de base : `<dl className="af-table-mobile-card">`.
- Dépendances internes : `getClassName`.

```tsx
import { TableMobileCard } from "@axa-fr/canopee-react/prospect";

<TableMobileCard variant="alternate">
  <TableMobileCard.DRow><TableMobileCard.Dt>Code</TableMobileCard.Dt><TableMobileCard.Dd>LU0101010101</TableMobileCard.Dd></TableMobileCard.DRow>
</TableMobileCard>;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `variant` | `"white" \| "blue" \| "alternate"` | `"alternate"` | Ajoute `af-table-mobile-card--${variant}`. | IMPLÉMENTÉ |
| `className` | `string` | `undefined` | Ajouté à la classe racine. | IMPLÉMENTÉ |
| Props racine | `ComponentPropsWithRef<"dl">` | selon HTML | Transmises au `<dl>`. | IMPLÉMENTÉ |
| `DRow.direction` | `"row" \| "column"` | `"row"` | Ajoute `af-table-mobile-card__drow--row` ou `--column`. | IMPLÉMENTÉ |
| `DRow` props | `ComponentPropsWithRef<"div">` | selon HTML | Transmises au `<div>` ligne. | IMPLÉMENTÉ |
| `Dt` props | `ComponentPropsWithRef<"dt">` | selon HTML | Transmises au `<dt>`. | IMPLÉMENTÉ |
| `Dd` props | `ComponentPropsWithRef<"dd">` | selon HTML | Transmises au `<dd>`. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Fond carte | `white` | `af-table-mobile-card--white` | IMPLÉMENTÉ |
| Fond carte | `blue` | `af-table-mobile-card--blue` | IMPLÉMENTÉ |
| Fond carte | `alternate` | `af-table-mobile-card--alternate` | IMPLÉMENTÉ |
| Direction ligne | `row` | `af-table-mobile-card__drow--row` | IMPLÉMENTÉ |
| Direction ligne | `column` | `af-table-mobile-card__drow--column` | IMPLÉMENTÉ |

## États et comportements

- Aucun état React interne.
- La composition est libre : les stories placent aussi des `Button` et `Icon` dans `Dd`.
- Les tests vérifient les classes `DRow` et la présence de rôles `definition` pour les `dd`.
- Le composant ne transforme pas automatiquement une `Table` desktop.

## Anatomie

- `dl.af-table-mobile-card.af-table-mobile-card--variant`.
- Chaque ligne : `div.af-table-mobile-card__drow.af-table-mobile-card__drow--direction`.
- Terme : `dt.af-table-mobile-card__dt`.
- Description : `dd.af-table-mobile-card__dd`.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Racine | Bordure `1px solid var(--blue-200)`, rayon `var(--radius-8)`, `overflow:hidden`. | OBSERVÉ |
| Typographie | `font-size: calc(16 / var(--font-size-base) * 1rem)`, line-height `20`. | OBSERVÉ |
| Ligne | `display:flex`, `padding:var(--rem-16)`, `gap:var(--rem-16)`, `justify-content:space-between`. | OBSERVÉ |
| Row | `dt` et `dd` à `50%`; `dt` poids `400`, `dd` poids `600` et aligné à droite. | OBSERVÉ |
| Column | `dt` et `dd` à `100%`; `dd` aligné à gauche. | OBSERVÉ |
| Fonds | `white`: `var(--white-1000)`, `blue`: `var(--blue-040)`, `alternate`: pair bleu/impair blanc. | OBSERVÉ |

Aucune media query dédiée n'a été relevée.

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Implémentation | Même fichier `TableMobileCard.tsx`, import CSS prospect. | Même fichier `TableMobileCard.tsx`. | IMPLÉMENTÉ |
| Export type | `TableMobileCard` seulement dans `prospect.ts`. | `TableMobileCard` et `TableMobileCardProps` dans `client.ts`. | OBSERVÉ |
| Stories | Exemple avec `Button`, `Icon` prospect. | Exemple équivalent avec imports client, mais placement des `onClick` d'icône/bouton varie. | DOCUMENTÉ |

## Accessibilité

- Sémantique native `dl`, `dt`, `dd` conservée.
- Les lignes intermédiaires sont des `div`.
- Aucune relation ARIA additionnelle, aucun comportement clavier propre.
- La conformité WCAG/RGAA n'est pas certifiée.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : règle responsive exacte reliant `Table` et `TableMobileCard`.
- `NON_CONFIRMÉ` : nombre maximal de lignes et contenu autorisé dans `Dd`.
- Vérifier dans le Storybook de la version installée : rendu mobile réel et association avec une table desktop.
