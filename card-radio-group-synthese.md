# CardRadioGroup — Synthèse

Synthèse d'implémentation du composant `CardRadioGroup` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Groupe de cartes radio rendu en `<fieldset role="radiogroup">` avec légende, description, options et message. | IMPLÉMENTÉ / DOCUMENTÉ |
| Quand l'utiliser | Le MDX documente le pattern de groupe radio, sans règle métier d'usage. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Non présent dans les sources techniques. | NON_CONFIRMÉ |
| Cardinalité par page | Non présent dans les sources techniques. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `CardRadioGroup`.
- Exports associés : aucun type public dans `prospect.ts`/`client.ts`; `CardRadioGroupProps` reste local au module.
- Élément racine rendu et classe CSS de base : `<fieldset className="af-card-radio-group" role="radiogroup">`.
- Dépendances internes : `CardRadio`, `ItemMessage`, `useId`, `GridContainerProps`.

```tsx
import { CardRadioGroup } from "@axa-fr/canopee-react/prospect";

<CardRadioGroup
  name="city"
  label="Choose a city"
  options={[{ label: "Paris", value: "paris" }]}
/>;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `label` | `ReactNode` | — | Affiché dans `p.af-card-radio-group__label` dans la légende. | IMPLÉMENTÉ |
| `description` | `ReactNode` | — | Affiché sous le label si fourni. | IMPLÉMENTÉ |
| `options` | `RadioOption[]` | — | Liste de props `CardRadio` sans `name`, `type`, `variant`. | IMPLÉMENTÉ |
| `cardStyle` | `CardRadioProps["position"]` | — | Transmis à chaque carte via `position`. | IMPLÉMENTÉ |
| `position` | `"line" \| "column"` | `cardStyle === "vertical" ? "column" : "line"` | Ajoute `af-card-radio-group__options--${position}`. | IMPLÉMENTÉ |
| `message` | `ItemMessageProps["message"]` | — | Affiche `ItemMessage`; déclenche l'erreur si `messageType="error"`. | IMPLÉMENTÉ |
| `messageType` | `ItemMessageProps["messageType"]` | `"error"` | Transmis à `ItemMessage`; seule la valeur `error` pilote `aria-invalid`. | IMPLÉMENTÉ |
| `required` | hérité de `CardRadioProps` | — | Ajoute `aria-required`, astérisque masqué aux lecteurs et `required` aux options. | IMPLÉMENTÉ |
| `containerProps` | `GridContainerProps<"fieldset">` | — | Étendu sur le `<fieldset>` après `id` et ARIA. | IMPLÉMENTÉ |
| Props radio restantes | `CardRadioProps` moins les champs exclus dans `CardRadioGroupCommon.tsx` | — | Transmises à chaque option avant les props propres à l'option. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Disposition des options | `position="column"` | `af-card-radio-group__options--column` | IMPLÉMENTÉ |
| Disposition des options | `position="line"` | `af-card-radio-group__options--line` | IMPLÉMENTÉ |
| Style des cartes | `cardStyle="vertical" \| "horizontal"` | transmis au `CardRadio` | IMPLÉMENTÉ |
| Erreur de groupe | `message` + `messageType="error"` | options avec `variant="error"` | IMPLÉMENTÉ |

## États et comportements

`useId` génère un identifiant si `id` est absent.
`messageId` vaut `${cardRadioGroupId}-error`.
`hasError` vaut `Boolean(message) && messageType === "error"`.
Quand `hasError` est vrai, le fieldset reçoit `aria-invalid` et `aria-errormessage`, et chaque carte reçoit `variant="error"`.
La clé d'option est `${name ?? cardRadioGroupId}-${cardRadioItemProps.label}`.

## Anatomie

Structure : `fieldset.af-card-radio-group` > `legend.af-card-radio-group__legend` > label et description > `div.af-card-radio-group__options` > `CardRadio` répétés > `ItemMessage`.
Le `legend` est en `display: contents`; ses paragraphes ont `margin: 0`.
Les options ont `flex-basis: 0` et `flex-grow: 1`.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Groupe | `display: flex`, `flex-direction: column`, `gap: var(--rem-8)`. | OBSERVÉ |
| Label | `font-size: var(--rem-18)`, `font-weight: 600`, couleur `--card-radio-group-color`. | OBSERVÉ |
| Description | `var(--rem-16)` puis desktop `var(--rem-18)`, line-height assortie. | OBSERVÉ |
| Options | `display: flex`, `gap: var(--rem-16)`, desktop `var(--rem-24)`. | OBSERVÉ |
| `--column` | `flex-direction: row`. | OBSERVÉ |
| `--line` | `flex-direction: column`. | OBSERVÉ |
| Couleurs | Label `var(--gray-1000)`, description `var(--gray-800)` dans les deux univers. | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Sous-composant carte | `CardRadioApollo` | `CardRadioLF` | IMPLÉMENTÉ |
| CSS group | mêmes valeurs de `CardRadioGroupApollo.css` | mêmes valeurs de `CardRadioGroupLF.css` | OBSERVÉ |
| Stories | import depuis `@axa-fr/canopee-react/prospect` | import depuis `@axa-fr/canopee-react/client` | DOCUMENTÉ |

## Accessibilité

Le MDX documente `<fieldset role="radiogroup">` avec `<legend>`.
Le code renseigne `aria-required`, `aria-invalid` et `aria-errormessage` lorsque pertinent.
Le MDX indique : Tab dans le groupe, navigation aux flèches.
L'astérisque required est `aria-hidden`.
La conformité WCAG/RGAA n'est pas certifiée par les sources lues.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : nombre d'options recommandé, cardinalité du groupe par page, formulations de labels et descriptions.
- `NON_CONFIRMÉ` : règle de design reliant `cardStyle` à `position`; le défaut est technique.
- Vérifier dans le Storybook de la version installée : rendu `messageType="warning"` ou `validation`, ordre responsive et comportement clavier navigateur.
