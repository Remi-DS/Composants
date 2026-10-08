# Heading — Synthèse

Synthèse d'implémentation du composant `Heading` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Titre typographique `h1` à `h4` avec sous-titres, icône et tag optionnels ; les stories montrent `Heading`, `All` et `IconOnTop`. | DOCUMENTÉ / IMPLÉMENTÉ |
| Quand l'utiliser | Non documenté comme règle d'usage ; vérifier le Zeroheight Prospect ou Client. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Non documenté dans les MDX/stories lues. | NON_CONFIRMÉ |
| Cardinalité par page | Non documentée ; `level={1}` par défaut n'établit pas de règle design. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `Heading` ; exports associés : `HeadingLevel`, `HeadingProps`.
- Types et objets associés : `HeadingLevel = 1 | 2 | 3 | 4`, `headingLevelToIconSizeDesktop`, `headingLevelToIconSizeMobile`.
- Élément racine rendu et classe CSS de base : `<div className="af-heading">`, enrichi des modificateurs calculés par `getClassName`.
- Dépendances internes : `HeadingCommon`, `HeadingWithSubheadings`, `Icon`, `Tag`, `useIsSmallScreen(BREAKPOINT.SM)`, `getClassName`.
- `HeadingProps = HeadingCommonProps & { tagProps?: Omit<TagProps, "children"> }`.

