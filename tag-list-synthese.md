# TagList — Synthèse

Synthèse d'implémentation du composant `TagList` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Le MDX indique que `TagList` affiche une liste de composants `Tag` et remplace les tags au-delà du seuil par un tag `+N`. | DOCUMENTÉ |
| Quand l'utiliser | Non décrit dans les sources React, CSS, MDX ou stories lues ; à confirmer dans le Zeroheight Prospect ou Client. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Non décrit dans les sources lues ; ne pas déduire une règle de design de la possibilité technique d'afficher des tags. | NON_CONFIRMÉ |
| Cardinalité par page | Aucune limite par page n'est publiée dans les sources lues. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `TagList`.
- Exports associés : `type TagListProps` est exporté publiquement dans les deux univers ; `Tag` est utilisé dans les stories.
- Élément racine rendu et classe CSS de base : `<div className="af-tag-list">`.
- Dépendances internes : `TagListCommon`, `Tag` comme `OverflowTag`, `getClassName`.

```tsx
import { Tag, TagList } from "@axa-fr/canopee-react/prospect";

export const TagListComponent = () => (
  <TagList>
    <Tag>Remboursement</Tag>
    <Tag>Santé</Tag>
    <Tag>Auto</Tag>
  </TagList>
);
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `children` | `ReactNode` | Aucun | Converti avec `Children.toArray(children)` puis filtré avec `isValidElement`; les valeurs non-éléments React ne sont pas rendues. | IMPLÉMENTÉ |
| `hideThreshold` | `number` | `2` | Nombre maximum d'éléments visibles avant remplacement des éléments suivants par un tag `+N`. | IMPLÉMENTÉ |
| `className` | `string` | `""` | Ajouté à la classe racine via `getClassName`. | IMPLÉMENTÉ |
| `OverflowTag` | `ComponentType<{ children: ReactNode }>` | Injecté en interne | Prop du composant commun, omise de l'API publique par `Omit<TagListCommonProps, "OverflowTag">`. | IMPLÉMENTÉ |

`TagList` n'hérite pas de props natives HTML via `ComponentPropsWithoutRef`.

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Variante propre à `TagList` | Aucune | Aucune classe de variante n'est générée. | IMPLÉMENTÉ |

Les éventuelles variantes des `Tag` enfants relèvent du composant `Tag`, pas de `TagList`.

## États et comportements

- `TagListCommon` calcule `total`, `isOverflowing`, `visibleChildren` et `hiddenCount`.
- Si `total > hideThreshold`, seuls les `hideThreshold` premiers éléments valides sont rendus.
- Le nombre masqué est rendu par `<OverflowTag>+{hiddenCount}</OverflowTag>`.
- Si `total <= hideThreshold`, tous les éléments valides sont rendus et aucun tag `+N` n'est ajouté.
- Aucune validation de borne n'est implémentée pour `hideThreshold`; une valeur fournie par l'intégrateur est utilisée telle quelle.

## Anatomie

- Racine : `div.af-tag-list`.
- Enfants visibles : éléments React valides fournis dans `children`, typiquement des `Tag` dans les stories.
- Indicateur de débordement : composant `Tag` injecté en interne, avec un contenu texte `+N`.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Racine `.af-tag-list` | `display: flex` | OBSERVÉ |
| Organisation | `flex-flow: row wrap` | OBSERVÉ |
| Espacement | `gap: var(--rem-8)` | OBSERVÉ |
| Responsive | Aucune media query dans `TagListAll.css`. | OBSERVÉ |

Les valeurs proviennent du CSS source et peuvent évoluer avec la version du design system.

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Export public | `TagListApollo` depuis `@axa-fr/canopee-react/prospect`. | `TagListLF` depuis `@axa-fr/canopee-react/client`. | IMPLÉMENTÉ |
| CSS importé | `@axa-fr/canopee-css/prospect/TagList/TagListAll.css`. | `@axa-fr/canopee-css/client/TagList/TagListAll.css`. | IMPLÉMENTÉ |
| Logique React | Même `TagListCommon` et même injection d'un `Tag` interne. | Même `TagListCommon` et même injection d'un `Tag` interne. | IMPLÉMENTÉ |
| Styles source | Même fichier source `TagListAll.css` dans le dépôt. | Même fichier source `TagListAll.css` dans le dépôt. | OBSERVÉ |

## Accessibilité

- La racine est un `div` sans rôle ARIA spécifique.
- L'indicateur `+N` est un texte visible dans un `Tag`; aucune alternative textuelle plus explicite n'est ajoutée par `TagList`.
- Le composant ne gère pas le focus, le clavier ou des attributs `aria-*` propres.
- La conformité WCAG/RGAA n'est pas certifiée par les sources lues.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : critères d'usage, cas à éviter, nombre maximal de listes ou de tags par page.
- `NON_CONFIRMÉ` : formulation attendue des libellés de tags et du compteur `+N`.
- Vérifier dans le Storybook de la version installée le rendu réel des `Tag` enfants et du tag de débordement.
