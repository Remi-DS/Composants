# MultiMessage — Synthèse

Synthèse d'implémentation du composant `MultiMessage` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Les MDX décrivent un carrousel de messages partageant la même surface. | DOCUMENTÉ |
| Quand l'utiliser | Les MDX indiquent : regrouper plusieurs notifications liées quand une seule doit être visible à la fois. | DOCUMENTÉ |
| Quand ne pas l'utiliser | Non publié dans les sources lues ; consulter le Zeroheight Prospect ou Client. | NON_CONFIRMÉ |
| Cardinalité par page | Non publiée dans les sources lues ; ne pas déduire une limite du carrousel technique. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `MultiMessage`.
- Exports associés : `MultiMessageProps`, `MultiMessageItem`, `messageVariants`, `MessageVariants`.
- Élément racine rendu : le composant `Message` de l'univers, avec classe de base `af-multi-message`.
- Dépendances internes : `Message`, `Pagination`, `MultiMessageFooter`, `clampIndex`, `getClassName`.

```tsx
import { MultiMessage } from "@axa-fr/canopee-react/prospect";

<MultiMessage
  items={[
    { variant: "information", title: "First", children: "Message information" },
    { variant: "error", title: "Second", children: "Message error" },
  ]}
/>;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `items` | `MultiMessageItem[]` | requis | Liste les messages ; si la liste est vide, le composant retourne `null`. | IMPLÉMENTÉ |
| `activeIndex` | `number` | `undefined` | Active le mode contrôlé, index zéro-based borné par `clampIndex`. | IMPLÉMENTÉ |
| `defaultActiveIndex` | `number` | `0` | Index initial non contrôlé, borné entre `0` et `total - 1`. | IMPLÉMENTÉ |
| `onChangeActive` | `(index: number) => void` | `undefined` | Appelé après navigation si l'index change, avec un index zéro-based. | IMPLÉMENTÉ |
| `iconSize` | `number` | `24` | Transmis au composant `Message`. | IMPLÉMENTÉ |
| `heading` | `"h2" \| "h3" \| "h4" \| "h5" \| "h6"` | `"h4"` | Niveau de titre transmis à chaque message. | IMPLÉMENTÉ |
| `prevLabel` / `nextLabel` | `string` | `"Message précédent"` / `"Message suivant"` | Alimente les `aria-label` des boutons de pagination. | IMPLÉMENTÉ |
| `prevButtonProps` / `nextButtonProps` | `PaginationProps["prevButtonProps"]` / `PaginationProps["nextButtonProps"]` | `undefined` | Fusionnés dans les props des boutons précédent et suivant. | IMPLÉMENTÉ |
| Props héritées | `Omit<MessageProps, "action" \| "children" \| "heading" \| "iconSize" \| "title" \| "variant">` | selon `Message` | Props transmises à la section `Message`, dont `className`. | IMPLÉMENTÉ |

`MultiMessageItem` contient `variant: MessageVariants`, `title?: string`, `children?: ReactNode`, `action?: ReactElement<typeof Link | ComponentType<ButtonProps>>`.

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Variante de message | `information`, `validation`, `warning`, `error`, `neutral` via `messageVariants` | portée par `Message`, pas par `MultiMessage` | DOCUMENTÉ |
| Variante propre `MultiMessage` | Aucune prop de variante propre | `af-multi-message` uniquement | IMPLÉMENTÉ |

## États et comportements

- Le composant peut être contrôlé (`activeIndex`) ou non contrôlé (`defaultActiveIndex`).
- `clampIndex` force un index négatif ou une liste vide à `0`, et un index trop grand à `total - 1`.
- La pagination n'est rendue que si `items.length > 1`.
- Le footer n'est pas rendu si le message n'a ni pagination ni `action`.
- Les couleurs du wrapper suivent la variante du message actif par délégation au composant `Message`.

## Anatomie

- `MessageComponent.af-multi-message` : conteneur principal, avec `variant`, `title`, `iconSize`, `heading`.
- Contenu : `item.children`.
- `div.af-multi-message__footer` : rendu si action ou plusieurs messages.
- `PaginationComponent.af-multi-message__pagination` : `numberPages`, `currentPage`, `asItem="button"`, `aria-label="Pagination des messages"`.
- `div.af-multi-message__action` : contient l'action optionnelle du message actif.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Commun | `gap: var(--multi-message-gap)` | OBSERVÉ |
| Prospect | `--multi-message-gap: var(--rem-16)` | OBSERVÉ |
| Client | `--multi-message-gap: var(--rem-12)` | OBSERVÉ |
| Footer | `display: flex`, `width: 100%`, `margin-top: var(--rem-20)`, `flex-wrap: wrap`, `align-items: flex-end`, `justify-content: space-between`, `gap: var(--rem-8)` | OBSERVÉ |
| Pagination embarquée | Le compteur reste visible et les items de page sont masqués via sélecteurs `:has`. | OBSERVÉ |
| Action | `margin-left: auto`, `justify-content: flex-end`, `gap: var(--rem-16)` | OBSERVÉ |

Les valeurs proviennent des CSS source et peuvent évoluer.

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Import CSS | `@axa-fr/canopee-css/prospect/MultiMessage/MultiMessageApollo.css` | `@axa-fr/canopee-css/client/MultiMessage/MultiMessageLF.css` | IMPLÉMENTÉ |
| Sous-composants | `MessageApollo`, `PaginationApollo` | `MessageLF`, `PaginationLF` | IMPLÉMENTÉ |
| Gap principal | `var(--rem-16)` | `var(--rem-12)` | OBSERVÉ |
| MDX | Même texte, avec import de package adapté. | Même texte, avec import de package adapté. | DOCUMENTÉ |

## Accessibilité

- La pagination interne reçoit `aria-label="Pagination des messages"`.
- Les boutons précédent/suivant reçoivent par défaut `aria-label="Message précédent"` et `aria-label="Message suivant"`.
- Les titres utilisent le niveau configuré par `heading`.
- La conformité WCAG/RGAA et l'ordre de lecture avec plusieurs messages ne sont pas certifiés par les sources lues.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : règles de non-usage, cardinalité par page, priorité entre messages, longueur des titres et microcopy.
- `NON_CONFIRMÉ` : comportement attendu si `activeIndex` ou `currentPage` est hors plage côté produit, au-delà du bornage technique.
- Vérifier dans le Storybook de la version installée les rendus hérités de `Message` et `Pagination`.
