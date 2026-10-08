# Toggle — Synthèse

Synthèse d'implémentation du composant `Toggle` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Le composant rend un contrôle natif `input type="checkbox"` présenté comme un interrupteur visuel. | IMPLÉMENTÉ |
| Quand l'utiliser | Non décrit dans les sources lues ; à confirmer dans le Zeroheight Prospect ou Client. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Non décrit dans les sources lues ; ne pas déduire une règle de design du fait qu'un checkbox natif est utilisé. | NON_CONFIRMÉ |
| Cardinalité par page | Aucune limite par page ou par formulaire n'est publiée dans les sources lues. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `Toggle`.
- Exports associés : aucun type `ToggleProps` n'est réexporté par `prospect.ts` ou `client.ts`.
- Élément racine rendu et classe CSS de base : `<label className="af-toggle">`.
- Dépendances internes : `ToggleCommon`, `Icon` Apollo ou LF, `getClassName`, `useId`, icônes Material `check.svg` et `close.svg`.

```tsx
import { Toggle } from "@axa-fr/canopee-react/prospect";

const MyComponent = () => (
  <Toggle checked={checked} disabled={disabled} onChange={onChange} />
);
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| Props natives | `Omit<InputHTMLAttributes<HTMLInputElement>, "style" \| "type">` | Selon React / HTML | Props transmises à l'`input`; `type` et `style` sont exclus du type public. | IMPLÉMENTÉ |
| `id` | `string` natif | `useId()` | Utilisé pour `htmlFor` du label et `id` de l'input. | IMPLÉMENTÉ |
| `className` | `string` natif | Aucun | Ajouté à la racine `label.af-toggle`, pas à l'input. | IMPLÉMENTÉ |
| `checked` | `boolean` natif | Non contrôlé par défaut | Pilote l'état visuel via `:has(:checked)` si fourni ou via l'état DOM. | IMPLÉMENTÉ |
| `disabled` | `boolean` natif | `false` dans les stories | Pilote l'état visuel via `:has(:disabled)` et désactive l'input natif. | IMPLÉMENTÉ |
| `onChange` | Handler natif | Aucun | Transmis à l'input checkbox. | IMPLÉMENTÉ |
| `type` | Exclu du type | Toujours `"checkbox"` | Le composant force `type="checkbox"` après les props transmises. | IMPLÉMENTÉ |
| `style` | Exclu du type | Aucun | Non accepté par le type public selon `ToggleProps`. | IMPLÉMENTÉ |
| `ref` | Montré dans le MDX | Non implémenté explicitement | Le MDX montre `<Toggle ref={checkboxRef} />`, mais le composant n'utilise pas `forwardRef` et ne transmet pas de ref dans le code lu. | DOCUMENTÉ / IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Variante propre à `Toggle` | Aucune prop de variante | Aucune classe de variante ; les états reposent sur `:checked`, `:disabled`, `:focus-visible`. | IMPLÉMENTÉ |

## États et comportements

- L'input est toujours rendu avec `type="checkbox"`.
- État non coché : fond `var(--gray-500)`, icône close visible, icône check masquée.
- État coché : `--toggle-bg-color: var(--blue-1000)`, handle déplacé à droite, icône check visible, icône close masquée.
- État disabled : fond `var(--gray-250)` et curseur désactivé sur le label.
- État disabled + checked : fond `var(--gray-250)`.
- Focus clavier : `.af-toggle:has(:focus-visible) .af-toggle__root` reçoit un outline.

## Anatomie

- `label.af-toggle` avec `htmlFor={inputId}` et `className` additionnel.
- `div.af-toggle__root` comme piste visuelle.
- `span.af-toggle__handle` comme poignée.
- `Icon.af-toggle__icon.af-toggle__icon--check`, `aria-hidden="true"`, taille `XS`.
- `Icon.af-toggle__icon.af-toggle__icon--close`, `aria-hidden="true"`, taille `XS`.
- `input[type="checkbox"]` positionné en absolu, largeur et hauteur `1px`, opacité `0`.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Variables par défaut | `--toggle-bg-color: var(--gray-500)`, `--toggle-focus-outline-color: var(--blue-1000)`, `--toggle-border-radius: var(--radius-100)` | OBSERVÉ |
| Dimensions | `--toggle-padding: 2px`, `--toggle-handle-size: 20px`, `--toggle-width: calc(var(--toggle-handle-size) * 2 - var(--toggle-padding) * 2)`, `--toggle-height: var(--toggle-handle-size)` | OBSERVÉ |
| Handle | Fond `var(--white-1000)`, `transform: translateX(var(--toggle-handle-position)) scale(var(--toggle-handle-scale))`, `transition: 200ms` | OBSERVÉ |
| Icône | `--icon-fill: var(--toggle-bg-color)` | OBSERVÉ |
| Focus | `outline: 2px solid var(--toggle-focus-outline-color)`, `outline-offset: 2px` | OBSERVÉ |
| Responsive | Aucune media query dans les CSS Toggle. | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Sous-composant icône | `IconApollo`. | `IconLF`. | IMPLÉMENTÉ |
| CSS | `ToggleApollo.css` importe `ToggleCommon.css`. | `ToggleLF.css` importe `ToggleCommon.css`. | IMPLÉMENTÉ |
| Logique React | Même `ToggleCommon`. | Même `ToggleCommon`. | IMPLÉMENTÉ |
| Styles source | Même règles communes observées. | Même règles communes observées. | OBSERVÉ |

## Accessibilité

- La sémantique interactive vient de l'`input type="checkbox"` natif.
- Le label enveloppe le contrôle et utilise aussi `htmlFor`.
- Les icônes check et close sont `aria-hidden="true"`.
- Aucun libellé visible n'est rendu par le composant ; un nom accessible doit être fourni par props natives telles que `aria-label` ou `aria-labelledby` si aucun contexte externe ne le fournit.
- La conformité WCAG/RGAA n'est pas certifiée par les sources lues.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : règles d'usage, différence attendue entre toggle et checkbox, cardinalité et libellés associés.
- `NON_CONFIRMÉ` : exigence d'un libellé visible autour du composant ; le code ne l'impose pas.
- Vérifier dans le Storybook de la version installée le comportement réel de l'exemple MDX avec `ref`.
