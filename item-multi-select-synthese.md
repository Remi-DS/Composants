# ItemMultiSelect — Synthèse

Synthèse d'implémentation du composant `ItemMultiSelect` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Combine une checkbox et un label alignés horizontalement. | DOCUMENTÉ |
| Quand l'utiliser | Le MDX montre un item de sélection multiple, sans règle d'arbitrage design. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Non publié dans les sources lues. | NON_CONFIRMÉ |
| Cardinalité par page | Non publiée. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `ItemMultiSelect`, `type ItemMultiSelectProps`.
- Exports associés : le type interne `ItemMultiSelectVariant = "primary" | "secondary"` n'est pas exporté publiquement dans `prospect.ts`/`client.ts`.
- Élément racine rendu et classe CSS de base : `<label className="af-item-multi-select af-item-multi-select--{variant}">`.
- Dépendances internes : `Checkbox` de l'univers, `getClassName`.

```tsx
import { ItemMultiSelect } from "@axa-fr/canopee-react/prospect";

<ItemMultiSelect label="I agree to the terms" variant="primary" name="agree" value="agree" />
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `label` | `ReactNode` | requis | Rendu dans `.af-item-multi-select__label`. | IMPLÉMENTÉ |
| `variant` | `"primary" \| "secondary"` | `"primary"` | Ajoute le modificateur de fond. | IMPLÉMENTÉ |
| `id` | `string` | - | Utilisé comme `htmlFor` du label et transmis à la checkbox. | IMPLÉMENTÉ |
| props natives | `Omit<ComponentProps<"input">, "type">` | natif | Transmises à `Checkbox`; `type` est porté par le composant Checkbox interne. | IMPLÉMENTÉ |
| `className` | `string` | `""` | Fusionné sur le label racine. | IMPLÉMENTÉ |

Préciser l'héritage des props natives : le type reprend les props d'`input` sauf `type`.

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Primaire | `primary` | `af-item-multi-select--primary` | IMPLÉMENTÉ |
| Secondaire | `secondary` | `af-item-multi-select--secondary` | IMPLÉMENTÉ |

## États et comportements

Les états `checked`, `disabled`, `name`, `value` et autres attributs sont transmis à la checkbox. Le MDX précise que seul l'état de la checkbox change au hover ou à la sélection, tandis que le fond du conteneur reste stable. La story Client ajoute un scénario `WithLongLabel`.

## Anatomie

Structure DOM : `<label>` racine, `<span className="af-item-multi-select__checkbox">` contenant `Checkbox`, puis `<span className="af-item-multi-select__label">` contenant le label.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Base | `display:inline-flex`, `width:100%`, `padding: var(--size-16)`, `align-items:center`, `column-gap: var(--size-8)` | OBSERVÉ |
| Curseur | `cursor:pointer` sur le label racine | OBSERVÉ |
| Checkbox | `display:inline-flex`, `flex: 0 0 auto`, `align-items:center` | OBSERVÉ |
| Label | `flex:1 1 auto`, `font-size: var(--rem-16)`, `font-weight:400` | OBSERVÉ |
| Primaire | `--item-multi-select-background-color: var(--blue-040)` | OBSERVÉ |
| Secondaire | `--item-multi-select-background-color: var(--white-1000)` | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Checkbox | `CheckboxApollo`. | `CheckboxLF`. | IMPLÉMENTÉ |
| CSS de variantes | mêmes valeurs `blue-040` et `white-1000`. | mêmes valeurs `blue-040` et `white-1000`. | OBSERVÉ |
| Stories | `checked:false` dans les args par défaut. | pas de `checked` par défaut ; story supplémentaire `WithLongLabel`. | DOCUMENTÉ |

## Accessibilité

Le composant utilise un `<label>` natif associé à la checkbox via `htmlFor={id}`. Les attributs ARIA éventuels passés comme props natives sont transmis à la checkbox. Aucune conformité WCAG/RGAA n'est certifiée.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : nombre d'options, règle de groupement, texte des options, comportement attendu avec labels longs.
- `DOCUMENTÉ` : deux variantes de fond et stabilité du fond au hover/sélection.
- Vérifier dans le Storybook de la version installée : rendu de la checkbox de l'univers et alignement des labels longs.
