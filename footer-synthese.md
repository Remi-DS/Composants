# Footer — Synthèse

Synthèse d'implémentation du composant `Footer` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Pied de page avec liens utiles, liens sociaux optionnels et copyright ; le code rend `<footer role="contentinfo">`. | IMPLÉMENTÉ |
| Quand l'utiliser | Règle d'usage non publiée dans les MDX/stories lues ; vérifier le Zeroheight Prospect ou Client. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Non documenté dans les sources techniques lues ; vérifier le Zeroheight de l'univers concerné. | NON_CONFIRMÉ |
| Cardinalité par page | Non documentée ; la sémantique `contentinfo` ne suffit pas à établir une règle design. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `Footer` ; export associé : `FooterProps`.
- Sous-types : `Link = { link: string; text: string; openInCurrentTab?: boolean }` et `SocialMedia = { icon: "facebook" | "twitter" | "youtube" | "linkedin"; link: string }`.
- Élément racine rendu et classe CSS de base : `<footer role="contentinfo" id={id} className="af-footer">`.
- Dépendances internes : `MenuLink`, `MenuIcons`, `DynamicIcon`, `Svg`, icône Material Symbols `keyboard_arrow_down`.
- Contradiction conservée : la MDX Apollo montre `@axa-fr/canopee-react/prospect`, mais sa story importe `client`; la MDX Look & Feel montre `client`, mais sa story importe `prospect`.

