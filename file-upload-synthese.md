# FileUpload — Synthèse

Synthèse d'implémentation du composant `FileUpload` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Les MDX indiquent que `FileUpload` fournit une dropzone et un contrôle fichier accessible similaire à `InputFile`, avec une zone dédiée aux fichiers importés via des `ItemFile` en `children`. | DOCUMENTÉ |
| Quand l'utiliser | Les sources montrent des exemples de justificatif et pièces jointes, mais ne publient pas de règle design d'usage ; vérifier Zeroheight. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Non indiqué dans les sources lues ; ne pas déduire une règle depuis `accept`, `multiple` ou `children`. | NON_CONFIRMÉ |
| Cardinalité par page | Non indiquée dans les sources React, CSS, MDX ou stories. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `FileUpload`; sous-composants utilisés dans les sources : `InputFile`, `ItemFile`.
- Exports associés : `ItemFile` est importé avec `FileUpload` dans les stories et MDX ; `FileUploadProps`, `InputFileProps`, `ItemFileProps` sont exportés dans les fichiers de composant.
- Élément racine rendu et classe CSS de base : `InputFileCommon` rend `<div className="af-input-file af-file-upload">` pour `FileUpload`.
- Dépendances internes : `FileUploadCommon`, `InputFileCommon`, `ItemFileCommon`, `ItemLabel`, `ItemMessage`, `Svg`, `ClickIcon`, `Icon`, `Spinner`, `ContentItemMonoCore`, `getClassName`, `generateId`.

