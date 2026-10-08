# Dropdown — Synthèse

Synthèse d'implémentation du composant `Dropdown` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Champ de sélection HTML `<select>` avec label, description, aide et message d'état. Les MDX montrent l'import et un usage `<Dropdown label="label">`. | DOCUMENTÉ |
| Quand l'utiliser | Absence de règle d'usage design dans les sources lues ; vérifier le Zeroheight Prospect ou Client. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Absence de règle d'exclusion dans les sources lues ; ne pas déduire une règle depuis la capacité technique du `<select>`. | NON_CONFIRMÉ |
| Cardinalité par page | Non indiquée dans les sources React, CSS, MDX ou stories. | NON_CONFIRMÉ |
| Contradiction conservée | Les MDX parlent d'une prop `type` avec variantes `default`, `error`, `disabled`, mais `DropdownProps` hérite de `ComponentPropsWithRef<"select">` et ne déclare pas `type`. | DOCUMENTÉ / IMPLÉMENTÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `Dropdown`.
- Exports associés : les stories importent `itemMessageVariants` pour piloter `messageType` ; aucun objet de variante propre à `Dropdown` n'est déclaré dans les fichiers lus.
- Élément racine rendu et classe CSS de base : `<div className="af-form__dropdown-container">`, puis `<select className="af-form__dropdown-input">`.
- Dépendances internes : `DropdownCommon`, `ItemLabel` Apollo/LF, `ItemMessage`, `classnames`, `useId`.

