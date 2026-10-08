# CheckboxText — Synthèse

Synthèse d'implémentation du composant `CheckboxText` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Checkbox avec libellé textuel et message optionnel, composé à partir de `Checkbox` et `ItemMessage`. | IMPLÉMENTÉ |
| Quand l'utiliser | Les MDX montrent un exemple d'acceptation avec texte long ; aucune règle design n'est publiée. | DOCUMENTÉ / NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Non présent dans les sources techniques. | NON_CONFIRMÉ |
| Cardinalité par page | Non présent dans les sources techniques. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `CheckboxText`.
- Exports associés : aucun type public dédié exporté par `prospect.ts`/`client.ts`.
- Élément racine rendu et classe CSS de base : `<div className="af-checkbox-text">`.
- Dépendances internes : `Checkbox`, `ItemMessage`, `useId`, `GridContainerProps`.

```tsx
import { CheckboxText } from "@axa-fr/canopee-react/prospect";

<CheckboxText name="option1" value="option1" label="J'accepte de fournir mes coordonnées." />;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `label` | `string \| ReactNode` | — | Rendu dans `span.af-checkbox-text__label-content`. | IMPLÉMENTÉ |
| `containerProps` | `GridContainerProps` | — | Étendu sur le `div.af-checkbox-text`. | IMPLÉMENTÉ |
| `id` | prop `Checkbox` | `useId()` | Sert à `htmlFor`, à l'`id` de checkbox et à construire `${id}-error`. | IMPLÉMENTÉ |
| `message` | `ItemMessageProps["message"]` | — | Affiche un `ItemMessage`. | IMPLÉMENTÉ |
| `messageType` | `ItemMessageProps["messageType"]` | `"error"` | Pilote le rendu message ; seul `error` active les attributs ARIA d'erreur. | IMPLÉMENTÉ |
| Props checkbox | `Omit<CheckboxProps, "aria-errormessage" \| "aria-invalid">` | — | Transmises au `CheckboxComponent`. | IMPLÉMENTÉ |

Préciser l'héritage : `CheckboxTextProps` reprend les props de `Checkbox` sauf `aria-errormessage` et `aria-invalid`, afin de les gérer à partir du message.

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Message | `messageType` parmi `itemMessageVariants` | classes gérées par `ItemMessage` | IMPLÉMENTÉ |
| Checkbox interne | `variant` hérité de `Checkbox` | `af-checkbox--error` ou `af-checkbox--warning` | IMPLÉMENTÉ |
| Variante propre | Aucune variante propre observée | — | IMPLÉMENTÉ |

## États et comportements

`hasError` vaut `Boolean(message) && messageType === "error"`.
Si `hasError`, le checkbox reçoit `aria-errormessage={messageId}` et `aria-invalid={true}`.
Le label utilise `htmlFor={inputId}` mais le CSS met le `label` en `display: contents`.
Le message est toujours instancié avec `id={messageId}`, même si `message` est vide ou absent selon le comportement de `ItemMessage`.

## Anatomie

Structure : `div.af-checkbox-text` > `label htmlFor` > `CheckboxComponent` + `span.af-checkbox-text__label-content` > `ItemMessageComponent`.
Le layout CSS place la checkbox en zone `icon`, le label en zone `label`, et le message en zone `message`.
Quand un `.af-item-message` est présent, la grille passe de `"icon label"` à deux lignes `"icon label"` / `"icon message"`.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Layout | `display: grid`, colonnes `auto 1fr`, `align-items: start`, `gap: var(--rem-8)`. | OBSERVÉ |
| Label contenu | `font-size: var(--rem-16)`, `font-weight: 400`, `line-height: 1.25`. | OBSERVÉ |
| Desktop | `@media (--desktop-small)` : label `var(--rem-18)`. | OBSERVÉ |
| Couleur | `--checkbox-text-label-color: var(--black-1000)` dans les deux univers. | OBSERVÉ |
| Curseur | `label { display: contents; cursor: pointer; }`. | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Sous-composants | `CheckboxApollo`, `ItemMessage` | `CheckboxLF`, `ItemMessage` | IMPLÉMENTÉ |
| CSS | `CheckboxTextApollo.css` importe le commun | `CheckboxTextLF.css` importe le commun | IMPLÉMENTÉ |
| Couleur de label | `var(--black-1000)` | `var(--black-1000)` | OBSERVÉ |
| Stories | `apps/apollo-stories/src/components/CheckboxText` | dossier `apps/look-and-feel-stories/src/components/CheckBoxText` côté Look & Feel | OBSERVÉ |

## Accessibilité

Le `label` est associé au contrôle par `htmlFor` et `id`.
En erreur, `aria-invalid` et `aria-errormessage` sont ajoutés au checkbox.
Le libellé peut être un `ReactNode`, ce qui permet techniquement des contenus riches ; aucune règle de microcopy n'est publiée.
La conformité WCAG/RGAA n'est pas certifiée.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : règle d'usage pour textes longs de consentement, cardinalité et formulations autorisées.
- `NON_CONFIRMÉ` : comportement attendu pour `messageType="warning"` ou validation sur le plan accessibilité.
- Vérifier dans le Storybook de la version installée : rendu réel de `ItemMessage` quand `message=""` et impact de `display: contents` sur les technologies d'assistance.
