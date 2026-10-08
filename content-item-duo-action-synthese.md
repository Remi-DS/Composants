# ContentItemDuoAction — Synthèse

Synthèse d'implémentation du composant `ContentItemDuoAction` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Compose un `ContentItemMono` avec une zone d'action contenant soit des boutons, soit un `Toggle`. | IMPLÉMENTÉ |
| Quand l'utiliser | Non présent dans un MDX ; seules les stories montrent les états `edit` et `toggle`. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Non présent dans les sources techniques. | NON_CONFIRMÉ |
| Cardinalité par page | Non présent dans les sources techniques. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `ContentItemDuoAction`, type `ContentItemDuoActionState`.
- Exports associés : `ContentItemDuoActionState` est exporté comme type dans `prospect.ts`/`client.ts`; l'objet runtime local contient `edit` et `toggle`.
- Élément racine rendu et classe CSS de base : `<div className="af-content-item-duo-action">`.
- Dépendances internes : `ContentItemMono`, `Toggle`, `Button` dans les stories, `getClassName`.

```tsx
import { ContentItemDuoAction, Button } from "@axa-fr/canopee-react/prospect";

<ContentItemDuoAction
  state="edit"
  contentItemProps={{ type: "icon", title: "Texte principale" }}
  buttons={<Button variant="ghost">Modifier</Button>}
/>;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `className` | `string` | — | Fusionnée avec `af-content-item-duo-action`. | IMPLÉMENTÉ |
| `state` | `"edit" \| "toggle"` | — | Détermine le rendu de la zone action. | IMPLÉMENTÉ |
| `contentItemProps` | `ContentItemProps` | — | Transmis à `ContentItemMonoComponent`. | IMPLÉMENTÉ |
| `buttons` | `ReactElement<ComponentType<ButtonProps>>` | — | Rendu tel quel si `state="edit"`. | IMPLÉMENTÉ |
| `toggleProps` | `ToggleProps` | — | Transmis au `ToggleComponent` si `state="toggle"`. | IMPLÉMENTÉ |

Le type public est une union : en état `toggle`, `toggleProps` est disponible ; en état `edit`, `buttons` est requis par le type spécifique.

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| État action | `edit` | pas de modificateur dédié | IMPLÉMENTÉ |
| État action | `toggle` | pas de modificateur dédié | IMPLÉMENTÉ |
| Variante visuelle propre | Aucune classe de variante observée | — | IMPLÉMENTÉ |

## États et comportements

Si `state === "edit"`, le composant rend `props.buttons` dans `.af-action-container`.
Sinon, il rend `ToggleComponent` avec `toggleProps`.
Le composant ne fournit pas de boutons par défaut ; les stories passent deux boutons `ghost` : "Modifier" et "Supprimer".
Le composant ne valide pas à l'exécution l'absence de `ToggleComponent`, mais Apollo et LF l'injectent toujours.

## Anatomie

Structure : `div.af-content-item-duo-action` > `ContentItemMonoComponent` > `div.af-action-container` > boutons ou toggle.
La zone action est séparée du contenu par une marge gauche CSS.
Les props de `ContentItemMono` contrôlent le contenu affiché à gauche.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Racine | `display: flex`, `flex-direction: row`, `justify-content: space-between`. | OBSERVÉ |
| Action mobile | `.af-action-container` en colonne, `margin-left: var(--rem-12)`, `row-gap: var(--rem-16)`. | OBSERVÉ |
| Desktop | `@media (--desktop-small)` : `margin-left: var(--rem-8)`, direction row, `column-gap: var(--rem-24)`. | OBSERVÉ |
| CSS | Fichier unique `ContentItemDuoActionAll.css` importé par les deux univers. | IMPLÉMENTÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| ContentItemMono | `ContentItemMonoApollo` | `ContentItemMonoLF` | IMPLÉMENTÉ |
| Toggle | `ToggleApollo` | `ToggleLF` | IMPLÉMENTÉ |
| CSS complémentaire | importe `ContentItemDuoActionAll.css` | importe aussi `ContentItemMonoLF.css` en plus de `ContentItemDuoActionAll.css` | IMPLÉMENTÉ |
| Story `iconProps` | inclut `variant: "primary"` | n'inclut pas `variant`, mais `hasBackground` et `src` | DOCUMENTÉ |

## Accessibilité

Le composant n'ajoute pas d'attribut ARIA propre.
L'accessibilité dépend de `ContentItemMono`, des boutons fournis ou du `Toggle`.
Les boutons des stories sont des composants `Button` avec texte visible.
La conformité WCAG/RGAA n'est pas certifiée.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : règles d'usage design de l'état `edit` versus `toggle`, nombre de boutons et libellés autorisés.
- `NON_CONFIRMÉ` : aucune documentation MDX spécifique n'a été trouvée pour ce composant dans les sources listées.
- Vérifier dans le Storybook de la version installée : alignement avec plusieurs boutons, état toggle et comportement responsive.
