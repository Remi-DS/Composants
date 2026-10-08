# Loader — Synthèse

Synthèse d'implémentation du composant `Loader` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Indicateur de chargement animé avec titre obligatoire et sous-titre facultatif, basé sur `Spinner`. | DOCUMENTÉ |
| Quand l'utiliser | Le MDX indique un usage pour signaler un chargement. | DOCUMENTÉ |
| Quand ne pas l'utiliser | Non présent dans les sources techniques. | NON_CONFIRMÉ |
| Cardinalité par page | Non présent dans les sources techniques. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `Loader`.
- Exports associés : `LoaderProps`.
- Exports publics : `prospect.ts` réexporte `LoaderApollo`, `client.ts` réexporte `LoaderLF`.
- Élément racine rendu et classe CSS de base : `<article class="af-loader">` ou `<dialog class="af-loader">`.
- Dépendances internes : `SpinnerApollo`/`SpinnerLF`, `PolymorphicComponent`, `useId`, `getClassName`.

```tsx
import { Loader } from "@axa-fr/canopee-react/prospect";

<Loader title="Chargement en cours" subtitle="Merci de patienter" />;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `title` | `string` | Aucun | Texte obligatoire dans `.af-loader__title`; sert à `aria-labelledby`. | IMPLÉMENTÉ |
| `subtitle` | `string` | `undefined` | Rendu uniquement si fourni. | IMPLÉMENTÉ |
| `spinnerProps` | `SpinnerProps` | Non défini dans le type ; MDX indique `{}` | Transmis au composant `Spinner`. | IMPLÉMENTÉ |
| `isDialog` | `boolean` | Faux par comportement | Rend `<dialog>` si vrai, sinon `<article>`. | IMPLÉMENTÉ |
| `className` | `string` | `undefined` | Ajouté à la classe calculée. | IMPLÉMENTÉ |
| Props polymorphes | `PolymorphicComponent<T, LoaderProps>` | Selon élément | Transmises à l'élément racine. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Racine standard | `isDialog=false` | `<article class="af-loader">` | IMPLÉMENTÉ |
| Racine dialog | `isDialog=true` | `<dialog class="af-loader">` | IMPLÉMENTÉ |
| Spinner | via `spinnerProps` | Classes du composant `Spinner` | IMPLÉMENTÉ |

Aucune variante visuelle propre à `Loader` n'est exposée.

## États et comportements

- Le rendu du composant représente l'état de chargement ; il n'expose pas de prop `loading`.
- `useId()` génère l'identifiant du titre.
- L'élément racine reçoit `aria-labelledby` pointant vers le titre.
- Le `Spinner` est rendu avant le contenu textuel.
- Le sous-titre est conditionnel.
- La capacité à rendre un `<dialog>` ne documente pas une règle de design de modale.

## Anatomie

```html
<article|dialog class="af-loader" aria-labelledby="loader-title-id">
  <Spinner />
  <div class="af-loader__content">
    <span id="loader-title-id" class="af-loader__title">Titre</span>
    <span class="af-loader__subtitle">Sous-titre</span>
  </div>
</article|dialog>
```

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Titre | `--af-loader-title-font-size: var(--rem-16)` | OBSERVÉ |
| Backdrop commun | `--af-loader-backdrop-color: var(--blue-1000-20)` | OBSERVÉ |
| Backdrop Client | `--af-loader-backdrop-color: var(--gray-1000-48)` dans `LoaderLF.css` | OBSERVÉ |
| Espacement | `--af-loader-gap: var(--rem-16)`, `--af-loader-padding: var(--rem-24)` | OBSERVÉ |
| Sous-titre | Taille `var(--rem-16)` dans un contexte desktop-small | OBSERVÉ |
| Transition | Utilise `var(--af-loader-transition-duration)` | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Implémentation | `LoaderApollo` avec `SpinnerApollo` | `LoaderLF` avec `SpinnerLF` | IMPLÉMENTÉ |
| API | `LoaderProps` commune | `LoaderProps` commune | IMPLÉMENTÉ |
| CSS | `LoaderApollo.css` et commun | `LoaderLF.css` et commun | IMPLÉMENTÉ |
| Backdrop | `var(--blue-1000-20)` dans le commun/Apollo | `var(--gray-1000-48)` dans LF | OBSERVÉ |

## Accessibilité

- `aria-labelledby` relie l'élément racine au titre généré.
- Racine en `<article>` par défaut ou `<dialog>` si `isDialog`.
- Aucun comportement de focus ou de clavier n'est implémenté directement.
- Le rapport entre `Spinner` et technologies d'assistance dépend du composant `Spinner`.
- Le MDX mentionne des éléments d'accessibilité comme rôle `status` et `aria-label`, mais ils ne sont pas visibles dans `LoaderCommon`.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : présence effective d'un rôle `status`.
- `NON_CONFIRMÉ` : présence effective d'une `aria-label`.
- `NON_CONFIRMÉ` : ratio de contraste annoncé par la documentation.
- `NON_CONFIRMÉ` : comportement de focus et fermeture du `<dialog>`.
- `NON_CONFIRMÉ` : durée d'affichage, cardinalité et règles d'usage.
- Vérifier dans le Storybook de la version installée : rendu du backdrop et du Spinner par univers.
