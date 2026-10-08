# Spinner — Synthèse

Synthèse d'implémentation du composant `Spinner` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Indicateur animé de chargement rendu comme un `div` circulaire avec `role="alert"` et `aria-busy`. | IMPLÉMENTÉ |
| Quand l'utiliser | Le MDX indique l'usage d'un spinner et renvoie au Zeroheight pour les guidelines. Les règles design restent à confirmer. | DOCUMENTÉ / NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Aucune contre-indication explicite dans les MDX lus. | NON_CONFIRMÉ |
| Cardinalité par page | Aucune limite publiée. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `Spinner`.
- Exports associés : `spinnerVariants`, `SpinnerVariants`.
- Élément racine rendu et classe CSS de base : `<div className="af-spinner af-spinner--variant af-spinner--size">`.
- Dépendances internes : aucune dépendance composant ; CSS commun + CSS par univers.

```tsx
import { Spinner } from "@axa-fr/canopee-react/prospect";

<Spinner size={40} variant="gray" text="Chargement en cours" />;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `size` | `24 \| 32 \| 40` | `40` | Ajoute `af-spinner--24`, `--32` ou `--40`, et définit `--spinner-size` inline. | IMPLÉMENTÉ |
| `variant` | `"blue" \| "gray" \| "white"` | `"blue"` | Ajoute `af-spinner--${variant}`. | IMPLÉMENTÉ |
| `text` | `string` | `"Chargement en cours"` | Utilisé comme `aria-label`, pas affiché visuellement. | IMPLÉMENTÉ |
| `className` | `string` | `undefined` | Ajouté après les classes calculées. | IMPLÉMENTÉ |
| Props natives | `ComponentPropsWithoutRef<"div">` | selon HTML | Transmises au `div` racine avant `aria-*`, `className` et `style`. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Couleur | `blue` | `af-spinner--blue` | IMPLÉMENTÉ |
| Couleur | `gray` | `af-spinner--gray` | IMPLÉMENTÉ |
| Couleur | `white` | `af-spinner--white` | IMPLÉMENTÉ |
| Taille | `24`, `32`, `40` | `af-spinner--24`, `--32`, `--40` | IMPLÉMENTÉ |

## États et comportements

- Animation CSS `spin` en rotation linéaire infinie sur 2 secondes.
- Le composant ne gère pas d'état React interne.
- La prop `text` donne le nom accessible ; elle n'ajoute pas de texte dans le DOM visible.
- Les stories documentent les tailles `40` par défaut, `32`, `24` et les variantes `blue`, `gray`, `white`.

## Anatomie

- Un unique `<div>` auto-fermant.
- Attributs forcés : `role="alert"`, `aria-busy`, `aria-label={text}`, `aria-live="assertive"`.
- Classes : `af-spinner`, `af-spinner--${variant}`, `af-spinner--${size}`, puis `className`.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Commun | `display:flex`, `container-type:inline-size`, `margin:auto`, `border-radius:50%`. | OBSERVÉ |
| Commun | `width`/`height: calc(var(--spinner-size) * 1px)`, `--spinner-border-size: 3`. | OBSERVÉ |
| Prospect | Couleurs `var(--blue-1000-20)`, `var(--blue-1000)`, gris `var(--gray-500-20)`/`var(--gray-1000)`, blanc `var(--white-1000-20)`/`var(--white-1000)`. | OBSERVÉ |
| Prospect | Largeur/hauteur diminuées de `8px` et `border-left` coloré avec la couleur haute. | OBSERVÉ |
| Client | Couleurs via `color-mix(... transparent 80%)`; `--spinner-border-size` vaut `2` en `24` et `4` en `40`. | OBSERVÉ |
| Responsive | Aucune media query dédiée. | OBSERVÉ |

Les valeurs proviennent du CSS source et peuvent évoluer.

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| CSS importé | `SpinnerApollo.css`. | `SpinnerLF.css`. | IMPLÉMENTÉ |
| Dimensions | Réduit `width` et `height` de `8px`. | Conserve les dimensions communes. | OBSERVÉ |
| Couleurs | Tokens `*-20` et `gray-1000`. | `color-mix` et `gray-800`. | OBSERVÉ |

## Accessibilité

- `role="alert"` et `aria-live="assertive"` annoncent l'état comme une alerte.
- `aria-busy` est présent sans valeur explicite.
- `aria-label` reprend `text`.
- Aucun contrôle clavier n'est implémenté ; la conformité WCAG/RGAA n'est pas certifiée.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : quand préférer `Spinner` à `SkeletonList` ou `Loader`.
- `NON_CONFIRMÉ` : durée avant affichage, cardinalité, texte accessible recommandé.
- Vérifier dans le Storybook de la version installée : contraste de `white` sur le fond réel.
