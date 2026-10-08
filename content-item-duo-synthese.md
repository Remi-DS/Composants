# ContentItemDuo — Synthèse

Synthèse d'implémentation du composant `ContentItemDuo` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Affiche une paire label / valeur, avec bouton d'action optionnel. Le MDX indique un usage pour des paires clé-valeur. | DOCUMENTÉ |
| Quand l'utiliser | Le MDX dit "present key-value pairs"; les critères design précis restent absents. | DOCUMENTÉ / NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Non présent dans les sources techniques. | NON_CONFIRMÉ |
| Cardinalité par page | Non présent dans les sources techniques. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `ContentItemDuo`.
- Exports associés : aucun type public dédié exporté par `prospect.ts`/`client.ts`.
- Élément racine rendu et classe CSS de base : `<div className="af-content-item-duo">`, contenant `dt` et `dd`.
- Dépendances internes : `Button`, `ItemMessage`, `getClassName`.

```tsx
import { ContentItemDuo } from "@axa-fr/canopee-react/prospect";

<dl>
  <ContentItemDuo label="Status" value="Active" buttonText="Details" onButtonClick={handleClick} />
</dl>;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `label` | `ReactNode` | — | Rendu dans `dt.af-content-item-duo__label`. | IMPLÉMENTÉ |
| `value` | `ReactNode` | — | Rendu dans `dd.af-content-item-duo__value`. | IMPLÉMENTÉ |
| `position` | `"horizontal" \| "vertical"` | `"horizontal"` | Ajoute `af-content-item-duo--horizontal` ou `--vertical`. | IMPLÉMENTÉ |
| `size` | `"small" \| "large"` | `"large"` | Ajoute `af-content-item-duo--small` seulement si `small`. Type : vertical seulement en `large`. | IMPLÉMENTÉ |
| `buttonText` | `string` | — | Texte du bouton optionnel. | IMPLÉMENTÉ |
| `onButtonClick` | `() => void` | — | Handler du bouton ; requis avec `buttonText` pour rendre le bouton. | IMPLÉMENTÉ |
| `message` | `ItemMessageProps["message"]` | — | Rend `ItemMessage` dans la valeur si fourni. | IMPLÉMENTÉ |
| `messageType` | `ItemMessageProps["messageType"]` | — | Transmis à `ItemMessage`. | IMPLÉMENTÉ |
| Props div | `ComponentProps<"div">` | — | Transmises au conteneur. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Position | `horizontal` | `af-content-item-duo--horizontal` | IMPLÉMENTÉ |
| Position | `vertical` | `af-content-item-duo--vertical` | IMPLÉMENTÉ |
| Taille | `large` | pas de modificateur dédié | IMPLÉMENTÉ |
| Taille | `small` | `af-content-item-duo--small` | IMPLÉMENTÉ |

## États et comportements

Le bouton est rendu uniquement si `buttonText` et `onButtonClick` sont tous deux fournis ; le MDX le documente explicitement.
Le bouton utilise `variant="ghost"` et la classe `af-content-item-duo__button`.
Le message est rendu dans le `dd`, sous la valeur, avec un `gap: var(--rem-8)`.
Le type TypeScript contraint `position="vertical"` à `size?: "large"`.

## Anatomie

Structure : `div.af-content-item-duo` > `dt.af-content-item-duo__label` > `dd.af-content-item-duo__value` > message éventuel > bouton éventuel.
Le MDX recommande d'utiliser le composant à l'intérieur d'un `<dl>` car il rend une paire `<dt>` / `<dd>`.
Le conteneur racine n'est pas lui-même un `<dl>`.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Commun | `display: grid`, `width: 100%`, colonnes `1fr 1fr`, gap ligne `var(--rem-8)`. | OBSERVÉ |
| Variables mobile | `--content-item-duo-column-gap: 16`, `--content-item-duo-text-font-size: 16`. | OBSERVÉ |
| Desktop | `@media (--desktop-small)` : column gap `40`, font size `18`. | OBSERVÉ |
| Horizontal + bouton | grid `"label value"` / `". button"`. | OBSERVÉ |
| Horizontal small | value aligné à droite, gap `16`, font size `16`. | OBSERVÉ |
| Vertical | grille une colonne ; avec bouton : `"label button"` / `"value value"`. | OBSERVÉ |
| Couleurs | label `var(--gray-800)`, value `var(--gray-1000)` dans les deux univers. | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Bouton interne | `ButtonApollo` | `ButtonLF` | IMPLÉMENTÉ |
| CSS couleurs | identique à LF dans les fichiers lus | identique à Apollo dans les fichiers lus | OBSERVÉ |
| Stories | mêmes variantes horizontal/vertical | mêmes variantes horizontal/vertical | DOCUMENTÉ |

## Accessibilité

Le MDX documente l'usage dans un `<dl>` pour une structure de liste de description correcte.
Le code rend effectivement `dt` et `dd`, mais ne force pas le parent sémantique.
Le bouton optionnel est un `Button` Canopée `ghost`.
La conformité WCAG/RGAA n'est pas certifiée.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : nombre recommandé de paires, format des valeurs, règles de bouton d'action.
- `NON_CONFIRMÉ` : règle design autorisant `small` seulement en horizontal ; seule la contrainte TypeScript est constatée.
- Vérifier dans le Storybook de la version installée : rendu avec messages `error`, `warning`, `success` et wrapping de valeurs longues.
