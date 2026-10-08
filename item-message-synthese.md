# ItemMessage — Synthèse

Synthèse d'implémentation du composant `ItemMessage` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Afficher un message de formulaire avec icône et variante `error`, `success` ou `warning`. | DOCUMENTÉ |
| Quand l'utiliser | Les MDX indiquent l'usage comme message d'item/formulaire mais ne publient pas de règle d'arbitrage. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Non publié dans les sources lues. | NON_CONFIRMÉ |
| Cardinalité par page | Aucune cardinalité publiée. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `ItemMessage`, `itemMessageVariants`, `type ItemMessageVariants`.
- Exports associés : `itemMessageVariants = { error, success, warning } as const`.
- Élément racine rendu et classe CSS de base : `<small className="af-item-message af-item-message--{messageType}">`.
- Dépendances internes : `Icon` et icônes Material Symbols `check_circle-fill`, `error-fill`, `warning-fill`.

```tsx
import { ItemMessage } from "@axa-fr/canopee-react/prospect";

<ItemMessage message="Message" messageType="warning" />
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `message` | `ReactNode` | - | Si absent, le composant retourne `null`. | IMPLÉMENTÉ |
| `id` | `string` | - | Passé au `<small>` pour liaison ARIA par les champs parents. | IMPLÉMENTÉ |
| `messageType` | `"error" \| "success" \| "warning"` | `"error"` | Détermine la classe, la couleur, l'icône et le rôle. | IMPLÉMENTÉ |

Préciser l'héritage des props natives : le composant n'étend pas les props natives d'un élément HTML.

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Erreur | `error` | `af-item-message--error` | IMPLÉMENTÉ |
| Succès | `success` | `af-item-message--success` | IMPLÉMENTÉ |
| Warning | `warning` | `af-item-message--warning` | IMPLÉMENTÉ |

## États et comportements

`messageType="success"` utilise l'icône `check_circle-fill` et ne définit pas `role`. `warning` utilise `warning-fill`; `error` utilise `error-fill`. Les messages `error` et `warning` reçoivent `role="alert"`. Tous les messages rendus ont `aria-live="assertive"`.

## Anatomie

Structure DOM : `<small>` racine, `Icon` avec `size="XS"`, `variant={messageType}` et `aria-hidden="true"`, puis `<span className="af-item-message__message">` contenant le message.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Base | `display:flex`, `align-items:center`, `gap: var(--rem-8)` | OBSERVÉ |
| Taille | `--item-message-font-size: var(--rem-14)`, puis `var(--rem-16)` en `@media (--desktop-small)` | OBSERVÉ |
| Erreur | `--item-message-color: var(--red-alert-1000)` | OBSERVÉ |
| Succès | `--item-message-color: var(--green-1000)` | OBSERVÉ |
| Warning | `--item-message-color: var(--orange-1000)` | OBSERVÉ |
| Icône | `.af-icon` reçoit `--icon-fill: var(--item-message-color)` | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| React | Même fichier `ItemMessage.tsx`, même export. | Même fichier `ItemMessage.tsx`, même export. | IMPLÉMENTÉ |
| CSS | Fichier unique `ItemMessageAll.css`. | Fichier unique `ItemMessageAll.css`. | IMPLÉMENTÉ |
| Stories | Import depuis `@axa-fr/canopee-react/prospect`. | Import depuis `@axa-fr/canopee-react/client`. | DOCUMENTÉ |

## Accessibilité

Le composant expose `role="alert"` sauf pour le succès, et `aria-live="assertive"` sur le `<small>`. L'icône est masquée par `aria-hidden="true"`, le texte reste dans `.af-item-message__message`. La conformité WCAG/RGAA n'est pas certifiée.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : règles d'usage design, formulation du message, délais d'annonce, cardinalité.
- `DOCUMENTÉ` contradictoire : les MDX parlent d'une prop `type`, mais le code et les stories utilisent `messageType`.
- Vérifier dans le Storybook de la version installée : rendu exact des icônes et annonce lecteur d'écran selon contexte parent.
