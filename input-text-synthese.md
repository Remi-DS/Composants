# InputText — Synthèse

Synthèse d'implémentation du composant `InputText` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Champ de saisie texte avec libellé, aide, unité optionnelle et message d'état. Le MDX montre l'import et un exemple de saisie `value`, `label`, `placeholder`, `helper`. | DOCUMENTÉ |
| Quand l'utiliser | Non précisé comme règle d'usage dans les MDX `apps/*/InputText/InputText.mdx`. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Non publié dans les sources lues ; vérifier le Zeroheight de l'univers. | NON_CONFIRMÉ |
| Cardinalité par page | Aucune limite publiée. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `InputText` depuis `prospect.ts` et `client.ts`.
- Exports associés : `itemMessageVariants` pour les contrôles de story ; `InputTextAtom` existe comme composant atomique public distinct.
- Élément racine rendu et classe CSS de base : `<div className="af-form__input-container">`, puis `.af-form__input-atom-container` et `<input className="af-form__input-text">`.
- Dépendances internes : `ItemLabel`, `ItemMessage`, `InputTextAtom`, `GridContainerProps`.

```tsx
import { InputText } from "@axa-fr/canopee-react/prospect";

<InputText
  value="John Doe"
  label="Label"
  placeholder="Placeholder"
  helper="Informations complémentaires"
  name="name"
  id="nameid"
  required
/>
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| props natives | `ComponentProps<"input">` | natif | Transmises à `InputTextAtom`; `type` vaut `text` par défaut dans l'atome. | IMPLÉMENTÉ |
| `unit` | `ReactNode` | - | Rendu après l'`input` dans `.af-form__input-atom-container`; les stories utilisent un `Svg` euro ou un `Spinner`. | IMPLÉMENTÉ |
| `label` | `ItemLabelProps["children"]` | - | Rendu par `ItemLabel`; si absent, `ItemLabel` retourne `null`. | IMPLÉMENTÉ |
| `helper` | `string` | - | Affiché dans `<span className="af-form__input-helper">` et relié par `aria-describedby`. | IMPLÉMENTÉ |
| `description`, `moreButtonLabel`, `onMoreButtonClick`, `sideButtonLabel`, `onSideButtonClick` | repris de `ItemLabelProps` | - | Transmis à `ItemLabel`. | IMPLÉMENTÉ |
| `message` | `ReactNode` | - | Transmis à `ItemMessage`; déclenche styles error/warning/success selon `messageType`. | IMPLÉMENTÉ |
| `messageType` | `"error" \| "success" \| "warning"` | `"error"` | `error` ajoute l'état erreur, `warning` ajoute l'état warning, `success` est seulement ajouté à `aria-describedby`. | IMPLÉMENTÉ |
| `containerProps` | `GridContainerProps` | - | Étendu sur le conteneur racine. | IMPLÉMENTÉ |

Préciser l'héritage des props natives : le type inclut `ComponentProps<"input">`.

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Message erreur | `messageType="error"` avec `message` | `af-form__input-text--error` via `InputTextAtom` | IMPLÉMENTÉ |
| Message warning | `messageType="warning"` avec `message` | `af-form__input-text--warning` | IMPLÉMENTÉ |
| Variante design nommée | Aucune prop `variant` exposée. | - | IMPLÉMENTÉ |

## États et comportements

`disabled`, `required`, `placeholder`, `value`, `onChange` et autres attributs suivent l'`input` natif. `InputTextCommon` génère un `id` via `useId` si `id` n'est pas fourni. Le message erreur définit `aria-errormessage` et `aria-invalid=true` dans l'atome ; le warning n'active pas `aria-invalid`. La story `Loading` simule un chargement par `unit={<Spinner size={24} />}` et `disabled`.

## Anatomie

Structure DOM : conteneur `.af-form__input-container`, `ItemLabel`, conteneur `.af-form__input-atom-container`, `<input>` avec classe `af-form__input-text` et modificateur éventuel, unité éventuelle, aide `.af-form__input-helper`, puis `ItemMessage`.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Conteneur | `display:flex`, `flex-direction:column`, `row-gap: var(--rem-8)` | OBSERVÉ |
| Helper | `font-size: var(--rem-14)`, puis `var(--rem-16)` en `@media (--desktop-small)` ; couleur `--input-helper-color: var(--gray-800)` | OBSERVÉ |
| Atome commun | grille `auto min-content`, `padding: var(--rem-16)`, `font-weight:600`, `line-height:1.25em`, gap `var(--rem-8)` si unité | OBSERVÉ |
| Prospect | rayon `var(--radius-8)`, bordure initiale `var(--blue-650)`, texte `var(--gray-800)`, icône `var(--blue-1000)` | OBSERVÉ |
| Client | rayon `var(--radius-4)`, bordure initiale `var(--gray-800)`, texte `var(--gray-1000)`, icône `var(--gray-1000)` | OBSERVÉ |
| États | hover/focus/active augmentent l'épaisseur à `2px`; erreur/warning hover/focus/active à `3px` | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Composant React | Même `InputTextCommon`, avec `ItemLabelApollo`, `InputTextAtomApollo`. | Même `InputTextCommon`, avec `ItemLabelLF`, `InputTextAtomLF`. | IMPLÉMENTÉ |
| Rayon | `var(--radius-8)` | `var(--radius-4)` | OBSERVÉ |
| Erreur | `var(--red-alert-1000)` | `var(--red-alert-1200)` | OBSERVÉ |
| Disabled avec sidebutton | bordure `var(--gray-500)` | bordure `var(--gray-140)` | OBSERVÉ |

## Accessibilité

Le composant relie le label par `htmlFor`, associe l'aide via `aria-describedby`, associe l'erreur via `aria-errormessage`, et définit `aria-invalid` en cas d'erreur. Le focus visible est stylé par le CSS de l'atome. Aucune conformité WCAG/RGAA n'est certifiée par les sources.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : règles d'usage, longueur du libellé, microcopy, cardinalité, choix entre InputText et autres champs.
- `DOCUMENTÉ` : le MDX décrit explicitement l'état warning avec bordure orange et icône.
- Vérifier dans le Storybook de la version installée : rendu exact de l'unité, du Spinner en état disabled et du `:has()` selon navigateur.
