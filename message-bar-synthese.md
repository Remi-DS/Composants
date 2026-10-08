# MessageBar — Synthèse

Synthèse d'implémentation du composant `MessageBar` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Bannière pleine largeur destinée à afficher une information importante. | DOCUMENTÉ |
| Description responsive | Le MDX indique une description repliable sur petits écrans et affichée directement sur desktop. | DOCUMENTÉ |
| Quand l'utiliser | Pour afficher une information importante, selon le MDX ; critères précis non détaillés. | DOCUMENTÉ |
| Quand ne pas l'utiliser | Non précisé dans les sources. | NON_CONFIRMÉ |
| Cardinalité par page | Non précisée dans les sources. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `MessageBar`.
- Exports associés : `MessageBarProps`, `MessageBarVariant`.
- Exports publics : `prospect.ts` réexporte `MessageBarApollo`, `client.ts` réexporte `MessageBarLF`.
- Élément racine rendu : `<section class="af-message-bar af-message-bar--info">` ou `AccordionCore` sur petit écran avec description.
- Dépendances internes : `AccordionCore`, `Button`, `Icon`, `MessageBarSummary`, `MessageBarDescription`, `MessageBarAction`, `useIsSmallScreen(BREAKPOINT.MD)`.

```tsx
import { MessageBar } from "@axa-fr/canopee-react/prospect";

<MessageBar title="Information importante" icon="info" description="Détail du message" />;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `title` | `string` | Aucun | Titre de la bannière et cible possible de `aria-labelledby`. | IMPLÉMENTÉ |
| `description` | `ReactNode` | Aucun | Description affichée ou repliable selon écran. | IMPLÉMENTÉ |
| `icon` | `string` | Aucun | Source de l'icône. | IMPLÉMENTÉ |
| `variant` | `"info" \| "error"` | `"info"` | Détermine les couleurs et la variante d'icône. | IMPLÉMENTÉ |
| `defaultDescriptionOpen` | `boolean` | `false` | État initial de la description mobile. | IMPLÉMENTÉ |
| `buttonProps` | `ButtonProps` | Aucun | Rend un bouton d'action si fourni. | IMPLÉMENTÉ |
| Props natives | `Omit<ComponentPropsWithoutRef<"section">, "children">` | Selon React/HTML | Transmises à la section ou à l'accordéon racine. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Information | `info` | `af-message-bar--info` | IMPLÉMENTÉ |
| Erreur | `error` | `af-message-bar--error` | IMPLÉMENTÉ |

`MessageBarVariant` vaut `"info" | "error"`. La correspondance d'icône est `info -> primary` et `error -> error`.

## États et comportements

- Avec `description` et sur petit écran : rendu via `AccordionCore`, avec rôle `region`.
- Sans description ou sur desktop : rendu via `<section>`.
- `defaultDescriptionOpen` initialise l'ouverture de la description mobile.
- Le bouton d'action n'est rendu que si `buttonProps` est fourni.
- Un identifiant de titre est généré avec `useId`.
- `aria-labelledby` est ajouté automatiquement sauf si `aria-label` ou `aria-labelledby` est fourni.

## Anatomie

```html
<section class="af-message-bar af-message-bar--info">
  <div class="af-message-bar__header">
    <Icon class="af-message-bar__icon" />
    <p class="af-message-bar__title">Information importante</p>
  </div>
  <div class="af-message-bar__body">Description</div>
  <button class="af-message-bar__action">Action</button>
</section>
```

Sur mobile avec description, `AccordionCore` porte la racine et `summaryProps.className` vaut `af-message-bar__header`.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Variables | `--message-bar-bg-color`, `--message-bar-color`, `--message-bar-body-color` | OBSERVÉ |
| Lignes | `--message-bar-title-line-height`, `--message-bar-body-line-height` | OBSERVÉ |
| Mobile | Padding supérieur `var(--rem-16) 0 var(--rem-16) var(--rem-12)` ; contrôles `var(--rem-16) var(--rem-12)` | OBSERVÉ |
| Corps mobile | Padding `0 var(--rem-16)` | OBSERVÉ |
| Desktop | `@media (--desktop-small)` : padding horizontal `var(--rem-120)` | OBSERVÉ |
| Desktop typo | Titre `var(--rem-20)`, corps `var(--rem-18)`, lignes `var(--rem-25)`/`var(--rem-23)` | OBSERVÉ |
| Variante info | Fond `var(--blue-040)`, couleur `var(--blue-1000)`, corps `var(--gray-1000)` | OBSERVÉ |
| Variante error | Fond `var(--red-040)`, couleur `var(--red-alert-1000)`, corps `var(--gray-1000)` | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Composant commun | `MessageBarCommon` | `MessageBarCommon` | IMPLÉMENTÉ |
| Accordion | `AccordionCoreApollo` | `AccordionCoreLF` | IMPLÉMENTÉ |
| Bouton | `ButtonApollo` | `ButtonLF` | IMPLÉMENTÉ |
| Icône | `IconApollo` | `IconLF` | IMPLÉMENTÉ |
| CSS | `MessageBarAll.css` | `MessageBarAll.css` | OBSERVÉ |

## Accessibilité

- Sur mobile avec description, rôle `region`.
- `aria-labelledby` pointe vers l'identifiant généré du titre si l'appelant ne fournit pas de nom accessible.
- L'accordéon délègue le comportement d'ouverture/fermeture à `AccordionCore`.
- Le bouton d'action dépend des props et de l'implémentation de `Button`.
- Aucun mécanisme de fermeture globale ou de persistance d'état n'est documenté pour `MessageBar`.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : règles de priorité, durée d'affichage, positionnement et cardinalité.
- `NON_CONFIRMÉ` : comportement attendu après fermeture ou lecture sur mobile.
- `NON_CONFIRMÉ` : conformité WCAG/RGAA complète.
- Vérifier dans le Storybook de la version installée : bascule mobile/desktop de la description.