```tsx
import { Dropdown } from "@axa-fr/canopee-react/prospect";

const MyComponent = () => <Dropdown label="label" />;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| Props natives | `ComponentPropsWithRef<"select">` | natif React | Transmises au `<select>` via `{...otherProps}` ; inclut notamment `value`, `onChange`, `disabled`, `required`, `children`. | IMPLÉMENTÉ |
| `id` | `string` | `useId()` | Remplace l'identifiant généré et sert à `htmlFor` du label et à `id` du `<select>`. | IMPLÉMENTÉ |
| `label` | `ItemLabelProps["children"]` | `undefined` | Contenu enfant de `ItemLabelComponent`. | IMPLÉMENTÉ |
| `placeholder` | `string` | `undefined` | Si présent, ajoute `<option value="">{placeholder}</option>`. | IMPLÉMENTÉ |
| `description` | `string` | `undefined` | Transmis au label. | IMPLÉMENTÉ |
| `helper` | `string` | `undefined` | Si présent, rend `<span className="af-form__input-helper">`; le CSS cible aussi `.af-form__dropdown-container--input-helper`. | IMPLÉMENTÉ / OBSERVÉ |
| `message` | `ItemMessageProps["message"]` | `undefined` | Transmis à `ItemMessageComponent` et déclenche état erreur ou warning selon `messageType`. | IMPLÉMENTÉ |
| `messageType` | `ItemMessageProps["messageType"]` | `"error"` | `error` ajoute `af-form__dropdown-input--error`; `warning` ajoute `af-form__dropdown-input--warning` si pas d'erreur. | IMPLÉMENTÉ |
| `moreButtonLabel`, `onMoreButtonClick` | issus de `ItemLabelProps` | `undefined` | Transmis au label. | IMPLÉMENTÉ |
| `sideButtonLabel`, `onSideButtonClick` | issus de `ItemLabelProps` | `undefined` | Transmis au label. | IMPLÉMENTÉ |
| `containerProps` | `GridContainerProps` | `undefined` | Étendu sur le `<div>` racine. | IMPLÉMENTÉ |

Précision : l'héritage natif est `ComponentPropsWithRef<"select">`, pas un type spécifique aux options.

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Erreur | `message` présent et `messageType="error"` | `af-form__dropdown-input--error` | IMPLÉMENTÉ |
| Warning | `message` présent, sans erreur, et `messageType="warning"` | `af-form__dropdown-input--warning` | IMPLÉMENTÉ |
| Disabled | prop native `disabled` | pseudo-classe `:disabled` | IMPLÉMENTÉ |
| Placeholder sélectionné | `option[value=""]:checked` | pseudo-classe `:has(option[value=""]:checked)` | IMPLÉMENTÉ |
| `type` documenté | `default`, `error`, `disabled` | aucune prop `type` déclarée dans `DropdownProps` | DOCUMENTÉ / NON_CONFIRMÉ |

## États et comportements

Le composant calcule `hasError` et `hasWarning` depuis `message` et `messageType`. Le `<select>` reste natif : ouverture, choix option, focus et disabled relèvent du navigateur. Les CSS modifient l'épaisseur du box-shadow à `2px` sur `:focus-visible`, `:hover`, `:active`, à `3px` pour erreur/warning interactif. Les icônes SVG de fond changent avec `:open` et `:disabled`.

## Anatomie

Structure DOM : `div.af-form__dropdown-container` ; `ItemLabelComponent` avec `htmlFor`, `description`, boutons optionnels et `required` ; `select.af-form__dropdown-input` ; option placeholder facultative ; `children` options ; aide facultative `.af-form__input-helper` ; `ItemMessageComponent` avec `id` généré par `useId()`.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Commun input | `display:block`, `width:100%`, `padding:var(--rem-16)`, `border:none`, `font-family:var(--font-family-sans-serif)`, `font-size:var(--rem-16)`, `font-weight:600`, `line-height:1.5`, `appearance:none`. | OBSERVÉ |
| Commun fond | `background: var(--dropdown-background-color) var(--dropdown-background-image) no-repeat right 1rem center / 1.5rem 1.5rem`; `background-position-x: calc(100% - 1rem)`. | OBSERVÉ |
| Commun contour | `box-shadow: 0 0 0 var(--dropdown-box-shadow-width) var(--dropdown-box-shadow-color) inset`; `--dropdown-box-shadow-width:1px`. | OBSERVÉ |
| Placeholder | `font-weight:400` et couleur `--dropdown-color` si `option[value=""]:checked`. | OBSERVÉ |
| Erreur/warning | largeur `2px`, puis `3px` en interaction ; warning `--dropdown-box-shadow-color: var(--orange-1050)`. | OBSERVÉ |
| Responsive | `@media (--desktop-small)` : `font-size:var(--rem-18)`, `line-height:1.25`; aide en `var(--rem-16)`. | OBSERVÉ |

Les variables CSS proviennent des fichiers source et peuvent évoluer.

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Import CSS | `@axa-fr/canopee-css/prospect/Form/Dropdown/DropdownApollo.css` | `@axa-fr/canopee-css/client/Form/Dropdown/DropdownLF.css` | IMPLÉMENTÉ |
| Label | `ItemLabelApollo` | `ItemLabelLF` | IMPLÉMENTÉ |
| Rayon | `--dropdown-border-radius: var(--radius-8)` | `--dropdown-border-radius: var(--radius-4)` | OBSERVÉ |
| Couleur normale | Couleur non initialisée dans Apollo hors placeholder ; border `var(--blue-650)` | `--dropdown-color: var(--gray-1000)` ; border `var(--gray-800)` | OBSERVÉ |
| Erreur | `var(--red-alert-1000)` | `var(--red-alert-1200)` | OBSERVÉ |
| Disabled | border `var(--gray-500)` | border `var(--gray-140)` | OBSERVÉ |
| Stories | `value: ""`, titre `Components/Form/Dropdown/Dropdown` | `value: "Lorem ipsum"`, titre `Components/Form/Input/Dropdown` | DOCUMENTÉ |

## Accessibilité

Le label reçoit `htmlFor={inputId}` et le `<select>` reçoit le même `id`. Le composant conserve la sémantique native du `<select>`. Aucun `aria-describedby`, `aria-invalid` ou `aria-errormessage` n'est ajouté dans `DropdownCommon` pour l'aide ou le message. La conformité WCAG/RGAA n'est pas certifiée par les sources lues.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : règles d'usage, cas d'exclusion, cardinalité, microcopy et nombre d'options à vérifier dans le Zeroheight Prospect ou Client.
- `NON_CONFIRMÉ` : comportement réel de la prop `type` documentée dans les MDX, absente du type TypeScript lu.
- Vérifier dans le Storybook de la version installée : rendu de `.af-form__input-helper` vs classe CSS `.af-form__dropdown-container--input-helper`, couleurs finales des tokens et support navigateur de `:open` / `:has()`.
