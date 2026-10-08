# TextArea — Synthèse

Synthèse d'implémentation du composant `TextArea` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Le composant rend un `<textarea>` natif dans un conteneur de champ avec label, aide et message optionnels. | IMPLÉMENTÉ |
| Quand l'utiliser | Non décrit dans les sources lues ; à confirmer dans le Zeroheight Prospect ou Client. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Non décrit dans les sources lues ; ne pas déduire une règle de design des props natives disponibles. | NON_CONFIRMÉ |
| Cardinalité par page | Aucune limite par page n'est publiée dans les sources lues. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `TextArea`.
- Exports associés : `itemMessageVariants` est utilisé par les stories pour `messageType`, mais `TextAreaProps` n'est pas réexporté par `prospect.ts` ou `client.ts`.
- Élément racine rendu et classe CSS de base : `div.af-form__input-container`, puis `textarea.af-form__textarea`.
- Dépendances internes : `TextAreaCommon`, `ItemLabel`, `ItemMessage`, `getClassName`, `useId`.

```tsx
import { TextArea } from "@axa-fr/canopee-react/prospect";

const MyComponent = () => <TextArea />;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| Props natives | `ComponentPropsWithRef<"textarea">` | Selon React / HTML | Toutes les props restantes sont transmises au `<textarea>`. | IMPLÉMENTÉ |
| `id` | `string` natif | `useId()` | Sert à lier `label`, `helper` et message ; généré si absent. | IMPLÉMENTÉ |
| `className` | `string` natif | `undefined` | Ajouté au conteneur `af-form__input-container`, pas au `<textarea>`. | IMPLÉMENTÉ |
| `label` | `ItemLabelProps["children"]` | Aucun | Rendu dans `ItemLabel`; si absent, `ItemLabelCommon` ne rend rien. | IMPLÉMENTÉ |
| `description` | `ReactNode` via `ItemLabelProps` | Aucun | Description associée au label dans `ItemLabel`. | IMPLÉMENTÉ |
| `helper` | `string` | Aucun | Rend `span.af-form__input-helper` et alimente `aria-describedby`. | IMPLÉMENTÉ |
| `message` | `ReactNode` via `ItemMessageProps` | Aucun | Rend `ItemMessage`; absent, aucun message n'est rendu. | IMPLÉMENTÉ |
| `messageType` | `"error" \| "success" \| "warning"` | `"error"` | Pilote le type de message ; `warning` ajoute la classe `af-form__textarea--warning` si `message` existe. | IMPLÉMENTÉ |
| `moreButtonLabel`, `onMoreButtonClick` | Props `ItemLabel` | Aucun | Affichent et pilotent le bouton d'information du label. | IMPLÉMENTÉ |
| `sideButtonLabel`, `onSideButtonClick` | Props `ItemLabel` | Aucun | Affichent et pilotent le bouton latéral du label. | IMPLÉMENTÉ |
| `containerProps` | `GridContainerProps` | Aucun | Étendu sur le conteneur racine après `className`. | IMPLÉMENTÉ |
| `placeholder` | `string` natif | `" "` | Valeur par défaut utilisée pour permettre les styles `:placeholder-shown`. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Message | `error`, `success`, `warning` | Message : `af-item-message--error`, `af-item-message--success` ou `af-item-message--warning`; textarea : `af-form__textarea--warning` pour warning avec message. | IMPLÉMENTÉ |
| Variante propre au champ | Aucune prop de variante dédiée | Aucune autre classe de variante dans `TextAreaCommon`. | IMPLÉMENTÉ |

## États et comportements

- Erreur : `Boolean(message) && messageType === "error"` ajoute `aria-invalid="true"` et `aria-errormessage="{id}-error"`.
- Warning : `Boolean(message) && messageType === "warning"` ajoute le modificateur CSS `warning`, sans `aria-invalid`.
- Helper : si `helper` existe, `aria-describedby` pointe vers `{id}-helper`.
- Required : `required` est transmis au label et au `<textarea>`.
- Disabled : transmis au `<textarea>` ; les couleurs disabled sont définies en CSS.
- `ItemMessage` rend `role="alert"` pour `error` et `warning`, pas pour `success`, avec `aria-live="assertive"`.

## Anatomie

- `div.af-form__input-container` avec `className` additionnel et `containerProps`.
- `ItemLabel` avec `htmlFor`, description et boutons optionnels.
- `textarea.af-form__textarea` avec id, placeholder, attributs ARIA et props natives.
- `span.af-form__input-helper` si `helper` est fourni.
- `ItemMessage` si `message` est fourni.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Commun | `width: 100%`, `min-height: var(--rem-72)`, `padding: var(--rem-16)`, `border: none`, `outline: none` | OBSERVÉ |
| Commun | `font-size: var(--rem-16)`, `font-weight: 600`, `line-height: 125%` | OBSERVÉ |
| Desktop `@media (--desktop-small)` | `min-height: var(--rem-80)`, `font-size: var(--rem-18)` | OBSERVÉ |
| Interaction | `--textarea-box-shadow-width` passe de `1px` à `2px`; erreur/warning focus peut passer à `3px`. | OBSERVÉ |
| Prospect | `--textarea-border-radius: var(--radius-8)`, couleur texte `var(--blue-1000)`, bordure `var(--blue-650)` puis `var(--blue-1000)`. | OBSERVÉ |
| Client | `--textarea-border-radius: var(--radius-4)`, couleur texte `var(--gray-1000)`, bordure `var(--gray-800)` puis `var(--blue-1000)`. | OBSERVÉ |
| Disabled | Fond `var(--gray-050)`, bordure `var(--gray-140)`, texte et placeholder `var(--gray-800)`. | OBSERVÉ |
| Erreur | Prospect `var(--red-alert-1000)` ; Client `var(--red-alert-1200)`. | OBSERVÉ |
| Warning | `var(--orange-1050)` dans le CSS commun. | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Sous-composant label | `ItemLabelApollo` | `ItemLabelLF` | IMPLÉMENTÉ |
| CSS | `TextAreaApollo.css` | `TextAreaLF.css` | IMPLÉMENTÉ |
| Rayon | `var(--radius-8)` | `var(--radius-4)` | OBSERVÉ |
| Couleur texte | `var(--blue-1000)` | `var(--gray-1000)` | OBSERVÉ |
| Couleur erreur | `var(--red-alert-1000)` | `var(--red-alert-1200)` | OBSERVÉ |
| Stories Client | Dossier `Textarea`, fichiers `TextArea.*`. | Dossier `Textarea`, fichiers `TextArea.*`. | DOCUMENTÉ |

## Accessibilité

- Le label est associé au champ via `htmlFor={inputId}` lorsque `label` est fourni.
- `aria-invalid` et `aria-errormessage` ne sont posés que pour les erreurs.
- `aria-describedby` ne référence que le helper ; le message non-erreur n'est pas référencé par le textarea.
- Les icônes de `ItemMessage` sont `aria-hidden="true"`.
- La conformité WCAG/RGAA n'est pas certifiée par les sources lues.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : usages recommandés, cas à éviter, longueur maximale réelle de saisie et microcopy du helper.
- `NON_CONFIRMÉ` : cardinalité de champs `TextArea` par page ou par formulaire.
- Vérifier dans le Storybook de la version installée les styles des sous-composants `ItemLabel` et `ItemMessage`.
