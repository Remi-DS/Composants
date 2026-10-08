# CardCheckbox — Synthèse

Synthèse d'implémentation du composant `CardCheckbox` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Groupe de cases à cocher présentées en cartes ou en mode texte. | IMPLÉMENTÉ / DOCUMENTÉ |
| Types | Le MDX documente deux types : `vertical` par défaut et `horizontal`. | DOCUMENTÉ |
| Mode texte | Le MDX indique que `mode="text"` utilise `CheckboxText` pour les options. | DOCUMENTÉ / IMPLÉMENTÉ |
| Quand l'utiliser | Non publié au-delà des exemples ; vérifier le Zeroheight de l'univers. | NON_CONFIRMÉ |
| Cardinalité par page | Non publiée dans les sources lues. | NON_CONFIRMÉ |

## Intégration React

- Package Prospect : `@axa-fr/canopee-react/prospect`, export `CardCheckbox`, type `CardCheckboxProps`.
- Package Client : `@axa-fr/canopee-react/client`, mêmes exports publics.
- Fichiers : `CardCheckboxCommon.tsx`, `CardCheckboxApollo.tsx`, `CardCheckboxLF.tsx`.
- Élément racine : `<fieldset class="af-card-checkbox">`.
- Dépendances internes : `CardCheckboxOption`, `CheckboxText`, `ItemMessage`.

```tsx
import { CardCheckbox } from "@axa-fr/canopee-react/prospect";

<CardCheckbox
  name="city"
  label="Choisissez des villes"
  options={[{ label: "Paris", value: "paris" }]}
/>;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `type` | `"vertical" \| "horizontal"` | `"vertical"` | Ajoute `af-card-checkbox__options--{type}` et transmis aux options. | IMPLÉMENTÉ |
| `mode` | `"text"` | `undefined` | Sélectionne `CheckboxText` au lieu de `CardCheckboxOption`. | IMPLÉMENTÉ |
| `label` | `ReactNode` | — | Rendu dans la légende, avec astérisque si `required`. | IMPLÉMENTÉ |
| `description` | `ReactNode` | `undefined` | Rendu sous le label dans la légende. | IMPLÉMENTÉ |
| `options` | `CheckboxOption[]` | — | Liste d'options, soit `Omit<CardCheckboxOptionProps, "name" \| "type">`. | IMPLÉMENTÉ |
| `containerProps` | `GridContainerProps<"fieldset">` | `undefined` | Props transmises au `<fieldset>` après `id` et `className`. | IMPLÉMENTÉ |
| `message` | `ReactNode` | `undefined` | Transmis à `ItemMessage`. | IMPLÉMENTÉ |
| `messageType` | `"error" \| "success" \| "warning"` | `"error"` | Pilote l'affichage de message et l'état d'erreur. | IMPLÉMENTÉ |
| Props héritées | Card option sans `value`, `label`, `type`, `icon`, `description`, `subtitle`, `children` | — | Inclut notamment `name`, `id`, `required`, `onChange`, attributs d'input non désactivés. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Vertical | `type="vertical"` | `af-card-checkbox__options--vertical` | IMPLÉMENTÉ |
| Horizontal | `type="horizontal"` | `af-card-checkbox__options--horizontal` | IMPLÉMENTÉ |
| Mode cartes | `mode` absent | Options `CardCheckboxOption` | IMPLÉMENTÉ |
| Mode texte | `mode="text"` | Options `CheckboxText` | IMPLÉMENTÉ |

## États et comportements

- `useId()` génère un id de groupe si `id` n'est pas fourni.
- Chaque option reçoit `id="${cardCheckboxId}-${value}"`, `name`, `required`, `onChange`, props communes et props d'option.
- Si `required` est vrai, `handleChange` compte les checkboxes cochées ; si aucune n'est cochée après décoche, il remet `required="true"` sur toutes les options, sinon il le retire.
- `hasError` vaut vrai seulement si `message` est présent et `messageType === "error"`.
- En erreur, les options reçoivent `aria-invalid` et `aria-errormessage` pointant vers `${id}-error`.

## Anatomie

- `<fieldset class="af-card-checkbox" id={cardCheckboxId}>`.
- `<legend class="af-card-checkbox__legend">` contenant `af-card-checkbox__label` et description optionnelle.
- `<div class="af-card-checkbox__options af-card-checkbox__options--vertical|horizontal">`.
- Options : composants `CardCheckboxOption` ou `CheckboxText`.
- Message : `ItemMessage` avec id `${cardCheckboxId}-error`.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Fieldset | `display: flex`, `flex-direction: column`, `gap: var(--rem-8)`. | OBSERVÉ |
| Legend | `display: contents`; les `<p>` enfants ont `margin: 0`. | OBSERVÉ |
| Label | Taille `var(--rem-18)`, graisse `600`, couleur `--card-checkbox-color`. | OBSERVÉ |
| Description | Taille `var(--rem-16)`, puis `var(--rem-18)` à `@media (--desktop-small)`. | OBSERVÉ |
| Options | `display: flex`, `gap: 1rem`; les enfants grandissent avec `flex-grow: 1`. | OBSERVÉ |
| Orientation | Vertical = `column`, horizontal = `row`. | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Dépendances | `CardCheckboxOptionApollo`, `CheckboxTextApollo`. | `CardCheckboxOptionLF`, `CheckboxTextLF`. | IMPLÉMENTÉ |
| Couleurs groupe | Label `var(--gray-1000)`, description `var(--gray-800)`. | Même valeurs dans le CSS LF lu. | OBSERVÉ |
| Story LF | Le fichier story LF importe `CardCheckbox` depuis `@axa-fr/canopee-react/prospect`, malgré le MDX client. | OBSERVÉ |

## Accessibilité

- Groupe structuré par `<fieldset>` et `<legend>`.
- L'astérisque requis est `aria-hidden`.
- Les erreurs utilisent `aria-invalid`, `aria-errormessage`, et `ItemMessage` rend `role="alert"` sauf en succès.
- La gestion requise multi-checkbox est implémentée en manipulant les attributs `required` des inputs.
- La conformité WCAG/RGAA n'est pas certifiée par les sources lues.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : nombre maximal d'options, règles d'orientation vertical/horizontal et microcopy du label.
- `NON_CONFIRMÉ` : différence voulue ou erreur d'import dans la story Look & Feel.
- Vérifier dans le Storybook de la version installée : comportement requis après cochage/décochage et rendu du mode `text`.
