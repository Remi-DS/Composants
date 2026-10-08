# CardMessage — Synthèse

Synthèse d'implémentation du composant `CardMessage` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS, tests et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Affiche un message textuel dans une carte colorée, avec titre optionnel. | IMPLÉMENTÉ |
| Documentation | Les MDX montrent un usage avec `title`, `text` et `variant="info"`. | DOCUMENTÉ |
| Quand l'utiliser | Non publié dans les sources lues ; vérifier le Zeroheight de l'univers. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Non publié dans les sources lues. | NON_CONFIRMÉ |
| Cardinalité par page | Non publiée dans les sources lues. | NON_CONFIRMÉ |

## Intégration React

- Package Prospect : `@axa-fr/canopee-react/prospect`, export `CardMessage`, `cardMessageVariants`, type `CardMessageVariants`.
- Package Client : `@axa-fr/canopee-react/client`, mêmes exports publics.
- Fichiers : `CardMessageCommon.tsx`, `CardMessageApollo.tsx`, `CardMessageLF.tsx`.
- Élément racine : `<div class="af-card-message af-card-message--{variant}">`.
- Dépendances internes : `getClassName`, `useMemo`.

```tsx
import { CardMessage } from "@axa-fr/canopee-react/prospect";

<CardMessage
  title="This is a title"
  text="I am the text of the card message"
  variant="info"
/>;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `variant` | `"info" \| "warning" \| "error" \| "neutral"` | `"info"` | Ajoute le modificateur `af-card-message--{variant}`. | IMPLÉMENTÉ |
| `title` | `string` | `undefined` | Rend un `<span class="af-card-message--title">` si fourni. | IMPLÉMENTÉ |
| `text` | `string` | — | Rend un `<span class="af-card-message--text">`. | IMPLÉMENTÉ |
| `className` | `string` | `undefined` | Fusionnée avec la classe racine via `getClassName`. | IMPLÉMENTÉ |
| Props natives | `ComponentPropsWithoutRef<"div">` | — | Les autres props sont transmises au `<div>`. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Information | `info` | `af-card-message--info` | IMPLÉMENTÉ |
| Avertissement | `warning` | `af-card-message--warning` | IMPLÉMENTÉ |
| Erreur | `error` | `af-card-message--error` | IMPLÉMENTÉ |
| Neutre | `neutral` | `af-card-message--neutral` | IMPLÉMENTÉ |

Le test `CardMessage.test.tsx` vérifie que chaque clé de `cardMessageVariants` ajoute la classe attendue.

## États et comportements

- Pas d'état fermé/ouvert, dismissible ou loading dans le code lu.
- `useMemo` recalcule la classe racine quand `className` ou `variant` change.
- Le titre est strictement optionnel ; le texte est requis dans le type.
- Les stories exposent `variant` avec un contrôle `select` basé sur `Object.values(cardMessageVariants)`.

## Anatomie

- Racine : `<div>` recevant les props natives restantes et `className={componentClassName}`.
- Titre optionnel : `<span class="af-card-message--title">{title}</span>`.
- Texte : `<span class="af-card-message--text">{text}</span>`.
- Aucun pictogramme, bouton de fermeture ou rôle ARIA n'est ajouté par le composant.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Commun | `display: flex`, `flex-direction: column`, `gap: var(--rem-4)`. | OBSERVÉ |
| Commun | `width: var(--card-message-width, 100%)`. | OBSERVÉ |
| Commun | `font-size: var(--card-font-size, var(--rem-14))`, puis `var(--rem-16)` à `@media (--desktop-small)`. | OBSERVÉ |
| Commun | `box-shadow: inset var(--card-message-border-width, 0) 0 var(--card-message-border-color)`. | OBSERVÉ |
| Titre/texte | `line-height: 125%`; titre `font-weight: 600`, texte `400`. | OBSERVÉ |
| Prospect base | Padding `var(--size-8) var(--size-16)`, radius `var(--radius-8)`. | OBSERVÉ |
| Client base | Padding `var(--rem-12)`, radius `var(--radius-4)`. | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Info | Fond/bord `var(--blue-040)`, texte `var(--blue-1000)`. | Fond `var(--blue-080)`, texte/bord `var(--blue-1000)`. | OBSERVÉ |
| Warning | Fond/bord `var(--red-040)`, texte `var(--orange-1000)`. | Fond `var(--orange-050)`, texte/bord `var(--orange-1050)`. | OBSERVÉ |
| Error | Fond/bord `var(--red-040)`, texte `var(--red-alert-1000)`. | Fond `var(--red-040)`, texte/bord `var(--red-alert-1200)`. | OBSERVÉ |
| Neutral | Fond/bord `var(--gray-050)`, texte `var(--gray-800)`. | Fond `var(--gray-050)`, texte/bord `var(--gray-1000)`. | OBSERVÉ |

## Accessibilité

- Le composant n'ajoute ni `role`, ni `aria-live`, ni icône.
- Les props natives de `<div>` permettent d'ajouter `role`, `aria-label` ou autres attributs ; le test utilise `aria-label="test"` pour récupérer le conteneur.
- La pertinence d'un rôle `status` ou `alert` selon variante n'est pas implémentée.
- La conformité WCAG/RGAA n'est pas certifiée par les sources lues.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : règle d'usage de chaque variante (`info`, `warning`, `error`, `neutral`).
- `NON_CONFIRMÉ` : nécessité d'un rôle ARIA ou d'une annonce live selon le contexte.
- `NON_CONFIRMÉ` : cardinalité et longueur recommandée de `title` / `text`.
- Vérifier dans le Storybook de la version installée : contraste effectif des variantes par univers.
