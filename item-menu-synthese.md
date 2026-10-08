# ItemMenu — Synthèse

Synthèse d'implémentation du composant `ItemMenu` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Élément atomique de navigation, rendu comme ancre, utilisé dans le Header pour naviguer entre plusieurs éléments selon le MDX. | DOCUMENTÉ |
| Quand l'utiliser | Le MDX indique un usage de navigation Header ; aucune règle de design plus précise n'est publiée. | DOCUMENTÉ |
| Quand ne pas l'utiliser | Non publié ; vérifier Zeroheight. | NON_CONFIRMÉ |
| Cardinalité par page | Non publiée. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `ItemMenu`, `type ItemMenuProps`.
- Exports associés : aucun objet de variante.
- Élément racine rendu et classe CSS de base : `<a className="af-item-menu">`.
- Dépendances internes : `getClassName`.

```tsx
import { ItemMenu } from "@axa-fr/canopee-react/prospect";

<ItemMenu href="#contracts" isActive>
  My contracts
</ItemMenu>
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `isActive` | `boolean` | `false` | Ajoute le modificateur `active`, donc la classe `af-item-menu--active`. | IMPLÉMENTÉ |
| `children` | `ReactNode` | - | Contenu de l'ancre. | IMPLÉMENTÉ |
| `className` | `string` | - | Fusionné avec `af-item-menu`. | IMPLÉMENTÉ |
| props natives | `ComponentPropsWithRef<"a">` | natif | `href`, `target`, `aria-*`, `ref` et autres attributs d'ancre sont transmis. | IMPLÉMENTÉ |

Préciser l'héritage des props natives : `ItemMenuProps` étend `ComponentPropsWithRef<"a">`.

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Actif | `isActive={true}` | `af-item-menu--active` | IMPLÉMENTÉ |
| Variante design nommée | Aucune prop `variant`. | - | IMPLÉMENTÉ |

## États et comportements

Le composant n'implémente pas de logique d'ouverture, de routage ou de sélection : il rend une ancre. L'état actif est piloté par la prop `isActive`. Les états `hover` et `focus-visible` sont CSS.

## Anatomie

Structure DOM : un seul `<a>` avec classe de base, modificateur éventuel et contenu enfant. Aucun sous-élément n'est ajouté par le composant.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Base | `display:inline-flex`, `width:100%`, `padding: var(--rem-16)`, `align-items:center` | OBSERVÉ |
| Typographie | `font-size: var(--rem-16)`, `line-height: var(--rem-20)`, poids `var(--item-menu-font-weight, 400)` | OBSERVÉ |
| Bordure mobile | `box-shadow: inset var(--item-menu-border-width, 1px) 0 0 0 var(--item-menu-border-color, var(--blue-200))` | OBSERVÉ |
| Hover | `--item-menu-border-width: 4px` | OBSERVÉ |
| Focus | `outline: 2px solid var(--blue-650)` ; desktop ajoute `outline-offset: 3px` | OBSERVÉ |
| Desktop | `height:100%`, bordure horizontale via `box-shadow: inset 0 var(--item-menu-border-width, -1px) 0 0 var(--item-menu-border-color, var(--blue-200))` | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| React | Même `ItemMenuCommon`. | Même `ItemMenuCommon`. | IMPLÉMENTÉ |
| Texte | Couleur par défaut `var(--blue-1000)`. | `--item-menu-text-color: var(--gray-1000)`. | OBSERVÉ |
| CSS Apollo | Importe seulement `ItemMenuCommon.css`. | Importe `ItemMenuCommon.css` et surcharge la couleur texte. | OBSERVÉ |

## Accessibilité

La sémantique est celle de l'ancre native. `focus-visible` est stylé. Aucun `aria-current` n'est ajouté automatiquement pour `isActive`; si requis par le contexte de navigation, ce point est à vérifier côté intégration.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : cardinalité, niveau de menu, relation avec l'URL courante, libellés autorisés.
- `DOCUMENTÉ` : les stories portent le titre `Components/ItemMenu 🚧`, indiquant un statut visuel de story en chantier sans autre règle.
- Vérifier dans le Storybook de la version installée : rendu dans le Header et comportement avec routeur applicatif.
