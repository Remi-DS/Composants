# SkeletonList — Synthèse

Synthèse d'implémentation du composant `SkeletonList` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Afficher des groupes de lignes `SkeletonGrid` dans des `List` pendant un chargement, puis rendre les `children` hors chargement. | IMPLÉMENTÉ |
| Quand l'utiliser | Présenté en story comme remplacement temporaire d'une liste d'informations pendant `isLoading=true`. Règle d'usage design à confirmer dans Zeroheight. | DOCUMENTÉ / NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Aucune interdiction d'usage n'est publiée dans les MDX lus. | NON_CONFIRMÉ |
| Cardinalité par page | Aucune limite de nombre d'instances n'est publiée. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `SkeletonList`.
- Exports associés : `SkeletonListProps`.
- Élément racine rendu et classe CSS de base : aucun wrapper propre ; le rendu est une suite de `ListComponent` ou les `children`.
- Dépendances internes : `SkeletonGrid`, `List` Apollo ou `List` Look & Feel.

```tsx
import { SkeletonList } from "@axa-fr/canopee-react/prospect";

<SkeletonList lists={lists} isLoading>
  <List>{/* contenu réel */}</List>
</SkeletonList>;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `lists` | `{ lines?: number; grid: SkeletonGridProps["grid"] }[]` | `[]` dans le destructuring | Définit les groupes de placeholders à rendre. | IMPLÉMENTÉ |
| `lists[].lines` | `number` | `1` | Répète le même `SkeletonGrid` autant de fois dans la `List`. | IMPLÉMENTÉ |
| `lists[].grid` | `SkeletonGridProps["grid"]` | requis par type | Transmis tel quel à `SkeletonGrid`. | IMPLÉMENTÉ |
| `isLoading` | `boolean` | aucun défaut explicite ; falsy si absent | Si truthy, rend les skeletons ; sinon rend `children`. | IMPLÉMENTÉ |
| `className` | `string` | `undefined` | Ajouté à chaque `ListComponent` injecté, pas à un wrapper global. | IMPLÉMENTÉ |
| `children` | `ReactNode` via `PropsWithChildren` | `undefined` | Rendu uniquement lorsque `isLoading` est falsy. | IMPLÉMENTÉ |

Aucun héritage de props natives n'est déclaré ; le type compose uniquement `PropsWithChildren`.

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Univers | `SkeletonListApollo` / `SkeletonListLF` | classes du `List` injecté | IMPLÉMENTÉ |

Le composant n'expose pas de prop de variante propre.

## États et comportements

- `isLoading=true` : `lists.map` crée un `ListComponent` par groupe, puis `Array(lines)` crée les lignes de `SkeletonGrid`.
- `isLoading=false` ou absent : les `children` sont renvoyés sans wrapper supplémentaire.
- Les clés React utilisent les index de groupe et de ligne ; le fichier désactive `react/no-array-index-key`.
- Les stories basculent `isLoading` toutes les 5 secondes avec `setInterval`; ce comportement appartient à la démonstration.

## Anatomie

- Rendu chargé : `ListComponent` > une ou plusieurs instances `SkeletonGrid`.
- Rendu non chargé : contenu libre fourni par l'intégrateur, par exemple `List`, `ContentItemDuo`, `ContentItemMono` dans les stories.
- Aucune classe `af-skeleton-list` n'est produite par `SkeletonListCommon`.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| CSS dédié | Aucun fichier CSS `SkeletonList` dans l'index local ; le rendu dépend des styles de `List` et `SkeletonGrid`. | OBSERVÉ |
| Story | `.skeleton-list-page` en `display: grid`, `grid-template-columns: 1fr`, `row-gap: 2rem`. | OBSERVÉ |
| Responsive | Aucune media query dédiée à `SkeletonList` dans les fichiers lus. | OBSERVÉ |

Les valeurs proviennent du CSS source de story et peuvent évoluer.

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Composant commun | `SkeletonListCommon` avec `ListApollo`. | `SkeletonListCommon` avec `ListLF`. | IMPLÉMENTÉ |
| Import story | `@axa-fr/canopee-react/prospect`. | `@axa-fr/canopee-react/client`. | DOCUMENTÉ |
| CSS dédié | Aucun fichier dédié dans l'index. | Aucun fichier dédié dans l'index. | OBSERVÉ |

## Accessibilité

- Aucun attribut ARIA, rôle ou gestion clavier n'est ajouté par `SkeletonListCommon`.
- La sémantique accessible dépend du `ListComponent`, de `SkeletonGrid` et du contenu réel fourni.
- La conformité WCAG/RGAA n'est pas certifiée par les sources lues.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : règles de choix entre skeleton liste et autre indicateur de chargement.
- `NON_CONFIRMÉ` : durée maximale d'affichage, cardinalité et microcopy associée au chargement.
- Vérifier dans le Storybook de la version installée : rendu exact des `List` et `SkeletonGrid` par univers.
