# Link — Synthèse

Synthèse d'implémentation du composant `Link` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle technique | Lien HTML `<a>` avec texte, icônes optionnelles, variante inverse et ouverture dans un nouvel onglet. | IMPLÉMENTÉ |
| État de publication | Les stories locales titrent le composant `Components/Link 🚧` dans les deux univers. | DOCUMENTÉ |
| Quand l'utiliser | Non présent dans les sources techniques ; à confirmer dans le Zeroheight. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Non présent dans les sources techniques. | NON_CONFIRMÉ |
| Cardinalité par page | Non présent dans les sources techniques. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `Link`.
- Exports associés : `linkVariants`, `LinkVariants`, `LinkProps`.
- Exports publics : `prospect.ts` réexporte `LinkApollo`, `client.ts` réexporte `LinkLF`.
- Élément racine rendu et classe CSS de base : `<a class="af-link">`.
- Dépendances internes : `Svg` pour l'icône automatique `open_in_new`, `getClassName`.

```tsx
import { Link } from "@axa-fr/canopee-react/prospect";

<Link href="https://www.axa.fr">Lien</Link>;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `variant` | `"inverse"` | Aucun | Ajoute `af-link--inverse`. | IMPLÉMENTÉ |
| `openInNewTab` | `boolean` | `false` | Ajoute `target="_blank"`, `rel="noopener noreferrer"` et `af-link--openInNewTab`. | IMPLÉMENTÉ |
| `leftIcon` | `ReactNode` | Aucun | Rendu avant les enfants. | IMPLÉMENTÉ |
| `rightIcon` | `ReactNode` | Aucun | Rendu après les enfants ; remplace l'icône automatique d'ouverture externe. | IMPLÉMENTÉ |
| `children` | `ReactNode` | Aucun | Contenu central du lien. | IMPLÉMENTÉ |
| Props natives | `ComponentPropsWithoutRef<"a">` | Selon React/HTML | Transmises à l'ancre, dont `href` et `className`. | IMPLÉMENTÉ |

L'héritage des props natives vient de `ComponentPropsWithoutRef<"a">`.

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Inverse | `inverse` | `af-link--inverse` | IMPLÉMENTÉ |
| Ouverture externe | `openInNewTab=true` | `af-link--openInNewTab` | IMPLÉMENTÉ |

Objet exporté :

```ts
export const linkVariants = { inverse: "inverse" } as const;
```

Les stories utilisent aussi `af-btn-client`, `af-btn-client--secondary` et
`af-btn-client--tertiary` via `className`; ces classes ne font pas partie de `LinkVariants`.

## États et comportements

- Le composant reste toujours une ancre HTML, même avec une apparence de bouton via `className`.
- `openInNewTab=true` force `target="_blank"` et `rel="noopener noreferrer"`.
- Sans `rightIcon`, `openInNewTab` affiche l'icône `open_in_new`.
- Avec `rightIcon`, l'icône fournie est utilisée.
- Aucun contrôle automatique de présence de `href`, nom accessible ou mention textuelle d'ouverture externe n'est implémenté.

## Anatomie

```html
<a class="af-link af-link--inverse af-link--openInNewTab" href="https://www.axa.fr">
  [leftIcon]
  children
  [rightIcon ou Svg open_in_new]
</a>
```

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Base | `display: inline-flex`, alignement centré, `gap: var(--rem-4)` | OBSERVÉ |
| Typographie | 16 px, ligne 20 px, graisse via `--link-font-weight` | OBSERVÉ |
| Décoration | Soulignement avec `text-underline-offset: var(--link-underline-offset, 25%)` | OBSERVÉ |
| Couleur | `var(--blue-1000)` ; hover `var(--blue-1200)` | OBSERVÉ |
| Focus | Contour bleu 2 px, offset 3 px | OBSERVÉ |
| Responsive | `@media (--desktop-small)` : taille 18 px | OBSERVÉ |
| Inverse | Texte blanc, graisse 400, couleur conservée au hover/focus | OBSERVÉ |
| Client | Surcharge de graisse à 400 ; `af-link--openInNewTab` à 600 | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Implémentation React | Même `LinkCommon` | Même `LinkCommon` | IMPLÉMENTÉ |
| CSS | `LinkApollo.css` importe le commun | `LinkLF.css` importe le commun et surcharge | IMPLÉMENTÉ |
| Graisse normale | Valeur commune par défaut : 600 | Surcharge à 400 | OBSERVÉ |
| Ouverture externe | Pas de surcharge spécifique relevée | `af-link--openInNewTab` en 600 | OBSERVÉ |
| Story d'ouverture externe | Une `rightIcon` est fournie dans l'exemple Prospect | Pas de `rightIcon` dans l'exemple Client | DOCUMENTÉ |

## Accessibilité

- Élément natif `<a>`.
- `href` et props ARIA natives sont transmis.
- `rel="noopener noreferrer"` accompagne l'ouverture dans un nouvel onglet.
- Le focus visible est stylé en CSS.
- Les icônes fournies comme `ReactNode` ne reçoivent pas de traitement accessible automatique.
- Aucun `aria-label`, `aria-describedby` ou avertissement textuel automatique n'est ajouté pour `openInNewTab`.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : règles d'usage d'un lien, cardinalité, microcopy et différence design entre lien et bouton.
- `NON_CONFIRMÉ` : accessibilité effective des icônes et annonce d'ouverture dans un nouvel onglet.
- `NON_CONFIRMÉ` : conformité WCAG/RGAA.
- Vérifier dans le Storybook de la version installée : statut de chantier `Components/Link 🚧` et rendu des classes bouton.