```tsx
import { Footer } from "@axa-fr/canopee-react/prospect";

<Footer
  links={links}
  socialMedias={socialMedias}
  copyright={copyright}
  expandLinkText={expandLinkText}
/>;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `links` | `Link[]` | — | Alimente `MenuLink`; si le tableau est vide, `MenuLink` retourne `null`. | IMPLÉMENTÉ |
| `socialMedias` | `SocialMedia[]` | `[]` | Alimente `MenuIcons`; si le tableau est vide, `MenuIcons` retourne `null`. | IMPLÉMENTÉ |
| `copyright` | `string` | — | Texte rendu dans `.af-footer__textCopyright`. | IMPLÉMENTÉ |
| `expandLinkText` | `string` | — | Libellé du bouton mobile et `aria-label` du nav principal. | IMPLÉMENTÉ |
| `id` | `string` | `undefined` | Transmis au `<footer>`. | IMPLÉMENTÉ |
| `Link.openInCurrentTab` | `boolean` | `undefined` | `true` rend `target="_top"`, sinon `target="_blank"`; `rel="noreferrer"`. | IMPLÉMENTÉ |
| `SocialMedia.icon` | union | — | `facebook`, `twitter`, `youtube`, `linkedin` chargent un SVG ; autre cas interne retournerait le texte. | IMPLÉMENTÉ |

Aucun héritage de props natives (`ComponentPropsWithoutRef`) n'est déclaré pour `FooterProps`.

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Univers Prospect | CSS `FooterApollo.css` | `.af-footer` avec `--footer-bg-color: var(--blue-1000)` et `--footer-color: var(--white-1000)` ; hover conserve `--footer-color`. | IMPLÉMENTÉ / OBSERVÉ |
| Univers Client | CSS `FooterLF.css` | `.af-footer` avec les mêmes variables de fond et de texte. | IMPLÉMENTÉ / OBSERVÉ |
| Ouverture mobile | `isAboutOpen` | `.af-footer__menuLinks--display`, `.af-footer__iconTrigger--display`. | IMPLÉMENTÉ |

Le composant n'expose pas de prop `variant`.

## États et comportements

- `isAboutOpen` est un état React interne initialisé à `false` et inversé par le bouton `.af-footer__menuAboutTrigger`. | IMPLÉMENTÉ |
- Sur petit écran (`useIsSmallScreen(BREAKPOINT.MD)`), les liens reçoivent `tabIndex={-1}` quand le panneau est fermé. | IMPLÉMENTÉ |
- Les liens de menu ouvrent par défaut un nouvel onglet (`target="_blank"`) ; `openInCurrentTab` force `target="_top"`. | IMPLÉMENTÉ |
- Les liens sociaux ouvrent `target="_blank"` avec `rel="noopener noreferrer"`. | IMPLÉMENTÉ |
- Les limites de nombre de liens, libellés ou réseaux sociaux ne sont pas documentées. | NON_CONFIRMÉ |

## Anatomie

- `.af-footer` contient `.af-footer__footerTop` puis `.af-footer__footerBottom`. | IMPLÉMENTÉ |
- `.af-footer__footerTop` contient un `<nav role="navigation" className="af-footer__menuTop" aria-label={expandLinkText}>`. | IMPLÉMENTÉ |
- Le bouton `.af-footer__menuAboutTrigger` contient `.af-footer__menuAboutTriggerText` et un `Svg` `.af-footer__icon.af-footer__iconTrigger`. | IMPLÉMENTÉ |
- `MenuLink` rend `<ul className="af-footer__menuLinks">`, des `<li>`, puis des `<a className="af-footer__linkItem">`. | IMPLÉMENTÉ |
- `MenuIcons` rend une navigation `af-footer__footerMenuIcons` contenant une liste de liens `af-footer__menuIconLinks`. | IMPLÉMENTÉ |
- `.af-footer__footerBottomWidth` enveloppe `.af-footer__textCopyright`. | IMPLÉMENTÉ |

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Racine | `z-index: 100`, `display: flex`, `flex-direction: column`, `justify-content: space-around`, `color: var(--footer-color)`, `background-color: var(--footer-bg-color)`. | OBSERVÉ |
| Menu fermé mobile | `.af-footer__menuLinks` : `all: unset`, `display: block`, `height: 0`, `overflow: hidden`, transition `0ms`. | OBSERVÉ |
| Menu ouvert mobile | `.af-footer__menuLinks--display` : `height: auto`, `padding: 0 2rem`, transitions `250ms` et `125ms`. | OBSERVÉ |
| Liens | `.af-footer__linkItem` : `display: block`, `padding: var(--rem-20) var(--rem-38)`, `text-decoration: none`, hover souligné. | OBSERVÉ |
| Réseaux sociaux | `ul` flex, `padding: var(--rem-26) var(--rem-13)`, bordure `rgba(255,255,255,20%)`, `gap: var(--rem-38)`. | OBSERVÉ |
| Déclencheur | `all: unset`, `width: 100%`, `padding: var(--rem-26) var(--rem-52)`, `cursor: pointer`. | OBSERVÉ |
| Icône | `.af-footer__icon` : `width: 1rem`, `height: 1rem`, `fill: var(--footer-color)` ; rotation via `--rotate-x`. | OBSERVÉ |
| Desktop | `@media (--desktop-small)` : liens en ligne, `.af-footer__menuAboutTrigger { display: none; }`, conteneurs max `128rem`. | OBSERVÉ |
| Bas de page desktop | `.af-footer__footerBottom` sans padding, bord haut `rgba(255,255,255,20%)`, copyright aligné à droite. | OBSERVÉ |

Les valeurs proviennent du CSS source et peuvent évoluer.

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Composant React | `FooterApollo.tsx` réexporte `FooterCommon` et importe le CSS Prospect. | `FooterLF.tsx` réexporte `FooterCommon` et importe le CSS Client. | IMPLÉMENTÉ |
| Variables | `--footer-bg-color: var(--blue-1000)`, `--footer-color: var(--white-1000)`. | Identique dans le CSS lu. | OBSERVÉ |
| Hover | Ajoute `.af-footer__linkItem:hover, .af-footer__menuIconLinks:hover { --footer-color: var(--white-1000); }`. | Pas de règle hover additionnelle lue. | OBSERVÉ |
| Stories | Story Apollo importe `@axa-fr/canopee-react/client`. | Story Look & Feel importe `@axa-fr/canopee-react/prospect`. | DOCUMENTÉ |

## Accessibilité

- Racine `role="contentinfo"`; nav principal `role="navigation"` avec `aria-label={expandLinkText}`. | IMPLÉMENTÉ |
- Bouton natif `type="button"` pour ouvrir/fermer les liens sur mobile. | IMPLÉMENTÉ |
- Liens sociaux : `aria-label="social media {icon}"`. | IMPLÉMENTÉ |
- Le bouton n'expose pas `aria-expanded` dans le code lu. | OBSERVÉ |
- Aucune conformité WCAG/RGAA n'est certifiée par les sources lues. | NON_CONFIRMÉ |

## Limites et points à confirmer

- `NON_CONFIRMÉ` : règles d'usage, cas d'exclusion, cardinalité, nombre maximal de liens et microcopy ; vérifier le Zeroheight Prospect ou Client.
- Vérifier dans le Storybook de la version installée : l'inversion des imports dans les stories Apollo/Look & Feel, le rendu des liens au clavier sur mobile, et les icônes sociales disponibles.
