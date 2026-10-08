# ProgressBarGroup — Synthèse

Synthèse d'implémentation du composant `ProgressBarGroup` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Les MDX décrivent un indicateur de progression multi-étapes pour assistants, formulaires ou processus à plusieurs étapes. | DOCUMENTÉ |
| Quand l'utiliser | Les MDX citent wizards, forms ou processus à plusieurs étapes, sans règle de parcours détaillée. | DOCUMENTÉ |
| Quand ne pas l'utiliser | Non publié ; consulter le Zeroheight de l'univers concerné. | NON_CONFIRMÉ |
| Cardinalité par page | Non publiée dans les sources lues. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `ProgressBarGroup`.
- Exports associés : aucun type public nommé dans les barrels lus.
- Élément racine rendu : `<ol className="af-progress-bar-group">`.
- Dépendances internes : `ProgressBar`, `useSequentialProgress`, `classnames`, `useId`.

```tsx
import { ProgressBarGroup } from "@axa-fr/canopee-react/prospect";

<ProgressBarGroup currentStep={1} currentStepProgress={50} stepsCount={4} />;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `currentStep` | `number` | requis | Index zéro-based de l'étape courante, transmis à `useSequentialProgress`. | IMPLÉMENTÉ |
| `currentStepProgress` | `number` | `0` | Divisé par `max` pour obtenir la progression de l'étape courante. | IMPLÉMENTÉ |
| `stepsCount` | `2 \| 3 \| 4 \| 5 \| 6 \| 7 \| 8` | `4` | Nombre d'items `<li>` et de barres. | IMPLÉMENTÉ |
| `max` | `number` | `100` | Échelle utilisée pour convertir `currentStepProgress`. | IMPLÉMENTÉ |
| `className` | `string` | `undefined` | Ajouté avec `classnames` à `af-progress-bar-group`. | IMPLÉMENTÉ |
| Props natives ol | `Omit<ComponentProps<"ol">, "children" \| "className">` | selon React | Propagées au `<ol>`. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Nombre d'étapes | `stepsCount` de 2 à 8 par type TypeScript | pas de classe dédiée | IMPLÉMENTÉ |
| Variante visuelle propre | Aucune prop de variante | `af-progress-bar-group` | IMPLÉMENTÉ |

## États et comportements

- `useSequentialProgress` produit un tableau de nombres entre `0` et `1`.
- Les étapes avant `currentStep` valent `1`, l'étape courante vaut `currentStepProgress / max`, les suivantes valent `0`.
- Le hook anime les changements par pas, avec une durée par défaut de `750 ms`.
- Si le nombre d'étapes change, le tableau est réinitialisé et le timeout existant est nettoyé.
- Chaque `ProgressBar` reçoit `value={value}` et `aria-hidden`.
- Aucun libellé d'étape n'est rendu par le composant.

## Anatomie

- `ol.af-progress-bar-group`.
- `li.af-progress-bar-group__item` pour chaque progression calculée.
- `ProgressBarComponent` dans chaque item.
- Les clés React incluent `useId()` et l'index ; le code désactive la règle `react/no-array-index-key` avec justification.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Commun | `display: flex`, `margin: 0`, `padding: 0`, `align-items: flex-end`, `gap: var(--rem-8)`, `list-style-type: none` | OBSERVÉ |
| Item | `.af-progress-bar-group__item { flex: 1; }` | OBSERVÉ |
| Prospect | CSS Apollo importe uniquement `ProgressBarGroupCommon.css`. | OBSERVÉ |
| Client | CSS LF importe le commun puis ajoute `border-radius: var(--rem-6)` et `overflow: hidden`. | OBSERVÉ |
| ProgressBar interne | Hérite de `height: var(--rem-6)`, couleurs `gray-140` / `blue-1000`. | OBSERVÉ |

Aucune media query n'a été relevée pour `ProgressBarGroup`.

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Import CSS | `ProgressBarGroupApollo.css` | `ProgressBarGroupLF.css` | IMPLÉMENTÉ |
| ProgressBar utilisée | `ProgressBarApollo` | `ProgressBarLF` | IMPLÉMENTÉ |
| Style de groupe | Commun seul | Ajout de rayon `var(--rem-6)` et `overflow: hidden` | OBSERVÉ |
| Stories | Args `stepsCount: 4`, `currentStep: 2`, `currentStepProgress: 80`. | Même args. | DOCUMENTÉ |

## Accessibilité

- Le `<ol>` conserve une sémantique de liste ordonnée.
- Les barres internes reçoivent `aria-hidden`, donc le groupe n'annonce pas directement la progression.
- Aucune prop `aria-label` par défaut n'est ajoutée au groupe.
- La source ne certifie pas la conformité WCAG/RGAA ; le libellé accessible du processus est à confirmer côté intégration.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : règles de choix entre `ProgressBar` et `ProgressBarGroup`, libellés d'étapes, affichage textuel de progression.
- `NON_CONFIRMÉ` : comportement attendu si `currentStep` sort de la plage ou si `currentStepProgress > max`.
- Vérifier dans le Storybook de la version installée l'animation et le rendu Client avec `overflow: hidden`.
