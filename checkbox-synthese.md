# Checkbox — Synthèse

Synthèse d'implémentation du composant `Checkbox` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Contrôle natif `<input type="checkbox">` stylé. Le MDX montre un usage simple avec `name` et `value`. | DOCUMENTÉ / IMPLÉMENTÉ |
| Quand l'utiliser | Non présent dans les sources techniques au-delà de l'exemple. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Non présent dans les sources techniques. | NON_CONFIRMÉ |
| Cardinalité par page | Non présent dans les sources techniques. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `Checkbox`.
- Exports associés : aucun type public dédié exporté par `prospect.ts`/`client.ts`.
- Élément racine rendu et classe CSS de base : `<input className="af-checkbox" type="checkbox">`.
- Dépendances internes : `getClassName`; CSS Prospect ou Client.

```tsx
import { Checkbox } from "@axa-fr/canopee-react/prospect";

const MyComponent = () => <Checkbox name="jedi" value="yoda" />;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `errorId` | `string` | — | Dépréciée ; appliquée à `aria-errormessage`. | IMPLÉMENTÉ |
| `variant` | `"error" \| "warning"` | — | Ajoute `af-checkbox--error` ou `af-checkbox--warning`; `error` met `aria-invalid`. | IMPLÉMENTÉ |
| `className` | `string` | — | Fusionnée avec `af-checkbox` et les modificateurs. | IMPLÉMENTÉ |
| `ref` | prop React | — | Transmise à l'input. | IMPLÉMENTÉ |
| Props natives | `Omit<ComponentProps<"input">, "disabled" \| "type">` | — | Transmises à l'input après ARIA initial. `type` reste forcé à `checkbox`. | IMPLÉMENTÉ |
| `checked`, `name`, `value` | props natives autorisées | — | Exposées dans les stories avec contrôles Storybook. | DOCUMENTÉ |

L'héritage natif exclut explicitement `disabled` et `type`, même si l'input HTML rendu pourrait techniquement recevoir des attributs via ordre de spread limité aux props typées.

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Défaut | `variant` absent | `af-checkbox` | IMPLÉMENTÉ |
| Erreur | `variant="error"` | `af-checkbox--error` | IMPLÉMENTÉ |
| Warning | `variant="warning"` | `af-checkbox--warning` | IMPLÉMENTÉ |

## États et comportements

L'état coché est natif ; le CSS applique une image SVG de coche via `background-image` quand `:checked`.
`variant="error"` ajoute `aria-invalid={true}` ; `warning` ne définit pas d'ARIA.
Les états `:hover`, `:focus-within` et `:checked` modifient couleur et épaisseur de bordure.
En `warning` ou `error` coché, le fond redevient blanc et la coche est supprimée dans le CSS commun.
Les stories désactivent dans la table `errorId` et `hasError`; `hasError` n'existe pas dans le type lu.

## Anatomie

Le composant rend un seul input sans label intégré.
Il doit donc être associé à un label externe ou utilisé via `CheckboxText` pour disposer d'un libellé visible.
La classe de base est produite par `getClassName`.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Dimensions | `width` et `height`: `var(--rem-24)`, `margin: 0`, `flex-shrink: 0`. | OBSERVÉ |
| Forme | `border-radius: var(--checkbox-border-radius)`, `outline` interne avec `--checkbox-border-width`. | OBSERVÉ |
| Base commune | `appearance: unset`, `cursor: pointer`, `background-color: --checkbox-background-color`. | OBSERVÉ |
| Prospect | rayon `var(--rem-6)`, bordure initiale `var(--blue-650)`, hover/checked `var(--blue-1000)` en `2px`. | OBSERVÉ |
| Client | rayon `var(--rem-4)`, bordure initiale `var(--gray-500)`, hover/checked `var(--blue-1000)` en `3px`. | OBSERVÉ |
| Warning | bordure `var(--orange-1050)`, `--checkbox-border-width: 2px`, hover/focus `3px`. | OBSERVÉ |
| Error Prospect | bordure `var(--red-alert-1000)`. | OBSERVÉ |
| Error Client | bordure `var(--red-alert-1200)`. | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| CSS importé | `CheckboxApollo.css` | `CheckboxLF.css` | IMPLÉMENTÉ |
| Rayon | `var(--rem-6)` | `var(--rem-4)` | OBSERVÉ |
| Bordure initiale | `var(--blue-650)` | `var(--gray-500)` | OBSERVÉ |
| Épaisseur hover/checked | `2px` | `3px` | OBSERVÉ |
| Rouge erreur | `var(--red-alert-1000)` | `var(--red-alert-1200)` | OBSERVÉ |

## Accessibilité

Le composant conserve la sémantique native d'un input checkbox.
`variant="error"` rend `aria-invalid`; `errorId` renseigne `aria-errormessage`.
Le MDX indique que le composant hérite de toutes les props natives de `<input type="checkbox"/>`, mais le type exclut `disabled` et `type` : contradiction conservée.
Aucun label accessible n'est ajouté par `Checkbox` seul.
La conformité WCAG/RGAA n'est pas certifiée.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : règles d'usage design, cardinalité, libellés et microcopy.
- `NON_CONFIRMÉ` : recommandation de préférer `CheckboxText` pour les cas libellés ; c'est une recommandation d'intégration, pas une règle Canopée publiée.
- Vérifier dans le Storybook de la version installée : rendu `warning` coché, focus clavier et contradiction MDX/type sur `disabled`.
