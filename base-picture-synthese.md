# BasePicture — Synthèse

Synthèse d'implémentation du composant `BasePicture` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Affiche une image ronde de base ; les stories montrent un cas par défaut et un cas avec photo. | IMPLÉMENTÉ / DOCUMENTÉ |
| Image par défaut | Utilise `logo-axa.svg` quand `src` est absent ou falsy. | IMPLÉMENTÉ |
| Quand l'utiliser | Non publié dans les sources lues ; vérifier le Zeroheight de l'univers. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Non publié dans les sources lues. | NON_CONFIRMÉ |
| Cardinalité par page | Non publiée dans les sources lues. | NON_CONFIRMÉ |

## Intégration React

- Package Prospect : `@axa-fr/canopee-react/prospect`, export `BasePicture`.
- Package Client : `@axa-fr/canopee-react/client`, export `BasePicture`.
- Fichier React commun : `BasePicture.tsx`.
- CSS commun importé depuis le fichier React : `@axa-fr/canopee-css/prospect/BasePicture/BasePictureAll.css`.
- Élément racine : `<img class="af-basepicture">`.

```tsx
import { BasePicture } from "@axa-fr/canopee-react/prospect";

<BasePicture src="https://picsum.photos/48" alt="random image" />;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `src` | `ComponentProps<"img">["src"]` | `logo-axa.svg` | Si `src` est absent ou falsy, le logo AXA est utilisé. | IMPLÉMENTÉ |
| `className` | `string` | `undefined` | Ajoutée après `af-basepicture` par jointure de chaînes. | IMPLÉMENTÉ |
| `alt` | `ComponentProps<"img">["alt"]` | `""` | `alt=""` est posé avant le spread ; une prop `alt` fournie le remplace. | IMPLÉMENTÉ |
| Props natives | `ComponentProps<"img">` | — | Toutes les autres props d'image sont transmises à `<img>`. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Aucune variante exposée | — | `af-basepicture` | IMPLÉMENTÉ |

Le composant n'expose pas d'objet de variantes, d'état contrôlé ni de taille paramétrable.

## États et comportements

- Absence de `src` : rendu du logo AXA importé depuis `@axa-fr/canopee-css/logo-axa.svg`.
- `src` fourni : rendu de la source passée.
- `alt` par défaut vide ; les stories avec photo fournissent `alt: "random image"`.
- Aucun état `loading`, `error`, placeholder distant ou fallback après erreur d'image n'est implémenté dans le composant.

## Anatomie

- Élément unique : `<img>`.
- Classes : `af-basepicture` puis `className` additionnelle éventuelle.
- Attributs posés : `alt`, `src`, puis les autres props.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Taille | `width: var(--rem-48)`, `height: var(--rem-48)`. | OBSERVÉ |
| Forme | `border-radius: 100%`. | OBSERVÉ |
| Responsive | Aucun `@media` dans `BasePictureAll.css`. | OBSERVÉ |
| Tokens | Utilise uniquement `--rem-48` dans le CSS lu. | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Export public | `@axa-fr/canopee-react/prospect`. | `@axa-fr/canopee-react/client`. | IMPLÉMENTÉ |
| Code React | Même fichier `BasePicture.tsx`. | Même fichier `BasePicture.tsx`. | IMPLÉMENTÉ |
| CSS | Le fichier React importe `prospect/BasePicture/BasePictureAll.css`. | Pas de CSS LF distinct listé dans l'index. | OBSERVÉ |
| Stories | Même structure, import prospect. | Même structure, import client. | DOCUMENTÉ |

## Accessibilité

- Le composant définit `alt=""` par défaut, ce qui rend l'image décorative sauf override.
- Les stories démontrent un `alt` explicite dans le cas avec photo.
- Aucune validation n'impose un texte alternatif pour une image informative.
- La conformité WCAG/RGAA n'est pas certifiée par les sources lues.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : usage design exact du logo AXA comme image par défaut.
- `NON_CONFIRMÉ` : règles de texte alternatif selon image décorative ou informative.
- `NON_CONFIRMÉ` : tailles alternatives, densité d'image et cardinalité.
- Vérifier dans le Storybook de la version installée : rendu réel de l'image par défaut et comportement si l'image distante échoue.
