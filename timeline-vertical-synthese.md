# TimelineVertical — Synthèse

Synthèse d'implémentation du composant `TimelineVertical` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Le composant rend une section avec un tag, un titre et un contenu descriptif optionnel. | IMPLÉMENTÉ |
| Quand l'utiliser | Non décrit dans les sources lues ; à confirmer dans le Zeroheight Prospect ou Client. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Non décrit dans les sources lues ; ne pas déduire une règle de design du nom `TimelineVertical`. | NON_CONFIRMÉ |
| Cardinalité par page | Aucune limite par page ou par parcours n'est publiée dans les sources lues. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `TimelineVertical`.
- Exports associés : `TimelineVerticalProps` est exporté depuis les fichiers d'univers, mais pas réexporté par `prospect.ts` ou `client.ts`.
- Élément racine rendu et classe CSS de base : `<section className="af-timeline-vertical">`.
- Dépendances internes : `TimelineVerticalCommon`, `Tag` Apollo ou LF, `classnames`.

```tsx
import { TimelineVertical } from "@axa-fr/canopee-react/prospect";

export const Example = () => (
  <TimelineVertical tag="Étape 1" title="Envoi de documents via Sécur'AXA">
    Vous allez recevoir un e-mail d'accès à Sécur'AXA.
  </TimelineVertical>
);
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `title` | `string` | Aucun | Rendu dans `<h4 className="af-timeline-vertical__title">`. | IMPLÉMENTÉ |
| `tag` | `ReactNode` | Aucun | En API publique, contenu passé à un composant `Tag` via `<Tag {...tagProps}>{tag}</Tag>`. | IMPLÉMENTÉ |
| `children` | `ReactNode` via `PropsWithChildren` | Aucun | Si truthy, rendu dans `<main className="af-timeline-vertical__description">`. | IMPLÉMENTÉ |
| `className` | `string` | Aucun | Ajouté à la racine via `classNames("af-timeline-vertical", className)`. | IMPLÉMENTÉ |
| `tagProps` | `Omit<TagProps, "children">` | `{}` | Transmis au `Tag` interne ; `children` reste piloté par `tag`. | IMPLÉMENTÉ |
| `description` | Non présent dans le type | Aucun | Le MDX montre `description`, mais la story et le code utilisent `children`; cette prop n'est pas consommée par le composant. | DOCUMENTÉ / IMPLÉMENTÉ |

`TimelineVertical` n'hérite pas de props natives HTML via `ComponentPropsWithoutRef`.

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Variante propre à `TimelineVertical` | Aucune | Aucune classe de variante dans le composant. | IMPLÉMENTÉ |
| Variante du tag | Selon `tagProps` et l'API de `Tag` | Classes du composant `Tag`, non définies par `TimelineVertical`. | IMPLÉMENTÉ |

La possibilité technique de passer `tagProps` n'est pas une autorisation de design pour changer le style du tag.

## États et comportements

- Aucun état interactif n'est implémenté dans `TimelineVerticalCommon`.
- Le contenu descriptif n'est rendu que si `Boolean(children)` vaut `true`.
- `tagProps` est initialisé à `{}` dans les composants Apollo et LF.
- Le tag est toujours encapsulé par le composant `Tag` de l'univers importé.
- Le MDX Prospect et Client documente un exemple avec `description`, mais la story fournie utilise `children`.

## Anatomie

- `section.af-timeline-vertical`.
- `header.af-timeline-vertical__header`.
- `Tag` interne contenant la prop `tag`.
- `h4.af-timeline-vertical__title` contenant `title`.
- `main.af-timeline-vertical__description` contenant `children`, uniquement si `children` est fourni.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Commun racine | `display: grid`, `row-gap: var(--rem-16)`, `border: 1px solid var(--timeline-vertical-border-color)`, `border-radius: var(--radius-8)` | OBSERVÉ |
| Commun titre | `font-weight: 600`, `line-height: 125%`, `color: var(--timeline-vertical-title-color)` | OBSERVÉ |
| Commun description | `font-weight: 400`, `line-height: 125%`, `color: var(--timeline-vertical-description-color)` | OBSERVÉ |
| Couleurs communes | Fond `var(--white-1000)`, bordure `var(--blue-200)`, titre `var(--gray-1000)`, description `var(--gray-800)` | OBSERVÉ |
| Prospect mobile | Padding `var(--rem-16)`, titre `var(--rem-16)`, description `var(--rem-16)` | OBSERVÉ |
| Prospect `@media (--desktop-small)` | Padding `var(--rem-24)`, titre `var(--rem-18)`, description `var(--rem-18)` | OBSERVÉ |
| Client mobile | Padding `var(--rem-24)`, titre `var(--rem-16)`, description `var(--rem-14)` | OBSERVÉ |
| Client `@media (--desktop-small)` | Titre `var(--rem-18)`, description `var(--rem-16)` ; padding inchangé dans le fichier LF. | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Tag interne | `Tag` depuis `TagApollo`. | `Tag` depuis `TagLF`. | IMPLÉMENTÉ |
| CSS | `TimelineVerticalApollo.css`. | `TimelineVerticalLF.css`. | IMPLÉMENTÉ |
| Padding initial | `var(--rem-16)`. | `var(--rem-24)`. | OBSERVÉ |
| Description initiale | `var(--rem-16)`. | `var(--rem-14)`. | OBSERVÉ |
| Description desktop | `var(--rem-18)`. | `var(--rem-16)`. | OBSERVÉ |
| Exemple MDX | Montre `description`, non conforme au type lu. | Montre `description`, non conforme au type lu. | DOCUMENTÉ |

## Accessibilité

- La racine est une `section`, sans `aria-label` ou `aria-labelledby` généré.
- Le titre est rendu en `h4`; le niveau n'est pas configurable par prop.
- Le contenu descriptif est rendu dans un élément `main` interne si `children` existe.
- Aucun comportement clavier, focus ou rôle ARIA spécifique n'est implémenté.
- La conformité WCAG/RGAA n'est pas certifiée par les sources lues.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : usage exact dans un parcours, nombre d'étapes attendu, cardinalité et règles de libellé des tags.
- `NON_CONFIRMÉ` : hiérarchie de titres à respecter autour du `h4` fixe.
- Vérifier dans le Storybook de la version installée si l'exemple MDX avec `description` est corrigé ou toujours contradictoire avec le code.
