# Radio — Synthèse

Synthèse d'implémentation du composant `Radio` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Input natif `type="radio"` stylé par la classe `af-radio`. | IMPLÉMENTÉ |
| Quand l'utiliser | Les MDX montrent l'import et un exemple `name`/`value`, sans règle de choix unique. | DOCUMENTÉ |
| Quand ne pas l'utiliser | Non publié ; consulter le Zeroheight Prospect ou Client. | NON_CONFIRMÉ |
| Cardinalité par page | Non publiée ; ne pas déduire des stories une règle de nombre d'options. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `Radio`.
- Exports associés : aucun type public dans les barrels lus.
- Élément racine rendu : `<input className="af-radio" type="radio">`.
- Dépendances internes : `getClassName`.

```tsx
import { Radio } from "@axa-fr/canopee-react/prospect";

<Radio name="name" value="value" />;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `variant` | `"error" \| "warning"` | `undefined` | Ajoute le modificateur `af-radio--error` ou `af-radio--warning`. | IMPLÉMENTÉ |
| `className` | `string` | `undefined` | Fusionné avec la classe de base. | IMPLÉMENTÉ |
| `ref` | ref input | `undefined` | Transmis à l'input. | IMPLÉMENTÉ |
| Props natives | `Omit<ComponentProps<"input">, "disabled" \| "type">` | selon input | `name`, `value`, `checked`, `onChange`, etc. sont transmis. | IMPLÉMENTÉ |
| `disabled` | exclu du type `RadioProps` | non exposé par le type | La prop est retirée du type public, même si l'input HTML la supporterait. | IMPLÉMENTÉ |
| `type` | exclu et fixé | `"radio"` | Toujours forcé à `type="radio"`. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Erreur | `variant="error"` | `af-radio--error` | IMPLÉMENTÉ |
| Avertissement | `variant="warning"` | `af-radio--warning` | IMPLÉMENTÉ |
| État courant | `checked` natif | pseudo-classe `:checked` | IMPLÉMENTÉ |

## États et comportements

- Le composant n'a pas d'état interne ; il dépend des props natives (`checked`, `defaultChecked`, `onChange`).
- `:hover`, `:focus-within` et `:checked` augmentent l'épaisseur de contour à `2px`.
- En variantes `error` et `warning`, l'état checked garde le point interne transparent selon le CSS commun, puis les couleurs sont pilotées par les CSS d'univers.
- En `error` ou `warning`, hover/focus peut monter le contour à `3px`.
- La story expose `checked` et `variant` dans les contrôles.

## Anatomie

- Un seul élément DOM : `input.af-radio`.
- Le point interne est créé par `::after`.
- Aucune balise `<label>` n'est rendue par `Radio` seul ; utiliser `RadioText` ou une association externe si nécessaire.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Commun | `width`/`height: var(--rem-24)`, `border-radius: 50%`, `appearance: unset`, `cursor: pointer` | OBSERVÉ |
| Commun | `outline: var(--radio-border-width) solid var(--radio-border-color)`, `outline-offset: calc(-1 * var(--radio-border-width))` | OBSERVÉ |
| Point interne | `--radio-ckecked-width: var(--rem-12)`, `background-color: var(--radio-ckecked-color)` | OBSERVÉ |
| Prospect | Bordure initiale `var(--blue-650)`, hover/focus/checked `var(--blue-1000)`, erreur `var(--red-alert-1000)` | OBSERVÉ |
| Client | Bordure initiale `var(--gray-800)`, hover/focus/checked `var(--blue-1000)`, erreur `var(--red-alert-1200)` | OBSERVÉ |
| Warning | Bordure `var(--orange-1050)` dans le CSS commun. | OBSERVÉ |

Aucune media query n'a été relevée dans les CSS de `Radio`.

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Import CSS | `RadioApollo.css` | `RadioLF.css` | IMPLÉMENTÉ |
| Couleur initiale | `var(--blue-650)` | `var(--gray-800)` | OBSERVÉ |
| Couleur erreur | `var(--red-alert-1000)` | `var(--red-alert-1200)` | OBSERVÉ |
| MDX et story | Même exemple, import Prospect. | Même exemple, import Client. | DOCUMENTÉ |

## Accessibilité

- Le composant conserve la sémantique native de `input type="radio"`.
- Le code ne rend pas de label ; un nom accessible doit venir d'un label externe, de `RadioText` ou d'attributs fournis.
- Aucune gestion clavier custom n'est ajoutée ; le comportement est celui du radio natif.
- La conformité WCAG/RGAA et les règles de groupement (`fieldset`, `legend`) ne sont pas certifiées par les sources.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : règles d'usage d'un groupe radio, nombre d'options, libellés et messages d'erreur.
- `NON_CONFIRMÉ` : exclusion de `disabled` par le type et politique design associée.
- Vérifier dans le Storybook de la version installée les contrastes des variantes `error` et `warning`.
