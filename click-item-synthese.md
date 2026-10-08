# ClickItem — Synthèse

Synthèse d'implémentation du composant `ClickItem` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Élément interactif pour déclencher une action ; utilisable seul ou dans listes, menus ou tableaux. Source : `ClickItem.mdx`. | DOCUMENTÉ |
| Quand l'utiliser | Le MDX mentionne listes, menus ou tableaux pour des interactions rapides ; les critères design restent à confirmer. | DOCUMENTÉ / NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Le MDX indique de ne pas fournir `onClick` si un parent gère déjà le clic. | DOCUMENTÉ |
| Cardinalité par page | Non présent dans les sources techniques. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `ClickItem`, `clickItemStates`, `clickItemVariants`, types `ClickItemStates`, `ClickItemVariants`.
- Élément racine rendu et classe CSS de base : `<button>` si `onClick`, sinon `<div>`, classe `af-apollo-click-item`.
- Dépendances internes : `ClickItemWrapper`, préfixe, contenu, suffixe, `Icon`, `Spinner`, `Tag`, `BasePicture`.

```tsx
import { ClickItem } from "@axa-fr/canopee-react/prospect";

<ClickItem icon={callIcon} state="default" variant="small" title="Titre" />;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `state` | `"default" \| "disabled" \| "loading"` | `"default"` | Ajoute `af-apollo-click-item--${state}` et désactive le bouton si `disabled` ou `loading`. | IMPLÉMENTÉ |
| `variant` | `"small" \| "medium" \| "large" \| "agent"` | `"large"` | Ajoute `af-apollo-click-item--${variant}` et pilote préfixe/suffixe. | IMPLÉMENTÉ |
| `title` | `string` | — | Rendu dans `p.af-apollo-click-item__title`. | IMPLÉMENTÉ |
| `subtitle` | `string` | — | Rendu si fourni. | IMPLÉMENTÉ |
| `textSecondary` | `string` | — | Rendu si fourni ; masqué selon variantes CSS. | IMPLÉMENTÉ |
| `textTertiary` | `string` | — | Rendu si fourni ; masqué selon variantes CSS. | IMPLÉMENTÉ |
| `tagLabel` / `tagProps` | `string` / `TagProps` | — | Rend un `Tag`; en disabled, `tagProps.variant` devient `neutral`. | IMPLÉMENTÉ |
| `icon` | `string` | — | Rend une icône de préfixe sauf cas sans icône. | IMPLÉMENTÉ |
| `basePictureProps` | `ComponentProps<typeof BasePicture>` | — | Rend `BasePicture` seulement si `variant="agent"`. | IMPLÉMENTÉ |
| `onClick` | fonction | — | Si présent, racine `<button>` avec handler ; sinon racine `<div>`. | IMPLÉMENTÉ |
| `ariaLabelForActionIcon` | `string` | — | Transmis comme `aria-label` au bouton cliquable. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Taille | `large` | `af-apollo-click-item--large` | IMPLÉMENTÉ |
| Taille | `medium` | `af-apollo-click-item--medium` | IMPLÉMENTÉ |
| Taille | `small` | `af-apollo-click-item--small` | IMPLÉMENTÉ |
| Taille | `agent` | `af-apollo-click-item--agent` | IMPLÉMENTÉ |
| État | `default`, `disabled`, `loading` | `af-apollo-click-item--${state}` | IMPLÉMENTÉ |

## États et comportements

`loading` affiche un `Spinner` de taille `32` seulement pour `variant="large"`.
`small` affiche toujours une icône `keyboard_arrow_right` en suffixe.
Les autres variantes affichent un conteneur `.af-click-icon` avec flèche ; en `large` disabled, ce conteneur reçoit `af-click-icon--disabled`.
En `disabled` ou `loading`, le bouton HTML reçoit `disabled` uniquement si `onClick` existe.
Le MDX documente que sans `onClick` le composant se comporte comme un élément d'affichage.

## Anatomie

Structure : racine `button`/`div.af-apollo-click-item` > `__leading` > préfixe > `__content` > textes et tag > `__trailing` > suffixe.
Le préfixe rend `BasePicture` pour `agent` avec `basePictureProps`, sinon une `Icon` si `icon` est fourni.
Le contenu rend `title`, `subtitle`, `secondary`, `tertiary`, puis `tag-container`.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Commun | `display: flex`, `width: 100%`, `gap: var(--rem-12)`, `background: none`, `cursor: pointer`. | OBSERVÉ |
| Small | masque subtitle, secondary, tertiary, tag ; hover/focus souligne le titre. | OBSERVÉ |
| Agent | masque secondary, tertiary, tag. | OBSERVÉ |
| Medium | règle CSS imbriquée masque secondary, tertiary, trailing via `.af-apollo-click-item--medium .af-apollo-click-item`; sélecteur potentiellement inopérant tel quel. | OBSERVÉ |
| Prospect | disabled text color déclaré `var(--gray-800)`. | OBSERVÉ |
| Client | disabled text color déclaré `var(--gray-500)` ; small passe titre en `gray-1000` et poids 400. | OBSERVÉ |
| Responsive | `@media (--desktop-small)` augmente plusieurs tailles de texte. | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Suffixe principal | `arrow_forward-fill.svg` | `keyboard_arrow_right-fill.svg` | IMPLÉMENTÉ |
| Icône story | `account_balance-fill.svg` | `account_balance_wallet-fill.svg` | DOCUMENTÉ |
| Agent font-size titre | `var(--rem-18)` dès mobile | `var(--rem-16)` puis desktop `var(--rem-18)` | OBSERVÉ |
| Tertiaire large desktop | pas de valeur desktop supplémentaire observée | `--click-item-text-tertiary-font-size: var(--rem-16)` | OBSERVÉ |

## Accessibilité

Si `onClick` est fourni, la racine est un bouton natif `type="button"` via `ClickItemWrapper`.
`ariaLabelForActionIcon` devient `aria-label` du bouton ; aucun fallback n'est codé.
Les icônes décoratives reçoivent `role="presentation"`.
La conformité WCAG/RGAA n'est pas certifiée.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : cardinalité, libellés de titre/sous-titre et choix design entre variantes.
- `NON_CONFIRMÉ` : effet réel du sélecteur CSS `medium` imbriqué, à vérifier dans le Storybook.
- Vérifier dans le Storybook de la version installée : loading hors `large`, disabled sans `onClick`, et nom accessible obligatoire.
