# Svg — Synthèse

Synthèse d'implémentation du composant `Svg` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Injecter le contenu d'un fichier SVG externe dans un élément `<svg>`, avec nettoyage DOMPurify. | IMPLÉMENTÉ |
| Quand l'utiliser | Le code marque `Svg` comme déprécié au profit de `Icon`; le MDX Client avertit de ne pas l'utiliser directement. | IMPLÉMENTÉ / DOCUMENTÉ |
| Quand ne pas l'utiliser | `Don't use this component directly. Please use Icon instead.` dans le MDX Client. | DOCUMENTÉ |
| Cardinalité par page | Aucune limite publiée. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `Svg`.
- Exports associés : aucun.
- Élément racine rendu et classe CSS de base : `<svg className="af-svg">`, ou `<span>` de fallback en erreur avec `alt`.
- Dépendances internes : `svgInjector`, `@tanem/svg-injector`, `dompurify`.

```tsx
import { Svg } from "@axa-fr/canopee-react/prospect";
import headphonesIcons from "@material-symbols/svg-400/outlined/headphones.svg";

<Svg src={headphonesIcons} fill="#00008f" />;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `src` | `string` | requis | Posé en `data-src`, sert à l'injection SVG. | IMPLÉMENTÉ |
| `alt` | `string` | `undefined` | Si injection en erreur, rend un `<span>` contenant `alt`; sinon `null`. | IMPLÉMENTÉ |
| `width` | `SVGAttributes` | `24` | Attribut du `<svg>` initial et du SVG temporaire injecté. | IMPLÉMENTÉ |
| `height` | `SVGAttributes` | `24` | Attribut du `<svg>` initial et du SVG temporaire injecté. | IMPLÉMENTÉ |
| `className` | `string` | `undefined` | Combiné avec `af-svg`. | IMPLÉMENTÉ |
| Props SVG | `SVGAttributes<SVGSVGElement>` | selon SVG | Attributs transmis au `<svg>` initial, puis clonés vers l'élément temporaire. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Aucune variante | Non exposée | `af-svg` uniquement | IMPLÉMENTÉ |

## États et comportements

- `useLayoutEffect` relance l'injection quand `src`, `width`, `height` ou `hasError` changent.
- Lors d'un changement de `src`, `hasError` est remis à `false`.
- En succès, `root.innerHTML` reçoit le contenu du SVG injecté et les attributs injectés sont clonés vers le root.
- En erreur, `setHasError(true)` déclenche le fallback `span` si `alt` existe.
- Le commentaire code indique que `width`/`height` par défaut restent à `24px` pour ne pas casser le CSS existant.

## Anatomie

- État normal : `<svg ref data-src width height class="af-svg">` contenant le SVG injecté.
- État erreur avec `alt` : `<span>{alt}</span>` avec les props castées en props de `span`.
- État erreur sans `alt` : `null`.
- `svgInjector` restaure les attributs `aria-*`, `fill` et `stroke` sur le SVG injecté.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| CSS commun | `.af-svg { flex-shrink: 0; }`. | OBSERVÉ |
| Story | `.icon-list` utilise grille, `gap:1rem`, cartes `10rem`, SVG `8rem`. | OBSERVÉ |
| Responsive | Aucune media query dédiée dans le CSS composant. | OBSERVÉ |

Les valeurs proviennent du CSS source et peuvent évoluer.

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Implémentation | Même fichier `Svg.tsx`, CSS importé depuis le chemin prospect. | Même fichier `Svg.tsx`, CSS importé depuis le chemin prospect. | IMPLÉMENTÉ |
| MDX | Présente l'import et l'usage. | Ajoute un warning demandant d'utiliser `Icon`. | DOCUMENTÉ |
| Story CSS | `Svg.story.scss`. | `Svg.stories.css`. | OBSERVÉ |

## Accessibilité

- Les attributs `aria-*` présents sur l'élément initial sont restaurés après injection.
- Aucun `role`, `aria-hidden` ou `aria-label` n'est ajouté automatiquement.
- `alt` n'est pas un nom accessible en état normal ; il ne sert qu'au fallback d'erreur.
- La conformité WCAG/RGAA n'est pas certifiée.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : cas où conserver `Svg` malgré la dépréciation au profit de `Icon`.
- `NON_CONFIRMÉ` : règles d'icônes décoratives vs informatives.
- Vérifier dans le Storybook de la version installée : comportement réseau et fallback d'injection selon le bundler.
