# Icon — Synthèse

Synthèse d'implémentation du composant `Icon` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Les MDX montrent l'affichage d'un SVG importé avec `src`, `variant` et `size`. | DOCUMENTÉ |
| Quand l'utiliser | La règle de design d'usage d'une icône n'est pas publiée dans les sources lues. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Aucun cas d'exclusion n'est documenté. | NON_CONFIRMÉ |
| Cardinalité par page | Aucune limite par page n'est indiquée. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `Icon`, `iconVariants`, `IconVariants`, `iconSizeVariants`, `IconSizeVariants`.
- Exports associés : les objets de variantes sont publics dans `prospect.ts` et `client.ts`.
- Élément racine rendu et classe CSS de base : `<div className="af-icon">`, enrichi des modificateurs de variante, taille et fond.
- Dépendances internes : `Svg`, `getClassName`, `useMemo`.

```tsx
import { Icon } from "@axa-fr/canopee-react/prospect";
import bank from "@material-symbols/svg-700/rounded/account_balance_wallet-fill.svg";

<Icon src={bank} variant="primary" size="S" />;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `variant` | `IconVariants` | `"primary"` | Ajoute le modificateur `af-icon--{variant}`. | IMPLÉMENTÉ |
| `size` | `IconSizeVariants` | `"S"` | Mappe `L/M/S/XS` vers `large/medium/small/extra-small`. | IMPLÉMENTÉ |
| `hasBackground` | `boolean` | `false` | Ajoute `af-icon--has-background` et active les tokens de fond/padding. | IMPLÉMENTÉ |
| `className` | `ComponentProps<typeof Svg>` | `undefined` | Ajoutée à la racine via `getClassName`. | IMPLÉMENTÉ |
| Props SVG | `ComponentProps<typeof Svg>` | selon prop | Transmises à `<Svg {...props} />`, dont `src` dans les stories. | IMPLÉMENTÉ |

Le type `IconProps` hérite de `ComponentProps<typeof Svg>` et ajoute `variant`, `size`, `hasBackground`.

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Couleur | `primary` | `af-icon--primary` | IMPLÉMENTÉ |
| Couleur | `secondary` | `af-icon--secondary` | IMPLÉMENTÉ |
| Couleur | `disabled` | `af-icon--disabled` | IMPLÉMENTÉ |
| Couleur | `success` | `af-icon--success` | IMPLÉMENTÉ |
| Couleur | `error` | `af-icon--error` | IMPLÉMENTÉ |
| Couleur | `warning` | `af-icon--warning` | IMPLÉMENTÉ |
| Taille | `L`, `M`, `S`, `XS` | `af-icon--large`, `--medium`, `--small`, `--extra-small` | IMPLÉMENTÉ |
| Fond | `hasBackground` | `af-icon--has-background` | IMPLÉMENTÉ |

## États et comportements

- Aucun état interactif n'est implémenté par `Icon`.
- Les contrôles Storybook exposent `variant` avec `Object.values(iconVariants)` et `size` avec `Object.keys(iconSizeVariants)`.
- L'icône rendue est encapsulée dans un `div`; les props restantes vont au composant `Svg`.
- `disabled` est une variante visuelle, pas un attribut HTML désactivant.

## Anatomie

- Racine : `div.af-icon` avec modificateurs de variante, taille et fond.
- Enfant : `<Svg {...props} />`.
- Le CSS cible le `svg` enfant pour fixer `width`, `height` et `fill`.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Tailles | `--icon-size-large: var(--rem-40)`, medium `var(--rem-32)`, small `var(--rem-24)`, extra-small `var(--rem-16)`. | OBSERVÉ |
| Base | `display: grid`, `width: fit-content`, `padding: var(--icon-padding)`, `border-radius: var(--icon-border-radius)`, fond `var(--icon-background-color, transparent)`. | OBSERVÉ |
| Primary | Fill `var(--blue-1000)` ; avec fond, fill blanc et fond bleu. | OBSERVÉ |
| Secondary | Fill blanc ; avec fond, fill bleu et outline solide `1px`. | OBSERVÉ |
| Disabled | Fill `var(--gray-500)` ; avec fond `var(--gray-050)`. | OBSERVÉ |
| Success / warning | Fill `var(--green-1000)` ou `var(--orange-1000)` ; avec fond, fill blanc et fond correspondant. | OBSERVÉ |
| Error commun | Avec fond, fill blanc. | OBSERVÉ |
| Responsive | Aucune media query dans les CSS `IconCommon`, `IconApollo` ou `IconLF`. | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Rayon | `--icon-border-radius: var(--radius-100)`. | `--icon-border-radius: var(--radius-12)`. | OBSERVÉ |
| Secondary avec fond | Fond et outline `var(--blue-040)`. | Fond `var(--white-1000)`, outline `var(--gray-050)`. | OBSERVÉ |
| Error | Fill et fond `var(--red-alert-1000)`. | Fill et fond `var(--red-alert-1200)`. | OBSERVÉ |
| React | Même `IconCommon`, CSS prospect importé. | Même `IconCommon`, CSS client importé. | IMPLÉMENTÉ |

## Accessibilité

- Aucun `aria-hidden`, `role` ou libellé accessible n'est ajouté automatiquement par `Icon`.
- Les attributs d'accessibilité éventuels doivent être passés via les props héritées de `Svg`, si ce composant les accepte.
- Une icône décorative doit être traitée côté intégration ; ce n'est pas automatisé ici.
- La conformité WCAG/RGAA n'est pas certifiée par les sources lues.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : choix des variantes selon le contexte, règles de combinaison fond/couleur et usage décoratif vs informatif dans Zeroheight.
- `NON_CONFIRMÉ` : source d'icônes autorisée ; les stories utilisent Material Symbols, mais aucune règle design générale n'est publiée dans les MDX lus.
- Vérifier dans le Storybook de la version installée : rendu exact des tokens par univers et accessibilité du composant `Svg`.
