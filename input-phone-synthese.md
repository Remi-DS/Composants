# InputPhone — Synthèse

Synthèse d'implémentation du composant `InputPhone` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Champ téléphone basé sur `InputTextAtom`, avec label, aide, message et sélecteur indicatif optionnel. | IMPLÉMENTÉ |
| Quand l'utiliser | Les stories montrent les cas avec et sans sélecteur ; aucune règle de choix n'est publiée. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Non publié dans les sources lues. | NON_CONFIRMÉ |
| Cardinalité par page | Non publiée. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `InputPhone`, `type OptionType`.
- Exports associés : `OptionType = { flag: string; code: string }`.
- Élément racine rendu et classe CSS de base : `<div className="af-form__input-phone-container">`.
- Dépendances internes : `ItemLabel`, `ItemMessage`, `InputTextAtom`, `Icon`, `CountryCodeSelect`, `react-select`.

```tsx
import { InputPhone, type OptionType } from "@axa-fr/canopee-react/prospect";

<InputPhone label="Label" showSelect countryCodeOptions={flagsList} placeholder="07 89 10 11 12" />
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| props natives | `ComponentPropsWithRef<"input">` | natif | Transmises à l'input texte, mais `type` est forcé à `tel`. | IMPLÉMENTÉ |
| `helper` | `string` | - | Affiché sous le champ et relié par `aria-describedby`. | IMPLÉMENTÉ |
| `defaultCountry` | `string` | `"+33"` si absent dans `CountryCodeSelect` | Recherche l'option sélectionnée initiale par code. | IMPLÉMENTÉ |
| `showSelect` | `boolean` | `false` dans les stories | Affiche ou non le sélecteur d'indicatif. | IMPLÉMENTÉ |
| `countryCodeOptions` | `OptionType[]` | `[]` | Options du `react-select`. | IMPLÉMENTÉ |
| `onChangeSelect` | `(value: SingleValue<OptionType>) => void` | - | Appelé quand une option non nulle est choisie. | IMPLÉMENTÉ |
| `onChangeInput` | `(value: string) => void` | - | Reçoit la valeur masquée, pas l'événement natif. | IMPLÉMENTÉ |
| `mask` | `(value: string) => string` | `maskFrenchPhoneNumber` | Nettoie/formate la saisie avant `onChangeInput`. | IMPLÉMENTÉ |
| `messageType` | `"error" \| "success" \| "warning"` | `"error"` | Pilote erreur/warning et `ItemMessage`. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Sans indicatif | `showSelect={false}` | `.af-form__input-phone-fields` avec seul input | IMPLÉMENTÉ |
| Avec indicatif | `showSelect={true}` | `.af-form__country-code-wrapper` + `react-select` | IMPLÉMENTÉ |
| Warning/erreur | `messageType` + `message` | Classes de l'`InputTextAtom` interne | IMPLÉMENTÉ |

## États et comportements

La fonction par défaut `maskFrenchPhoneNumber` supprime les non-chiffres, limite à 10 chiffres et formate par groupes de 2 séparés par espace. `disabled` désactive l'input et le sélecteur. `CountryCodeSelect` masque l'indicateur et le séparateur natifs de `react-select`, stocke l'option courante en state et expose `aria-label="Select country code"`.

## Anatomie

Structure DOM : conteneur `.af-form__input-phone-container`, `ItemLabel`, wrapper `.af-form__input-phone-fields`, éventuellement `.af-form__country-code-wrapper` avec `Select`, puis `InputTextAtom` classé `.af-form__input-phone`, aide `.af-form__input-phone-helper`, `ItemMessage`.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Champs | `display:flex`, `width:100%`, `gap: var(--rem-8)` | OBSERVÉ |
| Conteneur | `display:flex`, `flex-direction:column`, `row-gap: var(--rem-8)` | OBSERVÉ |
| Helper | `var(--rem-14)` puis `var(--rem-16)` en `@media (--desktop-small)`, couleur `var(--gray-800)` | OBSERVÉ |
| Select commun | largeur `7.4rem`, padding `var(--rem-11) 0`, font `var(--rem-16)` puis `var(--rem-18)` | OBSERVÉ |
| Prospect select | bordure `var(--blue-650)`, rayon `var(--radius-8)`, chevron SVG Material | OBSERVÉ |
| Client select | bordure `var(--gray-800)`, rayon `var(--radius-4)`, chevron polygon SVG | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Composants injectés | `ItemLabelApollo`, `InputTextAtomApollo`, `IconApollo`. | `ItemLabelLF`, `InputTextAtomLF`, `IconLF`. | IMPLÉMENTÉ |
| `FlagUtils` stories | Type `OptionType[]` importé depuis Prospect. | Tableau non typé explicitement. | DOCUMENTÉ |
| CSS select | Rayon `8`, bordure bleue initiale. | Rayon `4`, bordure grise initiale. | OBSERVÉ |

## Accessibilité

Le label est associé par `htmlFor`. L'aide et les succès alimentent `aria-describedby`; l'erreur définit `idMessage` transmis à `InputTextAtom`. Le sélecteur a `aria-label="Select country code"`. Aucune conformité WCAG/RGAA n'est certifiée.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : pays autorisés, format international attendu, usage de `defaultCountry`, règle de choix avec/sans sélecteur.
- `IMPLÉMENTÉ` : le `onChange` natif passé en prop n'est pas appelé par `handleChangeNumber`; la valeur masquée passe par `onChangeInput`.
- Vérifier dans le Storybook de la version installée : rendu de `react-select`, annonces lecteur d'écran et comportement avec masques personnalisés.
