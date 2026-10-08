# MultiSelectList — Synthèse

Synthèse d'implémentation du composant `MultiSelectList` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Liste `<ul>` d'items multisélectionnables, chaque item enveloppant une checkbox et un libellé. | IMPLÉMENTÉ |
| Quand l'utiliser | Les MDX montrent des exemples à 3 items et 6 items avec scroll, sans règle d'usage design. | DOCUMENTÉ |
| Quand ne pas l'utiliser | Non publié ; consulter le Zeroheight de l'univers concerné. | NON_CONFIRMÉ |
| Cardinalité par page | Non publiée ; la story montre 3 et 6 items mais ce ne sont pas des bornes design. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `MultiSelectList`.
- Exports associés : `MultiSelectListProps`.
- Élément racine rendu : `<ul className="af-multi-select-list">`.
- Dépendances internes : `ItemMultiSelectApollo` ou `ItemMultiSelectLF`, eux-mêmes basés sur `Checkbox`.

```tsx
import { MultiSelectList } from "@axa-fr/canopee-react/prospect";

<MultiSelectList
  items={[
    { id: "item-1", label: "Option 1" },
    { id: "item-2", label: "Option 2", checked: true },
  ]}
/>;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `items` | `Omit<ItemMultiSelectCommonProps, "Checkbox">[]` | requis | Chaque entrée produit un `<li>` et un `ItemMultiSelectComponent`. | IMPLÉMENTÉ |
| Props de `List` | `Omit<ListProps, "children" \| "separator" \| "onChange">` | non utilisées dans le rendu commun | Le type les hérite, mais `MultiSelectListCommon` ne les propage pas au `<ul>`. | IMPLÉMENTÉ |
| Props item natives | `Omit<ComponentProps<"input">, "type">` via `ItemMultiSelect` | selon input | `id`, `checked`, `name`, `value`, événements et autres props input sont transmis à la checkbox. | IMPLÉMENTÉ |
| `label` item | `ReactNode` | requis pour l'item | Rendu dans `span.af-item-multi-select__label`. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Alternance automatique | Index pair : `primary` | `af-item-multi-select--primary` | IMPLÉMENTÉ |
| Alternance automatique | Index impair : `secondary` | `af-item-multi-select--secondary` | IMPLÉMENTÉ |
| Variante utilisateur de liste | Aucune prop propre de variante | `af-multi-select-list` | IMPLÉMENTÉ |

L'alternance technique des items ne documente pas une règle de design sur la composition des listes.

## États et comportements

- Le composant mappe tous les `items` sans état interne de sélection.
- La sélection est portée par chaque input checkbox via les props transmises (`checked`, `defaultChecked`, `onChange`, etc.).
- La clé React de chaque `<li>` est `item.id`.
- Les items reçoivent une variante forcée par l'index ; une variante passée dans `item` serait écrasée par `variant={index % 2 === 0 ? "primary" : "secondary"}`.
- La zone devient scrollable par CSS quand le contenu dépasse la hauteur maximale.

## Anatomie

- `ul.af-multi-select-list`.
- `li` par item, sans classe spécifique.
- `label.af-item-multi-select.af-item-multi-select--primary|secondary`.
- `span.af-item-multi-select__checkbox` contenant le composant `Checkbox`.
- `span.af-item-multi-select__label` contenant le libellé.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Liste | `--item-select-height: calc(1lh + (2 * var(--size-16)))` | OBSERVÉ |
| Liste | `max-height: calc(4.75 * var(--item-select-height))` ; commentaire CSS : 5e item partiellement visible | OBSERVÉ |
| Liste | `padding: 0`, `border-radius: var(--radius-8)`, `overflow: hidden auto`, `box-shadow: 0 4px 16px -2px var(--gray-250)`, `list-style-type: none` | OBSERVÉ |
| Item | `display: inline-flex`, `width: 100%`, `padding: var(--size-16)`, `column-gap: var(--size-8)`, `cursor: pointer` | OBSERVÉ |
| Item primary | `--item-multi-select-background-color: var(--blue-040)` | OBSERVÉ |
| Item secondary | `--item-multi-select-background-color: var(--white-1000)` | OBSERVÉ |
| Label item | `font-size: var(--rem-16)`, `font-weight: 400` | OBSERVÉ |

Aucune media query n'a été relevée dans les CSS lus pour `MultiSelectList` et `ItemMultiSelect`.

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Import CSS | `@axa-fr/canopee-css/prospect/Form/MultiSelectList/MultiSelectListAll.css` | `@axa-fr/canopee-css/client/Form/MultiSelectList/MultiSelectListAll.css` | IMPLÉMENTÉ |
| Item utilisé | `ItemMultiSelectApollo` | `ItemMultiSelectLF` | IMPLÉMENTÉ |
| CSS lu | Même fichier `MultiSelectListAll.css` et mêmes valeurs d'item. | Même fichier `MultiSelectListAll.css` et mêmes valeurs d'item. | OBSERVÉ |
| Stories | Même structure, imports adaptés. | Même structure, imports adaptés. | DOCUMENTÉ |

## Accessibilité

- Chaque item est un `<label>` lié à la checkbox par `htmlFor={id}`.
- La sémantique de groupe vient du `<ul>` et des `<li>`, pas d'un rôle ARIA spécifique.
- Aucune gestion clavier propre n'est ajoutée ; elle dépend de l'input checkbox natif.
- La conformité WCAG/RGAA, le libellé de groupe et les messages d'erreur ne sont pas certifiés par ces sources.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : nombre recommandé d'options, critère d'affichage avec scroll, ordre des options, microcopy des labels.
- `NON_CONFIRMÉ` : règles de groupe de formulaire, légende et message d'aide associés.
- Vérifier dans le Storybook de la version installée l'effet visuel de la hauteur maximale et de l'alternance.
