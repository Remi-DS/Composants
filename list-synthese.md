# List — Synthèse

Synthèse d'implémentation du composant `List` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle technique | Liste HTML rendue par un `Card`, avec chaque enfant direct enveloppé dans un `<li>`. | IMPLÉMENTÉ |
| Rôle documenté | Les stories montrent une liste composée de contenus et d'éléments cliquables. | DOCUMENTÉ |
| Quand l'utiliser | Les exemples montrent `ContentItemMono` et `ClickItem`, sans règle générale d'usage. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Non présent dans les sources techniques. | NON_CONFIRMÉ |
| Cardinalité par page | Non présent dans les sources techniques. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `List`.
- Exports associés : `ListProps`.
- Exports publics associés à l'univers List : `ClickItem`, `clickItemStates`, `clickItemVariants`, `ClickItemStates`, `ClickItemVariants`, `ContentItemDuo`.
- Élément racine rendu : `Card` injecté avec `as="ul"` ou `as="ol"`.
- Dépendances internes : `CardApollo`/`CardLF`, `Children.toArray`, `generateId`.

```tsx
import { ClickItem, ContentItemMono, List } from "@axa-fr/canopee-react/prospect";

<List>
  <ContentItemMono secondaryText="nom.prénom@mail.fr">Prénom NOM</ContentItemMono>
  <ClickItem>Modifier le profil</ClickItem>
</List>;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `children` | `ReactNode` via props héritées | Aucun | Chaque enfant direct est rendu dans un `<li>`. | IMPLÉMENTÉ |
| `as` | `"ul" \| "ol"` | `"ul"` | Détermine le type de liste rendu par le `Card`. | IMPLÉMENTÉ |
| `variant` | `CardVariants` | `"default"` dans le commentaire de type | Variante visuelle transmise au `Card`. | IMPLÉMENTÉ |
| Props natives | `Omit<CardCommonProps<"ul" \| "ol">, "variant">` | Selon `Card` | Props polymorphes du `Card`, hors `variant` redéfini. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Élément HTML | `ul` | Élément racine de liste non ordonnée | IMPLÉMENTÉ |
| Élément HTML | `ol` | Élément racine de liste ordonnée | IMPLÉMENTÉ |
| Variante visuelle héritée | `default` | Classes du composant `Card` | IMPLÉMENTÉ |
| Variante visuelle héritée | `unstyled` | Classes du composant `Card`, utilisée notamment par `MenuBurger` | IMPLÉMENTÉ |

Les variantes viennent de `CardVariants`. Le choix entre `ul` et `ol` reste une règle de contenu non documentée ici.

## États et comportements

- `Children.toArray(children)` normalise les enfants.
- Chaque enfant direct devient le contenu d'un `<li>`.
- La clé de chaque `<li>` est générée par `generateId`.
- `List` ne gère pas d'état `loading`, `disabled`, `open`, `selected` ou `error`.
- Les états interactifs appartiennent aux enfants, par exemple `ClickItem`.

## Anatomie

```html
<Card as="ul|ol" class="classe-card-calculée">
  <li>premier enfant direct</li>
  <li>deuxième enfant direct</li>
</Card>
```

La classe racine exacte dépend du composant `Card` injecté. Les CSS List définissent les séparations et espacements propres aux éléments de liste.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Liste commune | Feuille `ListCommon.css` avec règles de séparation et de padding | OBSERVÉ |
| Padding desktop | `var(--rem-24)` | OBSERVÉ |
| Séparateur | `--list-item-separator-padding: 0rem` | OBSERVÉ |
| Couleur de séparation | `var(--list-item-separator-bg-color)` | OBSERVÉ |
| Responsive | `@media (--desktop-small)` | OBSERVÉ |
| Sous-composants | `ClickItem`, `ContentItemDuo` et `ContentItemDuoAction` ont leurs propres CSS | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Implémentation | `ListApollo` injecte `CardApollo` | `ListLF` injecte `CardLF` | IMPLÉMENTÉ |
| API | `ListProps` commune | `ListProps` commune | IMPLÉMENTÉ |
| CSS | `ListApollo.css` et CSS des sous-composants Apollo | `ListLF.css` et CSS des sous-composants LF | IMPLÉMENTÉ |
| Différence fonctionnelle | Aucune différence fonctionnelle documentée dans `ListCommon` | Aucune différence fonctionnelle documentée dans `ListCommon` | IMPLÉMENTÉ |

## Accessibilité

- Le composant rend un élément natif `<ul>` ou `<ol>`.
- Chaque enfant direct est placé dans un `<li>`.
- Aucun attribut ARIA spécifique n'est ajouté par `ListCommon`.
- Aucun comportement clavier ou gestion du focus n'est implémenté par `List` lui-même.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : règles de choix entre liste ordonnée et non ordonnée.
- `NON_CONFIRMÉ` : cardinalité, règles de contenu, microcopy et usage design.
- `NON_CONFIRMÉ` : classe CSS racine finale, car elle dépend du `Card` d'univers.
- `NON_CONFIRMÉ` : conformité WCAG/RGAA de la liste composée et de ses enfants.
- Vérifier dans le Storybook de la version installée : rendu des séparateurs et des sous-composants.
