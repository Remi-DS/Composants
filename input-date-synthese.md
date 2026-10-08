# InputDate — Synthèse

Synthèse d'implémentation du composant `InputDate` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Champ de saisie de date avec label, aide, messages et mode natif ou texte masqué par `hidePicker`. | DOCUMENTÉ |
| Quand l'utiliser | Non précisé hors exemples de story, dont `Date de naissance`. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Non publié dans les sources lues. | NON_CONFIRMÉ |
| Cardinalité par page | Non publiée. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `InputDate`.
- Exports associés : `itemMessageVariants` est utilisé par les stories ; `InputDateAtom` et `InputDateTextAtom` sont internes au dossier.
- Élément racine rendu et classe CSS de base : `<div className="af-form__input-container">`, puis `<input className="af-form__input-date">`.
- Dépendances internes : `ItemLabel`, `ItemMessage`, `InputDateAtom`, `InputDateTextAtom`, `formatInputDateValue`, `formatDateTextValue`.

```tsx
import { InputDate } from "@axa-fr/canopee-react/prospect";

<InputDate id="uniqueId" label="Date de naissance" name="birthDate" hidePicker={false} required />
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| props natives | `Omit<ComponentPropsWithRef<"input">, "value" \| "min" \| "max">` | natif | Transmises à l'input date ou à l'atome texte. | IMPLÉMENTÉ |
| `value`, `defaultValue` | `Date \| string` | - | Converties en `YYYY-MM-DD` si `Date`, via `toISOString().split("T")[0]`. | IMPLÉMENTÉ |
| `min`, `max` | `input["min"|"max"] \| Date` | - | Converties comme `value` sur l'input natif. | IMPLÉMENTÉ |
| `hidePicker` | `boolean` | `false` dans les stories | Bascule vers `InputDateTextAtom` avec format `jj/mm/aaaa` progressif. | IMPLÉMENTÉ |
| `label` | `ItemLabelProps["children"]` | requis | Transmis à `ItemLabel`. | IMPLÉMENTÉ |
| `helper` | `string` | - | Rendu dans `.af-form__input-helper` et relié par `aria-describedby`. | IMPLÉMENTÉ |
| `messageType` | `"error" \| "success" \| "warning"` | `"error"` | Erreur : `aria-invalid` et `aria-errormessage`; warning : modificateur CSS. | IMPLÉMENTÉ |
| `containerProps` | `GridContainerProps` | - | Étendu sur le conteneur racine. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Date native | `hidePicker` absent ou `false` | `af-form__input-date` sur `<input type="date">` | IMPLÉMENTÉ |
| Date texte | `hidePicker={true}` | `af-form__input-date` via `InputTextAtom` | IMPLÉMENTÉ |
| Warning | `messageType="warning"` avec `message` | `af-form__input-date--warning` | IMPLÉMENTÉ |

## États et comportements

Le mode natif force `type="date"`. Le mode texte force `pattern="\d{0,2}/?\d{0,2}/?\d{0,4}"`, `maxLength={10}` et `inputMode="numeric"` ; le `onChange` renvoie une valeur filtrée avec `/` après le jour et le mois. Les helpers limitent jour `1..31`, mois `1..12` et premier chiffre d'année supérieur à `0`.

## Anatomie

Structure DOM : conteneur `.af-form__input-container`, `ItemLabel`, input natif ou `InputTextAtom`, aide `.af-form__input-helper`, puis `ItemMessage`.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Base | `all: inherit`, `display:block`, `width:100%`, `padding: var(--rem-16)`, `text-transform: uppercase` | OBSERVÉ |
| Typographie | `font-size: var(--rem-16)`, `font-weight:600`, `line-height:1.5`; desktop `var(--rem-18)` et `1.25` | OBSERVÉ |
| Indicateur WebKit | `::-webkit-calendar-picker-indicator` avec icône calendrier SVG et `outline-*` | OBSERVÉ |
| Prospect | rayon `var(--radius-8)`, bordure `var(--blue-650)`, erreur `var(--red-alert-1000)` | OBSERVÉ |
| Client | rayon `var(--radius-4)`, bordure `var(--gray-800)`, erreur `var(--red-alert-1200)` | OBSERVÉ |
| Warning | `var(--orange-1050)`, largeur de box-shadow jusqu'à `3px` au hover/focus/active | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Date d'exemple remplie | `2000-09-12` dans les stories. | `2025-01-01` dans les stories. | DOCUMENTÉ |
| Couleurs | texte initial `var(--gray-800)`, valeur remplie bleue. | texte initial `var(--gray-1000)`, valeur remplie bleue. | OBSERVÉ |
| Disabled | bordure `var(--gray-140)`, texte `var(--gray-800)`, fond `var(--gray-050)`. | même disabled observé. | OBSERVÉ |

## Accessibilité

Le MDX documente que `hidePicker={true}` masque visuellement et désactive fonctionnellement le picker sur navigateurs WebKit sans impact annoncé sur l'accessibilité ; sur Firefox, le picker reste visible mais la fonctionnalité est désactivée par une astuce JavaScript, avec possibles problèmes d'accessibilité et de soumission Entrée. `aria-describedby`, `aria-errormessage` et `aria-invalid` sont gérés selon aide/message.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : règles d'usage design, format attendu par métier, validation de date réelle, cardinalité.
- `DOCUMENTÉ` : le MDX avertit explicitement des limites de `hidePicker` hors WebKit.
- Vérifier dans le Storybook de la version installée : rendu navigateur de l'input date et accessibilité du mode texte.
