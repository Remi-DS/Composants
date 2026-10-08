# DataAgent — Synthèse

Synthèse d'implémentation du composant `DataAgent` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas toutes dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Le MDX montre un bloc de données agent avec identité, contrat rattaché, informations et actions. | DOCUMENTÉ |
| Quand l'utiliser | Usage métier exact à confirmer dans le Zeroheight Prospect ou Client. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Aucun cas d'exclusion n'est publié dans les MDX lus. | NON_CONFIRMÉ |
| Cardinalité par page | Aucune limite par page n'est publiée. | NON_CONFIRMÉ |
| Variante compacte | `variant="compact"` force l'en-tête seul, quel que soit le viewport ; le MDX le cite pour un layout contraint comme une sidebar desktop. | DOCUMENTÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `DataAgent`, type `DataAgentProps`.
- Exports associés : aucun objet de variantes exporté ; le type local accepte `variant?: "default" | "compact"`.
- Élément racine rendu et classe CSS de base : `<section className="af-data-agent">`.
- Dépendances internes : `ContentItemMono`, `ClickItem`, `Divider`, `useIsSmallScreen(BREAKPOINT.SM)`, `getClassName`.

```tsx
import { DataAgent } from "@axa-fr/canopee-react/prospect";

<DataAgent agentProps={{ picture: "https://dummyimage.com/48/48/fff&text=A", title: "Michel Lhote", subtitle: "AXA Assurance & Banque", type: "picture" }} variant="compact" />;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `className` | `string` | `undefined` | Ajoutée à `af-data-agent` via `getClassName`. | IMPLÉMENTÉ |
| `agentProps` | `ContentMonoItemPictureProps` | requis | Rend l'identité agent en `ContentItemMono` `type="picture"` ou en `ClickItem` compact. | IMPLÉMENTÉ |
| `agentContractProps` | `ContentMonoItemStickProps` | `undefined` | Rend un second `ContentItemMono` `type="stick"` si fourni. | IMPLÉMENTÉ |
| `contents` | `TupleMax3<ContentMonoItemIconProps>` | `undefined` | Rend jusqu'à trois contenus en `ContentItemMono` `type="icon"`, séparés par `Divider`. | IMPLÉMENTÉ |
| `clickContents` | `TupleMax3<ClickItemProps>` | `undefined` | Rend jusqu'à trois `ClickItem` avec `variant="small"`, séparés par `Divider`. | IMPLÉMENTÉ |
| `texteOrias` | `string` | `undefined` | Affiche un paragraphe `af-data-agent__text-orias` si la valeur est truthy. | IMPLÉMENTÉ |
| `isCompact` | `boolean` | `true` | Sur petit écran, active le rendu compact si `true`. | IMPLÉMENTÉ |
| `variant` | `"default" \| "compact"` | `"default"` | `compact` force le rendu compact sans dépendre de la taille d'écran. | IMPLÉMENTÉ |

Pas d'héritage de props natives HTML déclaré.

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Layout par défaut | `variant="default"` | `af-data-agent` | IMPLÉMENTÉ |
| Layout compact | `variant="compact"` | `af-data-agent` avec seulement `af-data-agent__intro` | IMPLÉMENTÉ |

## États et comportements

- Le rendu compact est utilisé si `(isMobile && isCompact) || variant === "compact"`.
- Le rendu compact transforme `agentProps` en `ClickItem` `variant="agent"` et fournit `basePictureProps` avec `src: agentProps.picture` et `alt: agentProps.title`.
- Les listes `contents` et `clickContents` ne sont rendues que si elles existent et contiennent au moins un élément.
- Les clés de liste utilisent `crypto.randomUUID()`, donc elles changent à chaque rendu.
- Les stories Prospect et Client fournissent `isCompact: true` dans l'exemple `Default`.

## Anatomie

- Racine : `<section className="af-data-agent">`.
- Layout défaut : `section.af-data-agent__intro`, `Divider`, `section.af-data-agent__info-content`, `section.af-data-agent__info-click-content`, puis `p.af-data-agent__text-orias`.
- Layout compact : `section.af-data-agent__intro` contenant un `ClickItem`.
- Les séparateurs internes reçoivent parfois `className="af-data-agent__line"`.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Base | `display: flex`, `flex-direction: column`, `padding-bottom: var(--rem-16)`, bordure `1px solid var(--data-agent-border-color)`, rayon `var(--radius-8)`. | OBSERVÉ |
| Intro | `padding: var(--data-agent-intro-padding)`, `gap: var(--rem-16)`. | OBSERVÉ |
| Contenus | `gap: var(--rem-12)`, padding `var(--data-agent-intro-content-padding)` ou `var(--rem-16) var(--rem-16) 0`. | OBSERVÉ |
| Texte Orias | `font-size: var(--data-agent-text-orias-font-size)`, `line-height: 125%`, couleur `var(--data-agent-text-orias-font-color)`. | OBSERVÉ |
| Compact CSS | `.af-data-agent:has(> .af-data-agent__intro:only-child)` retire le padding bas et met l'intro à `var(--rem-16)`. | OBSERVÉ |
| Tokens Apollo | `--data-agent-border-color: var(--blue-200)`, texte Orias `var(--gray-800)`, taille `var(--rem-14)`. | OBSERVÉ |
| Tokens LF | Même base qu'Apollo, avec `@media (--desktop-small)` passant le texte Orias à `var(--rem-16)`. | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Sous-composants | Utilise `ClickItemApollo`, `ContentItemMonoApollo`, `DividerApollo`. | Utilise `ClickItemLF`, `ContentItemMonoLF`, `DividerLF`. | IMPLÉMENTÉ |
| CSS | Importe `@axa-fr/canopee-css/prospect/DataAgent/DataAgentApollo.css`. | Importe `@axa-fr/canopee-css/client/DataAgent/DataAgentLF.css`. | IMPLÉMENTÉ |
| Responsive CSS | Pas de media query spécifique dans `DataAgentApollo.css`. | `@media (--desktop-small)` augmente la taille du texte Orias. | OBSERVÉ |
| Stories | Exemple avec `account_balance` et action `Nous contacter`. | Exemple avec `account_balance_wallet`, `call`, `fax`. | DOCUMENTÉ |

## Accessibilité

- Le composant racine utilise une sémantique de `section`, sans `aria-label` injecté automatiquement.
- En compact, l'alternative de l'image est alimentée par `agentProps.title`.
- Les actions proviennent de `ClickItem`; les attributs ARIA exacts de ce sous-composant ne sont pas redécrits ici.
- La conformité WCAG/RGAA n'est pas certifiée par les sources lues.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : quand utiliser le bloc agent, quand l'éviter, cardinalité par page et règles de microcopy.
- `NON_CONFIRMÉ` : signification métier et validation du numéro Orias ; `texteOrias` est seulement affiché.
- Vérifier dans le Storybook de la version installée : rendu compact selon le breakpoint `BREAKPOINT.SM` et différences visuelles des sous-composants Prospect / Client.
