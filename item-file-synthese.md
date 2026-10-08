# ItemFile — Synthèse

Synthèse d'implémentation du composant `ItemFile` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Affiche un fichier téléversé : nom, taille, indicateur validation/erreur ou chargement, actions aperçu/suppression et message inline. | DOCUMENTÉ |
| Quand l'utiliser | Le MDX indique un usage avec des listes d'upload, sans règle de design détaillée. | DOCUMENTÉ |
| Quand ne pas l'utiliser | Non publié. | NON_CONFIRMÉ |
| Cardinalité par page | Non publiée. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `ItemFile`.
- Exports associés : aucun.
- Élément racine rendu et classe CSS de base : `<section className="af-item-file">`.
- Dépendances internes : `ContentItemMonoCore`, `ItemMessage`, `Icon`, `Spinner`, `ClickIcon`, Material Symbols `check_circle-fill`, `error-fill`, `visibility-fill`, icône suppression selon univers.

```tsx
import { ItemFile } from "@axa-fr/canopee-react/prospect";

<ItemFile file={file} isLoading={false} onPreview={(f) => console.log(f)} onRemove={(f) => console.log(f)} />
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `file` | `File` | requis | Source du nom, de la taille et paramètre des callbacks. | IMPLÉMENTÉ |
| `isLoading` | `boolean` | `false` documenté | Affiche un spinner si vrai et pas d'erreur ; masque l'action aperçu. | IMPLÉMENTÉ |
| `errorMessage` | `string` | - | Ajoute l'état erreur et rend `ItemMessage`. | IMPLÉMENTÉ |
| `onRemove` | `(file, event) => void` | `() => {}` | Appelé par le bouton suppression. | IMPLÉMENTÉ |
| `onPreview` | `(file, event) => void` | `() => {}` | Appelé par le bouton aperçu si visible. | IMPLÉMENTÉ |
| `previewProps`, `removeProps` | `Partial<Omit<ClickIconProps, "src" \| "onClick">>` | `{}` | Étendus sur les boutons icônes après `aria-label` par défaut. | IMPLÉMENTÉ |
| props natives | `Omit<ComponentPropsWithoutRef<"section">, "children">` | natif | Étendues sur la section racine. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Standard | pas d'erreur | `af-item-file` | IMPLÉMENTÉ |
| Erreur | `errorMessage` renseigné | `af-item-file--error` | IMPLÉMENTÉ |
| Chargement | `isLoading={true}` sans erreur | pas de modificateur ; `aria-busy=true` | IMPLÉMENTÉ |

## États et comportements

La taille est formatée en base 1000 avec unités `Ko`, `Mo`, `Go` et minimum affiché `0.1`. Si `isLoading && !hasError`, le statut est un `Spinner size={24}` ; sinon une icône validation ou erreur. L'aperçu est rendu seulement hors chargement et hors erreur. La suppression reste rendue même en chargement ou erreur.

## Anatomie

Structure DOM : `<section>`, `.af-item-file__body`, `ContentItemMonoCore` avec composant gauche et sous-titres, `.af-item-file__actions`, boutons `ClickIcon`, puis `ItemMessage` pour l'erreur.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Racine | `display:flex`, `flex-direction:column`, `gap: var(--item-file-gap)` | OBSERVÉ |
| Body | padding vertical/horizontal variables, `border: 1px solid`, `justify-content:space-between`, `gap: var(--rem-12)` | OBSERVÉ |
| Actions | `display:flex`, `align-items:center`, gap `var(--item-file-action-buttons-gap)` | OBSERVÉ |
| Prospect | rayon `var(--radius-8)`, bordure `var(--blue-1000)`, gap `var(--rem-8)`, actions gap `var(--rem-16)` | OBSERVÉ |
| Client | rayon `var(--radius-4)`, bordure `var(--gray-800)`, padding horizontal desktop `calc(24 / var(--font-size-base) * 1rem)` | OBSERVÉ |
| Erreur | Prospect `var(--red-alert-1000)`, Client `var(--red-alert-1200)` pour bordure et icône | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Icône suppression | `delete.svg` outlined. | `delete-fill.svg` outlined. | IMPLÉMENTÉ |
| ContentItemMono CSS | `ContentItemMonoApollo.css`. | `ContentItemMonoLF.css`. | IMPLÉMENTÉ |
| Story typing | `ComponentProps<typeof ItemFile>`. | type de story `Omit<typeof ItemFile, "file">` observé. | DOCUMENTÉ |

## Accessibilité

La section reçoit `aria-busy=true` en chargement et `aria-live="polite"`. Les boutons ont des `aria-label` par défaut en français : `Previsualiser le fichier {name}` et `Suppression du fichier {name}`. Le MDX recommande de conserver un nom accessible si les props sont surchargées. Aucune conformité WCAG/RGAA n'est certifiée.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : ordre dans une liste, gestion du focus après suppression, formats autorisés, nombre maximum de fichiers.
- `DOCUMENTÉ` : le MDX recommande de ne pas se reposer seulement sur la couleur et de fournir `errorMessage`.
- Vérifier dans le Storybook de la version installée : rendu ContentItemMono et labels d'action définitifs.
