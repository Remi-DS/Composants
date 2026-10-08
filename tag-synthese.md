# Tag — Synthèse

Synthèse d'implémentation du composant `Tag` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Afficher un libellé court dans un conteneur visuel `Tag` avec variante colorée. | IMPLÉMENTÉ |
| Quand l'utiliser | Le MDX documente l'import, le playground et les variantes ; pas de règle design d'usage. | DOCUMENTÉ / NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Aucune contre-indication publiée. | NON_CONFIRMÉ |
| Cardinalité par page | Aucune limite publiée. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `Tag`.
- Exports associés : `tagVariants`, `TagVariants`.
- Élément racine rendu et classe CSS de base : `<div className="af-tag">`.
- Dépendances internes : `getClassName`, `useMemo`.

```tsx
import { Tag } from "@axa-fr/canopee-react/prospect";

<Tag variant="success">Validé</Tag>;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `variant` | `"info" \| "success" \| "warning" \| "error" \| "neutral"` | `"info"` | Ajoute `af-tag--${variant}`. | IMPLÉMENTÉ |
| `children` | `ReactNode` via `ComponentProps<"div">` | `undefined` | Rendu dans `span.af-tag__label`. | IMPLÉMENTÉ |
| `className` | `string` | `undefined` | Ajouté via `getClassName`. | IMPLÉMENTÉ |
| Props natives | `ComponentProps<"div">` | selon HTML | Transmises au `div` racine. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Information | `info` | `af-tag--info` | IMPLÉMENTÉ |
| Succès | `success` | `af-tag--success` | IMPLÉMENTÉ |
| Avertissement | `warning` | `af-tag--warning` | IMPLÉMENTÉ |
| Erreur | `error` | `af-tag--error` | IMPLÉMENTÉ |
| Neutre | `neutral` | `af-tag--neutral` | IMPLÉMENTÉ |

## États et comportements

- Aucun état React interne.
- `useMemo` recalcule la classe quand `className` ou `variant` change.
- Les tests vérifient le rendu des enfants dans `af-tag__label` et la classe de variante.
- Les stories Apollo affichent toutes les variantes via `Object.values(tagVariants)`.

## Anatomie

- `div.af-tag.af-tag--variant`.
- `span.af-tag__label` contenant les enfants.
- Le composant ne rend pas d'icône, de bouton de fermeture ou de rôle ARIA par défaut.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Commun | `display:inline-flex`, `width:fit-content`, padding `var(--rem-2) var(--rem-8)`. | OBSERVÉ |
| Commun | Bordure `1px solid var(--tag-border-color)`, rayon `var(--tag-border-radius)`, gap `var(--rem-10)`, `cursor:default`. | OBSERVÉ |
| Label | `font-weight:600`, `font-size:var(--tag-font-size)`, `line-height:var(--tag-line-height)`. | OBSERVÉ |
| Prospect | Rayon `var(--radius-4)`, font `var(--rem-14)` puis `var(--rem-16)` en `@media (--desktop-small)`. | OBSERVÉ |
| Prospect couleurs | Fonds teintés : `blue-040`, `green-040`, `red-040`, `gray-050`; textes/bordures selon variante. | OBSERVÉ |
| Client couleurs | Fond `var(--white-1000)` pour toutes les variantes ; textes/bordures colorés. | OBSERVÉ |

Les valeurs proviennent du CSS source et peuvent évoluer.

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| CSS importé | `TagApollo.css`. | `TagLF.css`. | IMPLÉMENTÉ |
| Fond des variantes | Fonds colorés ou gris. | Fond blanc pour toutes les variantes. | OBSERVÉ |
| Erreur | Texte/bordure `var(--red-alert-1000)`. | Texte/bordure `var(--red-alert-1200)`. | OBSERVÉ |
| MDX | Documente `variant` et les exemples de toutes variantes. | Documente l'usage simple et toutes variantes. | DOCUMENTÉ |

## Accessibilité

- Le rendu est un `div` sans rôle spécifique.
- Aucun état interactif, focus ou clavier n'est implémenté.
- Le contenu textuel est disponible dans `span.af-tag__label`.
- La conformité WCAG/RGAA et les contrastes ne sont pas certifiés par les sources lues.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : signification métier exacte des variantes `info`, `success`, `warning`, `error`, `neutral`.
- `NON_CONFIRMÉ` : longueur de libellé, cardinalité et microcopy.
- Vérifier dans le Storybook de la version installée : contraste des couleurs par variante et univers.
