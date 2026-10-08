# ClickIcon — Synthèse

Synthèse d'implémentation du composant `ClickIcon` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Bouton icône rendu en `<button>` et composé avec `Icon`. Le MDX montre un clic déclenchant une action. | DOCUMENTÉ / IMPLÉMENTÉ |
| Quand l'utiliser | Non présent dans les sources au-delà de l'exemple. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Non présent dans les sources techniques. | NON_CONFIRMÉ |
| Cardinalité par page | Non présent dans les sources techniques. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `ClickIcon`.
- Exports associés : aucun type public dédié exporté par `prospect.ts`/`client.ts`.
- Élément racine rendu et classe CSS de base : `<button type="button" className="af-click-icon">`.
- Dépendances internes : `IconCommon`, `iconSizeVariants`, `getClassName`.

```tsx
import { ClickIcon } from "@axa-fr/canopee-react/prospect";

<ClickIcon src={visibility} aria-label="Afficher" onClick={handleClick} />;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `src` | `string` | — | Source SVG transmise à `Icon`. | IMPLÉMENTÉ |
| `iconVariant` | `IconVariants` | `"primary"` | Variante de l'icône ; si `"disabled"`, le bouton reçoit `disabled`. | IMPLÉMENTÉ |
| `iconClassName` | `string` | — | Transmise à `Icon`. | IMPLÉMENTÉ |
| `size` | `IconSizeVariants` | `"S"` | Transmise à `Icon` et convertie en modificateur via `iconSizeVariants[size]`. | IMPLÉMENTÉ |
| `variant` | `"default" \| "ghost"` | `"default"` | Ajoute `af-click-icon--default` ou `af-click-icon--ghost`. | IMPLÉMENTÉ |
| `className` | `string` | — | Fusionnée avec la classe de base et les modificateurs. | IMPLÉMENTÉ |
| Props bouton | `ComponentPropsWithRef<"button">` | — | Transmises au bouton après `disabled`. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Apparence | `default` | `af-click-icon--default` | IMPLÉMENTÉ |
| Apparence | `ghost` | `af-click-icon--ghost` | IMPLÉMENTÉ |
| Taille story | `S` | modificateur issu de `iconSizeVariants` | DOCUMENTÉ / IMPLÉMENTÉ |
| Taille story | `XS` | `af-click-icon--extra-small` observé côté CSS | DOCUMENTÉ / OBSERVÉ |

## États et comportements

Le bouton force `type="button"`.
`disabled` est calculé depuis `iconVariant === "disabled"`, puis les props restantes sont étalées après ; une prop `disabled` explicite peut donc techniquement surcharger.
Le CSS désactive les événements en `:disabled` et pour `.af-click-icon--disabled` marqué comme héritage à supprimer.
`variant="ghost"` met le fond transparent.
Les états hover, active et focus visible pilotent des variables CSS de fond, remplissage SVG et outline.

## Anatomie

Structure : `button.af-click-icon` > `Icon`.
Le SVG interne est ciblé par `.af-icon svg` et reçoit `fill: var(--click-icon-svg-fill, var(--icon-fill))`.
Aucun texte visible n'est rendu par le composant.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Commun | `display: grid`, `width: fit-content`, `padding: var(--rem-8)`, `border: 0`, `border-radius: 100vmax`. | OBSERVÉ |
| Extra small | `.af-click-icon--extra-small { padding: var(--rem-4); }`. | OBSERVÉ |
| Focus commun | `outline: 2px solid var(--click-icon-outline-color, transparent)`, `outline-offset: 2px`. | OBSERVÉ |
| Prospect | fond normal `var(--blue-080)`, hover `var(--blue-1200)` + icône blanche, active `var(--blue-900)`. | OBSERVÉ |
| Client | fond normal `var(--blue-080)`, hover `var(--blue-100)`, active `var(--blue-040)`. | OBSERVÉ |
| Disabled | fond `var(--gray-050)`, icône `var(--gray-500)` dans les deux univers. | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| CSS focus | `--click-icon-outline-color: var(--blue-1000)` | `--click-icon-outline-color: var(--blue-650)` et `:focus { outline: none; }` | OBSERVÉ |
| Ghost hover | icône `var(--blue-1200)` | icône `var(--blue-1000)` | OBSERVÉ |
| Ghost active | icône `var(--blue-900)` | icône `var(--blue-800)` | OBSERVÉ |
| Story icon | `visibility-fill.svg` | `article-fill.svg` | DOCUMENTÉ |

## Accessibilité

Les stories exposent `"aria-label"` avec description "The aria-label attribute for accessibility." et défaut Storybook "Click icon".
Le composant ne fournit pas d'`aria-label` par défaut dans le code ; l'intégrateur doit le passer pour un bouton sans texte.
La sémantique native `<button>` et le focus visible sont conservés.
La conformité WCAG/RGAA n'est pas certifiée.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : règles de choix entre `ClickIcon` et un bouton textuel, cardinalité, libellé accessible recommandé.
- `NON_CONFIRMÉ` : statut de `.af-click-icon--disabled`, commenté dans le CSS comme à supprimer.
- Vérifier dans le Storybook de la version installée : surcharge possible de `disabled`, contraste des états ghost sur fond réel.