```tsx
import { FileUpload, ItemFile } from "@axa-fr/canopee-react/prospect";

<FileUpload id="attachments" label="Attach supporting documents" fileListLabel="Your uploaded files:">
  <ItemFile file={new File([new Uint8Array(1024)], "example.jpg", { type: "image/jpg" })} />
</FileUpload>;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `FileUpload.children` | `ReactElement<ItemFileProps> \| ReactElement<ItemFileProps>[]` | `undefined` | Si présent, rend le titre de liste et un `<ul className="af-file-list">` ; chaque enfant est dans `<li className="af-file-list__item">`. | IMPLÉMENTÉ |
| `fileListLabel` | `ReactNode` | `"Vos fichiers importés :"` | Libellé au-dessus de la liste ; non rendu sans `children`. | IMPLÉMENTÉ |
| Props héritées | `Omit<InputFileProps, "children">` | selon `InputFile` | `FileUpload` hérite du contrôle fichier et force le contenu enfant à la liste de fichiers. | IMPLÉMENTÉ |
| `InputFile` natif | `Omit<ComponentPropsWithRef<"input">, "type">` | natif React | Transmis à `<input type="file">`; le `type` est forcé à `file`. | IMPLÉMENTÉ |
| `label`, `description`, `helper` | `ReactNode` | `undefined` | Label, description et aide sous la dropzone. | IMPLÉMENTÉ |
| `sideButtonLabel`, `onSideButtonClick`, `moreButtonLabel`, `onMoreButtonClick` | `ReactNode` / `MouseEventHandler<HTMLButtonElement>` | `undefined` | Transmis à `ItemLabelComponent`. | IMPLÉMENTÉ |
| `labelProps` | `Partial<Omit<ItemLabelProps, "children" \| "label" \| "description" \| "sideButtonLabel" \| "onSideButtonClick" \| "moreButtonLabel" \| "onMoreButtonClick">>` | `undefined` | Étendu sur `ItemLabelComponent` après les props principales. | IMPLÉMENTÉ |
| `dropzoneLabels` | `{ dropzone?: ReactNode; or?: ReactNode; button?: ReactNode }` | `{ dropzone:"Glisser et déposer un fichier", or:"ou", button:"Importer fichier" }` | Remplace les trois textes de la dropzone, avec fallback champ par champ. | IMPLÉMENTÉ |
| `message`, `messageType` | `ItemMessageProps` sans `id` | `messageType="error"` | Message sous la dropzone ; erreur si `message && messageType === "error"`. | IMPLÉMENTÉ |
| `ItemFile.file` | `File` | requis | Nom et taille du fichier affichés via `ContentItemMonoCore`. | IMPLÉMENTÉ |
| `ItemFile.isLoading` | `boolean` | `undefined` / faux | Affiche un `Spinner size={24}` au lieu de l'icône de validation si pas d'erreur. | IMPLÉMENTÉ |
| `ItemFile.errorMessage` | `string` | `undefined` | Passe l'item en erreur et affiche `ItemMessage`. | IMPLÉMENTÉ |
| `onRemove`, `onPreview` | `(file, event) => void` | `() => {}` | Appelés par les boutons d'action avec le fichier et l'événement. | IMPLÉMENTÉ |
| `previewProps`, `removeProps` | `Partial<Omit<ClickIconProps, "src" \| "onClick">>` | `{}` | Étendus sur les boutons d'icône. | IMPLÉMENTÉ |
| `ItemFile` section props | `Omit<ComponentPropsWithoutRef<"section">, "children">` | natif React | Étendu sur `<section className="af-item-file">`. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| FileUpload avec liste | `children` présent | `af-file-list__title`, `af-file-list`, `af-file-list__item` | IMPLÉMENTÉ |
| Dropzone erreur | `message` + `messageType="error"` | `af-input-file__dropzone--error` | IMPLÉMENTÉ |
| ItemFile erreur | `errorMessage` présent | `af-item-file--error` | IMPLÉMENTÉ |
| ItemFile chargement | `isLoading && !errorMessage` | pas de modificateur ; `aria-busy=true` et Spinner | IMPLÉMENTÉ |
| Success / warning InputFile | `messageType` peut valoir `success` ou `warning` dans les stories | Pas de modificateur dropzone dédié hors erreur ; `success` peut alimenter `aria-describedby`. | IMPLÉMENTÉ / DOCUMENTÉ |

## États et comportements

`InputFileCommon` génère `inputId`, `messageId` et `helpId`. Il construit `aria-describedby` avec l'aide et uniquement le message de succès. `hasError` ajoute `aria-invalid`, `aria-errormessage` et le modificateur de dropzone. `ItemFileCommon` calcule `hasError`, affiche validation ou erreur via `Icon`, spinner pendant chargement sans erreur, masque preview pendant chargement ou erreur, garde remove visible, et convertit la taille en `Ko`, `Mo`, `Go` par base 1000 avec minimum `0.1`.

## Anatomie

Structure `FileUpload` : `div.af-input-file.af-file-upload` ; `ItemLabelComponent` ; `div.af-input-file__container` ; `span.af-input-file__dropzone` ; `input[type="file"]` invisible couvrant la dropzone ; deux spans texte ; span bouton `af-btn-client af-btn-client--tertiary` avec `Svg` add-circle ; aide ; `ItemMessage`; puis liste facultative. Structure `ItemFile` : `section.af-item-file` ; `div.af-item-file__body` ; `ContentItemMonoCore` ; `div.af-item-file__actions` ; boutons `ClickIcon`; message d'item.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| FileUpload liste | Titre `font-size:var(--rem-18)`, `font-weight:600`, `line-height:125%`; liste et items `display:contents`; item étiré. | OBSERVÉ |
| InputFile conteneur | `display:flex`, colonne, `row-gap:var(--rem-12)`, puis `var(--rem-16)` en `@media (--desktop-small)`. | OBSERVÉ |
| Dropzone commune | `padding:var(--rem-16)`, `border-radius:var(--rem-8)`, outline dashed, `row-gap:var(--rem-4)`, input absolu opaque `0`. | OBSERVÉ |
| Dropzone desktop | `@media (--desktop-small)` : `padding:var(--rem-32) var(--rem-16)`, textes affichés, bouton centré avec `margin-top:var(--rem-4)`. | OBSERVÉ |
| Focus dropzone | hover/focus input : `--input-file-dropzone-border-width:2px`; focus-visible : outline bouton `2px` offset `3px`. | OBSERVÉ |
| ItemFile commun | Body flex, padding variables, border `1px solid`, radius variable, gap `var(--rem-12)`, actions gap variable. | OBSERVÉ |

Les variables CSS (`--input-file-*`, `--item-file-*`, `--button-*`, `--icon-fill`) proviennent des CSS source et peuvent évoluer.

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Import CSS FileUpload | `@axa-fr/canopee-css/prospect/Form/FileUpload/FileUpload/FileUploadAll.css` | `@axa-fr/canopee-css/client/Form/FileUpload/FileUpload/FileUploadAll.css` | IMPLÉMENTÉ |
| Label/InputFile | `ItemLabelApollo`, `InputFileApollo.css` | `ItemLabelLF`, `InputFileLF.css` | IMPLÉMENTÉ |
| Dropzone hover | bouton `--button-bg-color:var(--blue-1200)` et `--button-text-color:var(--white-1000)` | bouton `--button-bg-color:var(--blue-100)` | OBSERVÉ |
| Dropzone erreur | `var(--red-alert-1000)` | `var(--red-alert-1200)` | OBSERVÉ |
| ItemFile icône remove | `delete.svg` | `delete-fill.svg` | IMPLÉMENTÉ |
| ItemFile rayon/bordure | radius `var(--radius-8)`, border `var(--blue-1000)` | radius `var(--radius-4)`, border `var(--gray-800)` | OBSERVÉ |
| ItemFile desktop | pas de media query horizontale relevée | `@media (--desktop-small)` : padding horizontal `calc(24 / var(--font-size-base) * 1rem)` | OBSERVÉ |
| Stories/MDX | contenu identique sauf imports `prospect` | contenu identique sauf imports `client` | DOCUMENTÉ |

## Accessibilité

`InputFile` associe le label par `htmlFor`. L'input reçoit `required`, `aria-invalid`, `aria-errormessage` en erreur, et `aria-describedby` pour l'aide et le message de succès. Le `Svg` du bouton visuel a `role="presentation"`. `ItemFile` met `aria-live="polite"` et `aria-busy` pendant chargement ; les actions ont `aria-label` incluant le nom du fichier : ``Previsualiser le fichier ${file.name}``, ``Suppression du fichier ${file.name}``. La conformité WCAG/RGAA n'est pas certifiée.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : règles d'usage, types de fichiers autorisés, poids maximal, nombre maximal de fichiers, microcopy et cardinalité à vérifier dans le Zeroheight Prospect ou Client.
- `NON_CONFIRMÉ` : les exemples stories indiquent `2 fichiers max. / pdf, png, jpg, jpeg, gif / 5 Mo par fichier`, mais aucune validation technique de taille, extension ou nombre n'est implémentée dans les fichiers lus.
- Vérifier dans le Storybook de la version installée : rendu final de `af-btn-client--tertiary`, icônes Material, états success/warning et support navigateur de `:has()`.
