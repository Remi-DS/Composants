# ProgressBar — Synthèse

Synthèse d'implémentation du composant `ProgressBar` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Les MDX décrivent un indicateur visuel de progression basé sur l'élément HTML natif `<progress>`. | DOCUMENTÉ |
| Quand l'utiliser | Les MDX montrent un chargement ou une progression mesurable, sans règle de contexte produit. | DOCUMENTÉ |
| Quand ne pas l'utiliser | Non publié ; consulter le Zeroheight de l'univers concerné. | NON_CONFIRMÉ |
| Cardinalité par page | Non publiée dans les sources lues. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `ProgressBar`.
- Exports associés : aucun type public nommé dans les barrels lus.
- Élément racine rendu : fragment React contenant optionnellement un `<label>` puis `div.af-progress-bar`.
- Dépendances internes : `useId`, `getClassName`, props natives `<progress>`.

```tsx
import { ProgressBar } from "@axa-fr/canopee-react/prospect";

<ProgressBar value={70} max={100} label="Loading something" />;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `label` | `ReactNode` | `undefined` | Si présent, rend un `<label>` associé au `<progress>` par `htmlFor`. | IMPLÉMENTÉ |
| `showPercentage` | `boolean` | `false` | Si vrai, affiche un `<span aria-hidden="true">{props.value} %</span>`. | IMPLÉMENTÉ |
| `id` | prop native `progress` | `useId()` | Sert à lier le label ; un `id` fourni remplace l'id généré. | IMPLÉMENTÉ |
| `className` | `string` | `undefined` | Ajouté à `af-progress-bar`. | IMPLÉMENTÉ |
| `value` | prop native `progress` | valeur native si absente | Transmise au `<progress>` et réutilisée dans le pourcentage visible. | IMPLÉMENTÉ |
| `max` | prop native `progress` | `1` par défaut HTML selon le MDX | Les MDX indiquent `max=1` par défaut et possibilité de le remplacer. | DOCUMENTÉ |
| Props natives | `ComponentProps<"progress">` | selon HTML | Toutes les autres props sont propagées au `<progress>`. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Variante visuelle | Aucune prop de variante | `af-progress-bar` et `af-progress-bar__progress` | IMPLÉMENTÉ |
| Pourcentage visible | `showPercentage=true` | span enfant sans classe | IMPLÉMENTÉ |

## États et comportements

- Le composant ne gère pas d'état interne de progression.
- La valeur est celle fournie par les props natives `value` et `max`.
- Le label est présent dans le DOM mais stylé comme invisible.
- `showPercentage` affiche la valeur brute de `props.value` suivie de `%`; le code ne calcule pas `value / max * 100`.
- Les transitions de remplissage sont gérées en CSS sur les pseudo-éléments de `<progress>`.

## Anatomie

- Fragment React.
- `label.af-progress-bar__label` si `label` est fourni.
- `div.af-progress-bar`.
- `progress.af-progress-bar__progress` avec `id`.
- `span[aria-hidden="true"]` optionnel pour le pourcentage.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Label | `display: block`, `width: 0`, `height: 0`, `opacity: 0`, `user-select: none` | OBSERVÉ |
| Conteneur | `display: flex`, `flex-direction: row`, `align-items: center`, `gap: var(--rem-8)` | OBSERVÉ |
| Pourcentage | `width: 41px`, `font-size: var(--rem-16)`, `font-weight: 600`, `line-height: 125%`, `white-space: nowrap` | OBSERVÉ |
| Progress | `width: 100%`, `height: var(--rem-6)`, `border: none`, `border-radius: var(--radius-4)`, `overflow: hidden` | OBSERVÉ |
| Couleurs | `--progress-bar-background-color: var(--gray-140)`, `--progress-bar-appearance: var(--blue-1000)` | OBSERVÉ |
| WebKit | `::-webkit-progress-bar` et `::-webkit-progress-value`, transition `width 0.75s ease` | OBSERVÉ |
| Firefox | `::-moz-progress-bar`, commentaire indiquant que la transition ne fonctionne pas sur les barres natives Firefox | OBSERVÉ |

Aucune media query n'a été relevée pour `ProgressBar`.

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Import CSS | `@axa-fr/canopee-css/prospect/ProgressBar/ProgressBarApollo.css` | `@axa-fr/canopee-css/client/ProgressBar/ProgressBarLF.css` | IMPLÉMENTÉ |
| Code React | Même `ProgressBarCommon`. | Même `ProgressBarCommon`. | IMPLÉMENTÉ |
| Variables de couleur | `gray-140` / `blue-1000` | `gray-140` / `blue-1000` | OBSERVÉ |
| Stories | Mêmes args : `label`, `value: 70`, `max: 100`, `showPercentage: false`. | Mêmes args. | DOCUMENTÉ |

## Accessibilité

- Les MDX recommandent explicitement d'utiliser `label` pour fournir un libellé descriptif.
- Le label est lié au `<progress>` par `htmlFor` et `id`.
- Les MDX indiquent que les lecteurs d'écran annoncent par défaut le pourcentage et que `aria-valuetext` permet un texte personnalisé.
- Le span de pourcentage est `aria-hidden`.
- La conformité WCAG/RGAA n'est pas certifiée par les sources.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : contexts d'usage, seuils de progression, wording exact du label et cardinalité.
- `NON_CONFIRMÉ` : règle de design sur l'affichage ou non du pourcentage visible.
- Vérifier dans le Storybook de la version installée le rendu navigateur du `<progress>`.
