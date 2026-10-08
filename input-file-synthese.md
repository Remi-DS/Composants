# InputFile — Synthèse

Synthèse d'implémentation du composant `InputFile` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Fournit une dropzone et un `<input type="file">` accessible avec label enrichi, bouton d'action et message de statut. | DOCUMENTÉ |
| Quand l'utiliser | Le MDX le présente pour téléverser un document ; pas de règle design plus précise. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Non publié. | NON_CONFIRMÉ |
| Cardinalité par page | Non publiée. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `InputFile`.
- Exports associés : aucun type exporté dans `prospect.ts`/`client.ts`.
- Élément racine rendu et classe CSS de base : `<div className="af-input-file">`.
- Dépendances internes : `ItemLabel`, `ItemMessage`, `Svg`, icône `add_circle-fill`, `getClassName`.

```tsx
import { InputFile } from "@axa-fr/canopee-react/prospect";

<InputFile id="invoice" label="Upload a document" helper="Accepted formats: PDF, JPG - Max size 5MB" />
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| props natives | `Omit<ComponentPropsWithRef<"input">, "type">` | natif | Transmises à l'`input`; `type` est forcé à `file`. | IMPLÉMENTÉ |
| `label`, `description` | `ReactNode` | - | Transmis à `ItemLabel`. | IMPLÉMENTÉ |
| `sideButtonLabel`, `onSideButtonClick`, `moreButtonLabel`, `onMoreButtonClick` | `ReactNode` / handler | - | Transmis à `ItemLabel`. | IMPLÉMENTÉ |
| `labelProps` | subset partiel de `ItemLabelProps` | - | Étendu sur `ItemLabel`. | IMPLÉMENTÉ |
| `helper` | `ReactNode` | - | Affiché dans `.af-input-file__help` et relié par `aria-describedby`. | IMPLÉMENTÉ |
| `dropzoneLabels` | `{ dropzone?: ReactNode; or?: ReactNode; button?: ReactNode }` | `Glisser et déposer un fichier`, `ou`, `Importer fichier` | Libellés de la zone et du faux bouton. | IMPLÉMENTÉ |
| `messageType` | `"error" \| "success" \| "warning"` | `"error"` | Pilote `ItemMessage`; seule l'erreur modifie la dropzone. | IMPLÉMENTÉ |
| `containerProps` | `GridContainerProps` | - | Étendu sur le conteneur racine. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| État erreur | `message` + `messageType="error"` | `af-input-file__dropzone--error` | IMPLÉMENTÉ |
| Variante design nommée | Aucune prop `variant`. | - | IMPLÉMENTÉ |

## États et comportements

L'input fichier est positionné en absolu, couvre la dropzone, reste transparent (`opacity:0`) et capte le clic. `required`, `accept`, `multiple`, `disabled` et autres props natives sont transmis. Le MDX documente que `className` et `style` s'appliquent au parent, pas à l'input interne. Les stories filtrent les labels vides pour laisser les valeurs par défaut.

## Anatomie

Structure DOM : `.af-input-file`, `ItemLabel`, `.af-input-file__container`, `.af-input-file__dropzone`, `<input type="file">`, deux spans texte dropzone/or, span bouton `.af-btn-client.af-btn-client--tertiary` avec `Svg`, aide, `ItemMessage`, puis `children`.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Racine | `row-gap: var(--rem-12)`, puis `var(--rem-16)` en `@media (--desktop-small)` | OBSERVÉ |
| Dropzone | `padding: var(--rem-16)`, `border-radius: var(--rem-8)`, `outline` dashed, `row-gap: var(--rem-4)` | OBSERVÉ |
| Desktop | padding `var(--rem-32) var(--rem-16)`, textes dropzone/or visibles, bouton centré | OBSERVÉ |
| Mobile | textes dropzone/or cachés ; bouton étiré | OBSERVÉ |
| Prospect hover/focus | bouton fond `var(--blue-1200)`, texte blanc | OBSERVÉ |
| Client hover/focus | bouton fond `var(--blue-100)` | OBSERVÉ |
| Erreur | Prospect `var(--red-alert-1000)`, Client `var(--red-alert-1200)` | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| React | `ItemLabelApollo`, `ItemMessage`. | `ItemLabelLF`, `ItemMessage`. | IMPLÉMENTÉ |
| Hover bouton | fond bleu 1200 + texte blanc. | fond bleu 100. | OBSERVÉ |
| MDX | Import Prospect. | Import Client. | DOCUMENTÉ |

## Accessibilité

Le MDX recommande un `id` explicite ou généré, la liaison `htmlFor`, `aria-describedby` pour helper/succès et `aria-errormessage`/`aria-invalid` pour erreur. L'icône du bouton est `role="presentation"`. Les boutons optionnels du label doivent avoir un nom accessible selon le MDX. Aucune conformité WCAG/RGAA n'est certifiée.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : formats et poids autorisés, nombre de fichiers, wording français définitif, comportement drag-and-drop réel.
- `DOCUMENTÉ` contradictoire : le MDX liste `messageType` comme `'error' / 'success' / 'info'`, tandis que code et stories utilisent `error/success/warning`.
- Vérifier dans le Storybook de la version installée : comportement `disabled` visuel et interaction drag/drop native.
