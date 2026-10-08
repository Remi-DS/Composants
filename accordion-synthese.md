# Accordion — Synthèse

Synthèse d'implémentation du composant `Accordion` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Affiche un panneau repliable avec une zone de résumé personnalisable : icône, titre, sous-titre, tag, date et informations additionnelles. | DOCUMENTÉ |
| Interaction | Le clic sur le résumé ouvre/ferme le panneau ; si `onClick` est fourni, le handler reçoit l'événement natif. | DOCUMENTÉ / IMPLÉMENTÉ |
| Quand l'utiliser | Usage design précis non publié dans les sources techniques ; vérifier le Zeroheight de l'univers. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Non publié dans les sources lues. | NON_CONFIRMÉ |
| Cardinalité par page | Non publiée dans les sources lues. | NON_CONFIRMÉ |

## Intégration React

- Package Prospect : `@axa-fr/canopee-react/prospect`, export `Accordion`, `accordionVariants`, type `AccordionVariants`.
- Package Client : `@axa-fr/canopee-react/client`, mêmes exports publics.
- Fichiers : `AccordionCommon.tsx`, `AccordionApollo.tsx`, `AccordionLF.tsx`.
- Élément racine rendu par dépendance : `AccordionCore`, donc un `<details>` avec classe de base `af-apollo-accordion`.
- Dépendances internes : `AccordionCore`, `Tag`, `Icon`, `getClassName`.

```tsx
import { Accordion } from "@axa-fr/canopee-react/prospect";

<Accordion title="Titre onglet" info1="Information" info2="+ 686,00 €">
  Contenu de l'accordéon
</Accordion>;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `variant` | `"primary" \| "secondary"` | `"primary"` | Ajoute `af-apollo-accordion--primary` ou `--secondary`. | IMPLÉMENTÉ |
| `title` | `string` | — | Rend `<p class="af-accordion__title">`. | IMPLÉMENTÉ |
| `subtitle` | `string` | `undefined` | Rend `af-accordion__subtitle` si fourni. | IMPLÉMENTÉ |
| `icon` | `string` | `undefined` | Rend un `Icon` décoratif `role="presentation"`, `variant="primary"`, `size="S"`. | IMPLÉMENTÉ |
| `info1` | `string` | — | Type requis ; rendu conditionnel dans `af-accordion__info1`. | IMPLÉMENTÉ |
| `info2` | `string` | — | Type requis ; rendu conditionnel dans `af-accordion__info2`. | IMPLÉMENTÉ |
| `dateLabel` | `string` | `undefined` | Rend un `<time>` si fourni. | IMPLÉMENTÉ |
| `dateProps` | `Omit<ComponentProps<"time">, "children">` | `undefined` | Transmet les props au `<time>` et fusionne `className`. | IMPLÉMENTÉ |
| `tagLabel` | `string` | `undefined` | Rend un `Tag` interne si fourni. | IMPLÉMENTÉ |
| `tagProps` | `Omit<TagProps, "children">` | `undefined` | Transmet les props au `Tag`, avec `variant="warning"` posé avant le spread. | IMPLÉMENTÉ |
| `isPlain` | `boolean` | `undefined` | Ajoute le modificateur `plain`, donc `af-apollo-accordion--plain`. | IMPLÉMENTÉ |
| Props héritées | `Omit<AccordionCoreProps, "summary">` | — | Inclut `children`, `open`, `onClick`, `summaryProps`, `arrowClickIconVariant`, etc. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Primaire | `primary` | `af-apollo-accordion--primary` | IMPLÉMENTÉ |
| Secondaire | `secondary` | `af-apollo-accordion--secondary` | IMPLÉMENTÉ |
| Plain | `isPlain` | `af-apollo-accordion--plain` | IMPLÉMENTÉ |

Les MDX utilisent aussi des props `tag`, `date` et `info` dans les exemples, alors que le code lu expose `tagLabel`, `dateLabel`, `info1` et `info2`. Cette contradiction est conservée.

## États et comportements

- Le composant délègue l'ouverture à l'élément natif `<details>` via `AccordionCore`.
- `open` peut contrôler l'état expansé car hérité de `ComponentProps<"details">`.
- Si `onClick` est fourni à `AccordionCore`, le code appelle `event.preventDefault()` avant d'appeler le handler ; le basculement natif n'est donc pas automatique dans ce cas.
- L'icône de flèche est ajoutée par `AccordionCore` et pivote quand `[open]`.

## Anatomie

- Racine : `<details>` avec `af-apollo-accordion`, le modificateur de variante et les classes additionnelles éventuelles.
- Résumé : `<summary class="af-apollo-accordion__summary" tabIndex={0}>`.
- Enfants du résumé : icône optionnelle `af-accordion__icon`, titre, sous-titre, tag, date, `info1`, `info2`, puis `af-accordion__arrow`.
- Contenu : `children` rendu après le `<summary>`.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Commun | Grille pilotée par `--summary-areas`, `--summary-columns`, `--accordion-column-gap`, `--accordion-gap`. | OBSERVÉ |
| Prospect | Titre `var(--blue-1000)` en primaire ; secondaire titre `var(--gray-1000)`, date `var(--gray-800)`. | OBSERVÉ |
| Client | Titre `var(--gray-1000)` ; tailles desktop titre `var(--rem-18)`, sous-titre `var(--rem-16)`, info2 `var(--rem-18)`. | OBSERVÉ |
| Responsive | `@media (--desktop-small)` modifie les colonnes, zones de grille et tailles. | OBSERVÉ |
| Primaire | Mobile : zones empilées avec icon/title/arrow ; desktop : icon, title, tag, info1, info2, arrow. | OBSERVÉ |
| Secondaire | Masque l'icône et `info1` ; desktop place tag, date, title, info2, arrow. | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Implémentation React | `AccordionApollo` utilise `IconApollo` et `TagApollo`. | `AccordionLF` utilise `IconCommon` et `TagLF`. | IMPLÉMENTÉ |
| Couleur titre primaire | `var(--blue-1000)`. | `var(--gray-1000)`. | OBSERVÉ |
| Typographie desktop | `info1` passe à `var(--rem-16)`. | Titre, sous-titre et info2 augmentent aussi. | OBSERVÉ |

## Accessibilité

- Le composant s'appuie sur `<details>` et `<summary>`, avec `tabIndex={0}` explicitement posé sur le résumé.
- Les icônes décoratives reçoivent `role="presentation"`.
- `dateProps` permet d'ajouter `dateTime` sur le `<time>`.
- La conformité WCAG/RGAA n'est pas certifiée par les sources lues.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : critères d'usage, cas d'exclusion, cardinalité par page et microcopy.
- `NON_CONFIRMÉ` : correspondance exacte entre les exemples MDX (`tag`, `date`, `info`) et l'API réelle (`tagLabel`, `dateLabel`, `info1`, `info2`).
- Vérifier dans le Storybook de la version installée : rendu responsive des grilles primaire/secondaire et comportement avec `onClick` contrôlé.