```tsx
import { Heading } from "@axa-fr/canopee-react/prospect";

<Heading firstSubtitle="Subtitle" level={3}>
  Heading
</Heading>;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `children` | `ReactNode` | — | Utilisé comme titre dans `HeadingWithSubheadings`. | IMPLÉMENTÉ |
| `level` | `1 | 2 | 3 | 4` | `1` | Détermine le composant `h1` à `h4` et la taille d'icône. | IMPLÉMENTÉ |
| `icon` | `string` | `undefined` | Si présent, rend `Icon` avec `src={icon}`, `hasBackground`, `variant="secondary"`. | IMPLÉMENTÉ |
| `iconProps` | `Omit<IconProps, "src">` | `{}` | Étendu sur `Icon`; la MDX indique que toutes les props d'icône sauf `src` sont acceptées. | IMPLÉMENTÉ / DOCUMENTÉ |
| `iconPosition` | `"start" | "top"` | `"start"` | Ajoute le modificateur `af-heading--icon-top` quand vaut `top`. | IMPLÉMENTÉ / DOCUMENTÉ |
| `tag` | `ReactNode` | `undefined` | Si présent et `level < 3`, rendu dans `.af-heading__label`. | IMPLÉMENTÉ |
| `tagProps` | `Omit<TagProps, "children">` | `{}` | Fusionné avec `DEFAULT_TAG_PROPS` puis transmis à `Tag`. | IMPLÉMENTÉ / DOCUMENTÉ |
| `firstSubtitle` | `ReactNode` | `undefined` | Rend un `<p className="af-heading__subtitle">`. | IMPLÉMENTÉ |
| `secondSubtitle` | `ReactNode` | `undefined` | Rendu seulement si le composant titre est `h1`. | IMPLÉMENTÉ |
| `className` | `string` | `undefined` | Ajouté à `.af-heading`. | IMPLÉMENTÉ |
| Props `<div>` | `JSX.IntrinsicElements["div"]` | — | Propagées sur la racine. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Niveau | `1`, `2`, `3`, `4` | `h1`, `h2`, `h3`, `h4` dans `.af-heading__title`. | IMPLÉMENTÉ |
| Position icône | `start` | Pas de modificateur ; grid avec colonne icône si présente. | IMPLÉMENTÉ |
| Position icône | `top` | `.af-heading--icon-top`. | IMPLÉMENTÉ |
| Tag par défaut | `DEFAULT_TAG_PROPS` | `Tag` avec `variant: "neutral"`. | IMPLÉMENTÉ |
| Univers Prospect | `HeadingApollo` | Importe `HeadingApollo.css` et `TagApollo`. | IMPLÉMENTÉ |
| Univers Client | `HeadingLF` | Importe `HeadingLF.css` et `TagLF`. | IMPLÉMENTÉ |

## États et comportements

- La taille d'icône est responsive : mobile (`BREAKPOINT.SM`) utilise `{1:"S",2:"S",3:"S",4:"XS"}`, desktop utilise `{1:"L",2:"M",3:"S",4:"S"}`. | IMPLÉMENTÉ |
- `HeadingWithSubheadings` rend un `<hgroup className="af-heading__title-container">`. | IMPLÉMENTÉ |
- `titleComponent` vaut `h1` par défaut dans `HeadingWithSubheadings`, mais `HeadingCommon` transmet toujours `h${level}`. | IMPLÉMENTÉ |
- `secondSubtitle` n'est rendu que pour `h1`. | IMPLÉMENTÉ |
- Le tag est techniquement fourni à tous les niveaux, mais rendu seulement si `level < 3`; cela n'autorise pas une règle design non documentée. | IMPLÉMENTÉ / RECOMMANDATION |

## Anatomie

- `.af-heading` est la racine en grille. | IMPLÉMENTÉ |
- Si `tag && level < 3`, `.af-heading__label` précède l'icône et le titre. | IMPLÉMENTÉ |
- Si `icon`, `Icon` reçoit la classe `af-heading__icon` en plus de `iconProps.className`. | IMPLÉMENTÉ |
- `.af-heading__title-container` contient `.af-heading__title` puis zéro, un ou deux `.af-heading__subtitle`. | IMPLÉMENTÉ |
- La story `All` rend les quatre niveaux dans `.af-heading-client-demo`. | DOCUMENTÉ |

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Commun racine | `--heading-column-gap: var(--rem-8)`, `display: grid`, `align-items: start`, `gap: var(--rem-8) var(--heading-column-gap)`. | OBSERVÉ |
| Commun h1 | `&:has(h1) { --heading-column-gap: var(--rem-12); }`. | OBSERVÉ |
| Avec icône | `grid-template-columns: auto 1fr`; `.af-heading__icon` : `border-radius: 0.75rem`, `grid-row: 1/3`, ombre `rgba(var(--blue-1000), 0.15)`. | OBSERVÉ |
| Icône top | `.af-heading--icon-top:has(.af-heading__icon)` met `--heading-grid-column: 1`, `grid-template-columns: 1fr`; icône `justify-self: start`. | OBSERVÉ |
| Sous-titre | `font-weight: 400`, `line-height: 125%`, couleur `var(--heading-subtitle-color)`. | OBSERVÉ |
| Label | `.af-heading__label` : `margin-block-end: var(--rem-4)`, `grid-column: span 3`. | OBSERVÉ |
| Prospect mobile | H1 `var(--blue-1000)`, `var(--rem-28)/var(--rem-35)`, poids `700`, Publico headline ; H2 `var(--rem-24)/var(--rem-30)`, poids `300`, Publico ; H3 `var(--rem-20)/var(--rem-25)`, poids `600`; H4 `var(--rem-18)/var(--rem-23)`. | OBSERVÉ |
| Client mobile | H1 `var(--blue-1000)`, `var(--rem-28)/var(--rem-35)`, poids `700`; H2 `var(--gray-1000)`, `var(--rem-24)/var(--rem-30)`, poids `700`; H3 `var(--rem-20)/var(--rem-25)`, poids `600`, sous-titre `var(--rem-14)`; H4 `var(--rem-18)/var(--rem-23)`, poids `600`, sous-titre `var(--rem-14)`. | OBSERVÉ |
| Desktop | `@media (--desktop-small)` : sous-titre `var(--rem-18)` ; H1 `var(--rem-40)/var(--rem-50)`, H2 `var(--rem-32)/var(--rem-40)`, H3 `var(--rem-24)/var(--rem-30)`, H4 `var(--rem-20)/var(--rem-25)`. | OBSERVÉ |
| Story CSS | `.af-heading-client-demo` : `display: grid`, `grid-template-columns: 1fr`, `row-gap: 3rem`. | OBSERVÉ |

Les variables CSS et media queries proviennent du CSS source et peuvent évoluer.

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Tag | Utilise `TagApollo`. | Utilise `TagLF`. | IMPLÉMENTÉ |
| H2 | Bleu `var(--blue-1000)`, poids `300`, `font-family: var(--font-family-publico)`. | Couleur de titre par défaut `var(--gray-1000)`, poids `700`, Publico headline. | OBSERVÉ |
| H3 | Bleu `var(--blue-1000)`, pas de variable spécifique de sous-titre. | Couleur par défaut grise, sous-titre `var(--rem-14)` en mobile et `var(--rem-16)` desktop. | OBSERVÉ |
| Icône de story | `account_balance.svg`. | `account_balance_wallet-fill.svg`. | DOCUMENTÉ |
| Libellés story | `Heading`, `Subtitle`, `Sous Titre`. | `Titre de la page`, `Sous titre`. | DOCUMENTÉ |

## Accessibilité

- Le titre est un élément sémantique `h1`, `h2`, `h3` ou `h4` selon `level`. | IMPLÉMENTÉ |
- Les sous-titres sont des paragraphes dans un `<hgroup>`. | IMPLÉMENTÉ |
- Les props `iconProps` peuvent porter des attributs d'accessibilité d'icône, mais aucune règle de texte alternatif n'est documentée ici. | NON_CONFIRMÉ |
- Aucune conformité WCAG/RGAA ni hiérarchie de titres obligatoire n'est certifiée par les sources lues. | NON_CONFIRMÉ |

## Limites et points à confirmer

- `NON_CONFIRMÉ` : règles d'usage, choix du niveau sémantique, nombre de `h1`, longueur des titres/sous-titres, microcopy et usage du tag ; vérifier le Zeroheight Prospect ou Client.
- Vérifier dans le Storybook de la version installée : tailles exactes rendues par univers, comportement `hgroup`, rendu du tag aux niveaux 3 et 4, et accessibilité des icônes.
