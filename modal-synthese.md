# Modal — Synthèse

Synthèse d'implémentation du composant `Modal` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Le MDX distingue `ModalCore`, configurable librement, et `Modal`, composition standardisée. | DOCUMENTÉ |
| Quand l'utiliser | Les sources documentent l'ouverture via `showModal()` et la fermeture via `close()`, sans règle générale d'usage. | DOCUMENTÉ |
| Quand ne pas l'utiliser | Non précisé dans les sources. | NON_CONFIRMÉ |
| Cardinalité par page | Non précisée dans les sources. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Exports : `Modal`, `ModalCore`, `ModalCoreBody`, `ModalCoreFooter`, `ModalCoreHeader`.
- Types associés : `ModalCoreProps`, `ModalCoreBodyProps`, `ModalCoreFooterProps`, `ModalCoreHeaderProps` dans les fichiers source.
- Exports publics : `prospect.ts` réexporte `ModalApollo`, `client.ts` réexporte `ModalLF`.
- Élément racine rendu : `<dialog class="af-modal">`.
- Dépendances internes : `Button`, `Heading`, `ModalCore`, `ModalCoreHeader`, `ModalCoreBody`, `ModalCoreFooter`.

```tsx
const ref = useRef<HTMLDialogElement>(null);

<Modal ref={ref} title="Titre de modale" onClose={() => ref.current?.close()}>
  Contenu
</Modal>;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `title` | requis dans `ModalCoreProps` | Aucun | Sert de libellé ARIA par défaut et de titre de la composition. | IMPLÉMENTÉ |
| `onClose` | `VoidFunction` | Aucun | Appelé au clic sur le `<dialog>` et par le bouton de fermeture du header. | IMPLÉMENTÉ |
| `children` | `ReactNode` | Aucun | Contenu du body pour `Modal`, contenu libre pour `ModalCore`. | IMPLÉMENTÉ |
| `headingProps` | `Omit<HeadingProps, "children">` | Aucun | Props du composant `Heading` exposées par `Modal`. | IMPLÉMENTÉ |
| `modalCoreBodyProps` | props de body | Aucun | Transmises à `ModalCoreBody`. | IMPLÉMENTÉ |
| `modalCoreFooterProps` | props de footer | Aucun | Transmises à `ModalCoreFooter`. | IMPLÉMENTÉ |
| `modalCoreHeaderProps` | props de header | Aucun | Transmises à `ModalCoreHeader`. | IMPLÉMENTÉ |
| `closeButtonAriaLabel` | `string` | Selon header | Label du bouton de fermeture. | IMPLÉMENTÉ |
| `primaryButtonProps`/`secondaryButtonProps`/`tertiaryButtonProps` | `ButtonProps` | Aucun | Boutons rendus dans le footer si fournis. | IMPLÉMENTÉ |
| Props natives | `ComponentPropsWithRef<"dialog">` hors conflits | Selon React/HTML | `ref`, `open`, `aria-*` et autres props du dialogue. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Composition libre | `ModalCore` | `af-modal` | IMPLÉMENTÉ |
| Composition standard | `Modal` | `af-modal` + header/body/footer | IMPLÉMENTÉ |
| Sous-composants | `ModalCoreHeader`, `ModalCoreBody`, `ModalCoreFooter` | Classes préfixées `af-modal__` | IMPLÉMENTÉ |

Aucun objet de variantes visuelles propre à `Modal` n'est exposé.

## États et comportements

- L'ouverture et la fermeture reposent sur l'API native `<dialog>` : `showModal()` et `close()`.
- Les stories pilotent l'ouverture avec une `ref` vers `HTMLDialogElement`.
- Un clic sur le `<dialog>` appelle `onClose`.
- Le clic dans `.af-modal__content` appelle `stopPropagation()` et ne ferme pas par propagation.
- Le bouton de fermeture du header appelle `onClose`.
- Aucun état React interne d'ouverture n'est créé par `Modal`.

## Anatomie

```html
<dialog class="af-modal" aria-modal aria-label="Titre de modale">
  <section class="af-modal__content">
    <header class="af-modal__header">
      <button class="af-modal__header-close-btn">Fermer</button>
      <div class="af-modal__header-title">Titre de modale</div>
    </header>
    <main class="af-modal__body">Contenu</main>
    <footer class="af-modal__footer">
      <button class="af-modal__footer-button">Action</button>
    </footer>
  </section>
</dialog>
```

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Commun | `--modal-transition-duration: 0.2s`, `--modal-max-height`, `--modal-border-radius` | OBSERVÉ |
| Couleurs/padding | `--modal-bg-color`, `--modal-text-color`, `--modal-default-padding` | OBSERVÉ |
| Body | `--modal-body-padding-bottom`, `--modal-body-gap` | OBSERVÉ |
| Footer | `--modal-footer-vertical-padding`, `--modal-footer-horizontal-padding`, `--modal-footer-gap`, `--modal-footer-border-top` | OBSERVÉ |
| Responsive | `@media (--desktop-small)` | OBSERVÉ |
| Prospect | Fond `var(--white-1000)`, rayon `var(--radius-16)`, texte `var(--rem-16)`, gap footer `var(--rem-16)` | OBSERVÉ |
| Client | Fond `var(--white-1000)`, texte `var(--rem-16)`, padding défaut `var(--rem-16)`, body gap `var(--rem-32)` puis `var(--rem-40)` | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Export | `ModalApollo` | `ModalLF` | IMPLÉMENTÉ |
| Bouton | `ButtonApollo` | `ButtonLF` | IMPLÉMENTÉ |
| Titre | `HeadingApollo` | `HeadingLF` | IMPLÉMENTÉ |
| Header/Footer | Sous-composants Apollo | Sous-composants LF | IMPLÉMENTÉ |
| Espacements | Variables Apollo spécifiques | Padding et gaps LF spécifiques | OBSERVÉ |

## Accessibilité

- Élément natif `<dialog>`.
- Attribut `aria-modal` posé.
- `aria-label` reçoit la valeur fournie ou, par défaut, `title`.
- Le bouton de fermeture accepte `closeButtonAriaLabel`.
- Les props `aria-*` natives peuvent être transmises au dialogue.
- Aucun focus trap explicite, focus initial ou retour de focus n'est implémenté dans les fichiers consultés.
- Aucune gestion clavier personnalisée n'est implémentée dans `ModalCore`.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : focus initial, retour du focus, focus trap et comportement clavier détaillé au-delà du natif.
- `NON_CONFIRMÉ` : règles de contenu, dimensions recommandées, fréquence d'utilisation et cardinalité.
- `NON_CONFIRMÉ` : conformité WCAG/RGAA complète.
- Vérifier dans le Storybook de la version installée : API `ModalCore`, composition standard et rendu responsive.
