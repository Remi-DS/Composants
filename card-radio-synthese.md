# CardRadio — Synthèse

Synthèse d'implémentation du composant `CardRadio` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Carte sélectionnable unitaire, utilisée dans `CardRadioGroup`, avec label, icône ou image optionnelle, description et sous-titre. Source : `CardRadio.mdx`. | DOCUMENTÉ |
| Quand l'utiliser | À confirmer dans le Zeroheight de l'univers ; le MDX indique seulement l'usage dans `CardRadioGroup`. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Non présent dans les sources techniques. | NON_CONFIRMÉ |
| Cardinalité par page | Non présent dans les sources techniques. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `CardRadio` depuis `prospect.ts` et `client.ts`.
- Exports associés : le type `CardRadioProps` existe dans `CardRadioCommon.tsx` mais n'est pas exporté publiquement par `prospect.ts`/`client.ts`.
- Élément racine rendu et classe CSS de base : `<label className="af-card-radio">`.
- Dépendances internes : `Radio` Prospect ou Client, `Icon` Prospect ou Client, `BasePicture`, `getClassName`.

```tsx
import { CardRadio } from "@axa-fr/canopee-react/prospect";

<CardRadio name="city" value="paris" label="Paris" description="Capitale de la France" />;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `label` | `ReactNode` | — | Affiché dans `p.af-card-radio__label`. | IMPLÉMENTÉ |
| `description` | `ReactNode` | — | Rend `p.af-card-radio__description` si truthy. | IMPLÉMENTÉ |
| `subtitle` | `ReactNode` | — | Rend `p.af-card-radio__subtitle` si truthy. | IMPLÉMENTÉ |
| `position` | `"vertical" \| "horizontal"` | `undefined` côté code ; story : `"vertical"` | Ajoute `af-card-radio--horizontal` si `horizontal`. | IMPLÉMENTÉ / DOCUMENTÉ |
| `icon` | `Icon["src"]` | — | Rend `IconComponent` avec `role="presentation"`. | IMPLÉMENTÉ |
| `src` | `BasePicture["src"]` | — | Rend `BasePicture` seulement si `position="horizontal"` et `src` fourni. | IMPLÉMENTÉ |
| `basePictureProps` | `Omit<BasePictureProps, "src">` | — | Transmis à `BasePicture` après `src`. | IMPLÉMENTÉ |
| `variant` | hérité de `Radio` | — | Transmis au `Radio`; `error` ajoute le modificateur CSS `invalid`. | IMPLÉMENTÉ |
| Props radio natives | `Omit<ComponentProps<typeof Radio>, "size">` | — | Transmises au composant `RadioComponent`. | IMPLÉMENTÉ |

Préciser l'héritage : `CardRadioProps` étend les props de `Radio` sauf `size`.

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Orientation | `position="horizontal"` | `af-card-radio--horizontal` | IMPLÉMENTÉ |
| État visuel | `variant="error"` | `af-card-radio--invalid` | IMPLÉMENTÉ |
| État story | `variant="warning"` | transmis au `Radio`, pas de style carte dédié observé | DOCUMENTÉ / OBSERVÉ |

## États et comportements

Le composant ne gère pas lui-même l'état coché ; il s'appuie sur l'`input` radio rendu par `RadioComponent`.
Le CSS utilise `:has(input:checked)` pour modifier fond, bordure et graisse du label.
Les états hover, `:focus-visible` et `:focus-within` augmentent l'épaisseur de contour.
Un état invalide non coché force une bordure plus épaisse ; l'univers Prospect ajoute un fond rouge clair.
`disabled` est hérité du radio natif mais la carte garde `cursor: pointer` dans le CSS lu.

## Anatomie

Structure rendue : `label.af-card-radio` > icône éventuelle > image éventuelle en horizontal > `div.af-card-radio__content` > label, description, subtitle > `RadioComponent`.
En horizontal, le radio reçoit `order: -1` et le contenu est aligné à gauche.
En vertical, Client positionne `.af-radio` en absolu en haut à gauche ; Prospect lui applique une marge haute.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Commun | `padding: var(--rem-16)`, `display: flex`, `outline` interne, `gap: var(--radio-gap)`. | OBSERVÉ |
| Label | `font-size: var(--rem-16)`, desktop `var(--rem-18)`, `font-weight: 600` si coché. | OBSERVÉ |
| Description / subtitle | `var(--rem-14)`, desktop `1rem`, couleur `--radio-color-subtitle`. | OBSERVÉ |
| Prospect | bordure `var(--blue-650)`, rayon `var(--radius-8)`, checked `var(--blue-040)` / `var(--blue-1000)`, invalid `var(--red-alert-1000)` et `var(--red-040)`. | OBSERVÉ |
| Client | bordure `var(--gray-800)`, rayon `var(--radius-4)`, checked `var(--blue-040)` / `var(--blue-1000)`, invalid `var(--red-alert-1200)`. | OBSERVÉ |
| Responsive | `@media (--desktop-small)` augmente icône et typographies. | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Sous-composants | `RadioApollo`, `IconApollo` | `RadioLF`, `IconLF` | IMPLÉMENTÉ |
| Bordure initiale | `--blue-650` | `--gray-800` | OBSERVÉ |
| Rayon | `--radius-8` | `--radius-4` | OBSERVÉ |
| Invalide | fond `--red-040` et titre `--gray-800` | bordure `--red-alert-1200` sans fond dédié observé | OBSERVÉ |

## Accessibilité

Le MDX indique que l'option est un `<label>` enveloppant le contrôle radio et demande un `label` descriptif.
L'icône décorative reçoit `role="presentation"`.
Les responsabilités clavier et ARIA sont documentées comme prises en charge par `CardRadioGroup` lorsqu'il est utilisé dans le groupe.
La conformité WCAG/RGAA n'est pas certifiée par les sources lues.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : règles d'usage design hors mention d'usage dans `CardRadioGroup`, cas d'exclusion, cardinalité et microcopy.
- `NON_CONFIRMÉ` : comportement design autorisé pour `warning` au niveau carte ; seule la possibilité technique est exposée en story.
- Vérifier dans le Storybook de la version installée : rendu disabled, combinaison `src` + `icon`, et styles focus réels par univers.
