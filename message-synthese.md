# Message — Synthèse

Synthèse d'implémentation du composant `Message` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Message affichant un contenu, éventuellement un titre, une icône et une action. | DOCUMENTÉ |
| Quand l'utiliser | Non précisé explicitement dans les sources. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Non précisé explicitement dans les sources. | NON_CONFIRMÉ |
| Cardinalité par page | Non précisé explicitement dans les sources. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `Message`.
- Exports associés : `messageVariants`, `MessageVariants`.
- Exports publics : `prospect.ts` réexporte `MessageApollo`, `client.ts` réexporte `MessageLF`.
- Élément racine rendu et classe CSS de base : `<section class="af-message af-message--{variant}">`.
- Dépendances internes : `IconApollo`/`IconLF`, `Link`, types `ButtonProps`, `getRoleFromVariant`.

```tsx
import { Message } from "@axa-fr/canopee-react/prospect";

<Message variant="error">My error message</Message>;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `variant` | `MessageVariants` | `messageVariants.information` | Détermine classe, icône et rôle ARIA. | IMPLÉMENTÉ |
| `title` | `string` | Aucun | Titre rendu si fourni. | IMPLÉMENTÉ |
| `children` | `ReactNode` | Aucun | Contenu principal. | IMPLÉMENTÉ |
| `action` | `ReactElement<typeof Link \| ComponentType<ButtonProps>>` | Aucun | Action rendue dans `.af-message__action`. | IMPLÉMENTÉ |
| `iconSize` | `number` | `24` | Largeur et hauteur de l'icône en pixels. | IMPLÉMENTÉ |
| `heading` | `"h2" \| "h3" \| "h4" \| "h5" \| "h6"` | `"h4"` | Élément HTML utilisé pour le titre. | IMPLÉMENTÉ |
| Props natives | `ComponentPropsWithoutRef<"section">` | Selon React/HTML | Transmises à la section racine. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Information | `information` | `af-message--information` | IMPLÉMENTÉ |
| Erreur | `error` | `af-message--error` | IMPLÉMENTÉ |
| Avertissement | `warning` | `af-message--warning` | IMPLÉMENTÉ |
| Neutre | `neutral` | `af-message--neutral` | IMPLÉMENTÉ |
| Validation | `validation` | `af-message--validation` | IMPLÉMENTÉ |

`messageVariants` et `iconByVariant` définissent ces valeurs et les icônes associées.

## États et comportements

- `information`, `neutral` et `validation` produisent `role="status"`.
- `error` et `warning` produisent `role="alert"`.
- L'icône est choisie automatiquement selon la variante.
- Le composant ne gère pas d'ouverture, fermeture, chargement ou désactivation.
- Une action facultative peut être rendue ; cette capacité technique n'est pas une règle d'usage design.

## Anatomie

```html
<section class="af-message af-message--information" role="status|alert">
  <Icon class="af-message__icon" role="presentation" />
  <div class="af-message__content">
    <h4 class="af-message__title">Titre</h4>
    <p>Contenu du message</p>
    <div class="af-message__action">Action optionnelle</div>
  </div>
</section>
```

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Commun | `padding: var(--rem-16)`, `border: 1px solid var(--message-border-color)`, `gap: var(--rem-8)` | OBSERVÉ |
| Variables | `--message-theme-color`, `--message-bg-color`, `--message-border-color`, `--message-icon-color` | OBSERVÉ |
| Typographie | `--message-title-font-size`, `--message-content-font-size`, `--message-content-line-height` | OBSERVÉ |
| Responsive | `@media (--desktop-small)` ; le titre passe de 16 à 18 dans les CSS d'univers | OBSERVÉ |
| Prospect | Fonds colorés : bleu 040, rouge 040, gris 050, vert 040 selon variante ; rayon `var(--radius-8)` | OBSERVÉ |
| Client | Fond `var(--white-1000)`, rayon `var(--radius-12)`, couleurs de thème par variante | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Export | `MessageApollo` | `MessageLF` | IMPLÉMENTÉ |
| Icône | `IconApollo` | `IconLF` | IMPLÉMENTÉ |
| CSS | `MessageApollo.css` | `MessageLF.css` | IMPLÉMENTÉ |
| Fond | Fonds colorés par variante | Fond blanc | OBSERVÉ |
| Rayon | `var(--radius-8)` | `var(--radius-12)` | OBSERVÉ |
| Couleur erreur | `var(--red-alert-1000)` | `var(--red-alert-1200)` | OBSERVÉ |

## Accessibilité

- Rôle `alert` ou `status` calculé selon la variante.
- Icône avec `role="presentation"`.
- Niveau de titre configurable de `h2` à `h6`.
- Props natives de `<section>` transmises.
- Aucun mécanisme de focus ou de clavier n'est implémenté directement.
- L'annonce dynamique effective dépend de l'insertion/mise à jour dans la page.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : règles d'usage, cardinalité, microcopy, ordre de priorité des messages.
- `NON_CONFIRMÉ` : comportement d'annonce lorsque le contenu change dynamiquement.
- `NON_CONFIRMÉ` : conformité WCAG/RGAA complète.
- Vérifier dans le Storybook de la version installée : rendus de variantes et actions.
