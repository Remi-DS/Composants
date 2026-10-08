# LevelSelector — Synthèse

Synthèse d'implémentation du composant `LevelSelector` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle technique | Sélecteur de niveau avec boutons moins/plus et choix radio parmi des étapes. | IMPLÉMENTÉ |
| Intention design | Les sources montrent l'API et des exemples, sans règle de contexte d'usage. | NON_CONFIRMÉ |
| Quand l'utiliser | Non présent dans les sources techniques ; à confirmer dans le Zeroheight. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Non présent dans les sources techniques. | NON_CONFIRMÉ |
| Cardinalité par page | Non présent ; `stepsCount` limite les étapes à 1, 2 ou 3 mais pas le nombre de composants par page. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `LevelSelector`.
- Exports associés : `LevelSelectorProps`.
- Exports publics : `prospect.ts` réexporte `LevelSelectorApollo`, `client.ts` réexporte `LevelSelectorLF`.
- Élément racine rendu : `Card` injecté avec `as="fieldset"` et `role="radiogroup"`.
- Dépendances internes : `CardApollo`/`CardLF`, `ClickIconApollo`/`ClickIconLF`, `useId`, `getClassName`.

```tsx
const [value, setValue] = useState(1);

<LevelSelector
  title="Niveau de garantie"
  description="Description"
  value={value}
  onChange={setValue}
/>;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `title` | `string` | Aucun | Rendu dans un `<legend>` si la chaîne n'est pas vide. | IMPLÉMENTÉ |
| `description` | `string` | Aucun | Rendu dans `af-level-selector__description` avec `aria-live="polite"`. | IMPLÉMENTÉ |
| `value` | `number` | `0` | Niveau courant ; contrôle radios, segments actifs et boutons aux bornes. | IMPLÉMENTÉ |
| `stepsCount` | `1 \| 2 \| 3` | `2` | Nombre d'étapes générées. | IMPLÉMENTÉ |
| `minusAriaLabel` | `string` | `"Diminuer le niveau"` | Label accessible du bouton moins. | IMPLÉMENTÉ |
| `plusAriaLabel` | `string` | `"Augmenter le niveau"` | Label accessible du bouton plus. | IMPLÉMENTÉ |
| `onChange` | `(value: number) => void` | Aucun | Appelé avec la nouvelle valeur. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Nombre d'étapes | `1`, `2`, `3` | Nombre de labels radio rendus | IMPLÉMENTÉ |
| Segment actif | `step <= value` | `af-level-selector__active` | IMPLÉMENTÉ |

Aucun objet de variantes public n'est exporté.

## États et comportements

- Le composant est contrôlé : aucune valeur interne n'est stockée.
- Le bouton moins est désactivé quand `value === 0`.
- Le bouton plus est désactivé quand `value === stepsCount`.
- Chaque radio est cochée lorsque `step === value`.
- Cliquer un segment radio appelle `onChange(step)`.
- Cliquer moins/plus appelle `onChange(value - 1)` ou `onChange(value + 1)`.

## Anatomie

```html
<fieldset class="af-level-selector" role="radiogroup">
  <legend class="af-level-selector__title">Niveau de garantie</legend>
  <div class="af-level-selector__content">
    <button aria-label="Diminuer le niveau">bouton moins</button>
    <div class="af-level-selector__group">
      <label class="af-level-selector__radio af-level-selector__active">
        <input type="radio" name="id-generé" aria-describedby="description-id" />
      </label>
    </div>
    <button aria-label="Augmenter le niveau">bouton plus</button>
  </div>
  <span class="af-level-selector__description" aria-live="polite">Description</span>
</fieldset>
```

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Racine | Flex vertical, centrage, `gap: var(--rem-16)` | OBSERVÉ |
| Groupe | Flex, `gap: var(--rem-8)` | OBSERVÉ |
| Radio | Hauteur `var(--rem-24)`, bordure, rayon `var(--rem-16)`, curseur pointer | OBSERVÉ |
| Actif | `background-color: var(--level-selector-color)` | OBSERVÉ |
| Hover/focus | Contour 1 px via `var(--level-selector-outline-color)` et `:has(input:focus-visible)` | OBSERVÉ |
| Responsive | `@media (--desktop-small)` : gaps réduits, hauteur radio `var(--rem-16)` | OBSERVÉ |
| Prospect | Titre bleu, description 14 px | OBSERVÉ |
| Client | Titre gris, description 16 px | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Composant commun | `LevelSelectorCommon` | `LevelSelectorCommon` | IMPLÉMENTÉ |
| Carte injectée | `CardApollo` | `CardLF` | IMPLÉMENTÉ |
| Icônes d'action | `ClickIconApollo` | `ClickIconLF` | IMPLÉMENTÉ |
| Titre | `var(--blue-1000)` | `var(--gray-1000)` | OBSERVÉ |
| Description | `var(--rem-14)` | `var(--rem-16)` | OBSERVÉ |

## Accessibilité

- Racine en `fieldset` avec `role="radiogroup"`.
- Titre en `legend`.
- Contrôles internes en `input type="radio"`.
- Nom de groupe généré par `useId`.
- `aria-describedby` relie les radios à la description.
- `aria-live="polite"` est présent sur la description.
- Les boutons moins/plus ont des libellés ARIA configurables et sont désactivés aux bornes.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : règles d'usage design, choix des libellés, nombre de LevelSelector par page.
- `NON_CONFIRMÉ` : comportement clavier global au-delà des radios et boutons natifs.
- `NON_CONFIRMÉ` : conformité WCAG/RGAA de l'ensemble.
- Vérifier dans le Storybook de la version installée : rendu des `ClickIcon` et des cartes dans chaque univers.
