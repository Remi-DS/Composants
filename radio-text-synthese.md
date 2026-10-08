# RadioText — Synthèse

Synthèse d'implémentation du composant `RadioText` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Compose un radio natif stylé et un libellé texte dans une balise `<label>`. | IMPLÉMENTÉ |
| Quand l'utiliser | Les MDX montrent un radio avec un long libellé de consentement, sans règle d'usage design. | DOCUMENTÉ |
| Quand ne pas l'utiliser | Non publié ; consulter le Zeroheight Prospect ou Client. | NON_CONFIRMÉ |
| Cardinalité par page | Non publiée dans les sources lues. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `RadioText`.
- Export associé : `RadioTextProps`.
- Élément racine rendu : `<label className="af-radio-text">`.
- Dépendances internes : `RadioApollo` ou `RadioLF`, `RadioTextCommon`, `forwardRef`.

```tsx
import { RadioText } from "@axa-fr/canopee-react/prospect";

<RadioText
  name="option1"
  value="option1"
  label="J'accepte de fournir à AXA mes coordonnées ainsi que les données relatives à mon projet et ma situation."
/>;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `label` | `string \| ReactNode` | requis | Rendu dans `span.af-radio-text__label-content`. | IMPLÉMENTÉ |
| `labelProps` | `GridContainerProps<"label">` | `undefined` | Propagé sur le `<label>` racine après `className="af-radio-text"`. | IMPLÉMENTÉ |
| Props `Radio` | `RadioProps` | selon `Radio` | `name`, `value`, `checked`, `variant`, événements et autres props input sont transmises au radio. | IMPLÉMENTÉ |
| `ref` | `HTMLInputElement` | `undefined` | Transmis au composant `Radio` via `forwardRef`. | IMPLÉMENTÉ |

`RadioTextProps` est défini comme `{ label; labelProps? } & RadioProps`.

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Variante radio | `variant="error"` | `af-radio--error` sur l'input interne | IMPLÉMENTÉ |
| Variante radio | `variant="warning"` | `af-radio--warning` sur l'input interne | IMPLÉMENTÉ |
| Variante propre `RadioText` | Aucune prop propre | `af-radio-text` | IMPLÉMENTÉ |

## États et comportements

- `RadioText` n'a pas d'état interne ; la sélection dépend du radio natif.
- Cliquer sur le label active l'input parce que le radio est imbriqué dans `<label>`.
- Les variantes d'erreur et d'avertissement sont portées par `Radio`, pas par le conteneur texte.
- Les stories exposent `label`, `name`, `value`, `checked` et `variant`.
- Une capacité technique à passer un `ReactNode` comme label ne définit pas les règles de microcopy du label.

## Anatomie

- `label.af-radio-text`.
- `RadioComponent` interne, donc `input.af-radio`.
- `span.af-radio-text__label-content`.
- `labelProps` peut ajouter des attributs au label racine selon `GridContainerProps`.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Conteneur | `display: flex`, `align-items: start`, `gap: var(--rem-8)`, `cursor: pointer` | OBSERVÉ |
| Libellé | `font-size: var(--rem-16)`, `font-weight: 400`, `line-height: 1.25`, `color: var(--gray-1000)` | OBSERVÉ |
| Desktop | `@media (--desktop-small)` : `font-size: var(--rem-18)` pour le libellé | OBSERVÉ |
| Radio interne | Styles de `RadioApollo` ou `RadioLF` selon univers. | OBSERVÉ |

Les valeurs proviennent du CSS source `RadioTextAll.css` et peuvent évoluer.

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Import CSS | `@axa-fr/canopee-css/prospect/Form/Radio/RadioText/RadioTextAll.css` | `@axa-fr/canopee-css/client/Form/Radio/RadioText/RadioTextAll.css` | IMPLÉMENTÉ |
| Radio utilisé | `RadioApollo` | `RadioLF` | IMPLÉMENTÉ |
| CSS RadioText | Même fichier `RadioTextAll.css`. | Même fichier `RadioTextAll.css`. | OBSERVÉ |
| MDX et stories | Même contenu, import Prospect. | Même contenu, import Client. | DOCUMENTÉ |

## Accessibilité

- Le radio est associé à son libellé par imbrication dans `<label>`.
- La navigation clavier et la sélection reposent sur l'input radio natif.
- Aucun `fieldset`, `legend`, message d'aide ou message d'erreur n'est rendu par `RadioText`.
- La conformité WCAG/RGAA et les règles de groupement radio sont à vérifier côté intégration.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : longueur recommandée du libellé, formulations de consentement, nombre d'options et cas de non-usage.
- `NON_CONFIRMÉ` : règles design pour variantes `error` et `warning` dans un groupe radio.
- Vérifier dans le Storybook de la version installée l'alignement sur labels longs et le comportement responsive.
