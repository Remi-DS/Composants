# Fieldset — Synthèse

Synthèse d'implémentation du composant `Fieldset` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Les stories montrent un groupement de contrôles de formulaire, avec radio buttons ou checkboxes. | DOCUMENTÉ |
| Quand l'utiliser | La règle de design exacte n'est pas publiée dans les sources lues. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Aucun cas d'exclusion n'est documenté. | NON_CONFIRMÉ |
| Cardinalité par page | Aucune limite par page n'est publiée. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `Fieldset`, type `FieldsetProps`.
- Exports associés : aucun objet de variantes.
- Élément racine rendu et classe CSS de base : `CardComponent` rendu avec `as="fieldset"` et classe `af-fieldset`.
- Dépendances internes : `Card`, `Icon`, `ContentItemMonoCore`, `getClassName`.

```tsx
import { Fieldset, Radio } from "@axa-fr/canopee-react/prospect";

<Fieldset title="Genre">
  <Radio id="gender-male" name="gender" value="male" />
</Fieldset>;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `children` | `PropsWithChildren` | `undefined` | Insérés dans `div.af-fieldset__content`. | IMPLÉMENTÉ |
| `title` | `string` | requis | Transmis à `ContentItemMonoCore` rendu en `legend`. | IMPLÉMENTÉ |
| `iconProps` | `IconProps` | `undefined` | Si fourni, rend une icône à gauche avec `aria-hidden="true"`. | IMPLÉMENTÉ |
| `className` | `string` | `undefined` | Ajoutée à `af-fieldset` via `getClassName`. | IMPLÉMENTÉ |

Pas d'héritage explicite de props natives `fieldset` ; la racine est pilotée par le composant `Card`.

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Aucune variante React | Non applicable | `af-fieldset` | IMPLÉMENTÉ |

Le composant n'expose pas de prop `variant`.

## États et comportements

- Aucun état `disabled`, `error`, `required` ou `invalid` n'est implémenté par `Fieldset`.
- `iconProps` conditionne uniquement la présence de l'icône.
- Les children restent libres ; les stories montrent des `Radio` et `CheckboxText`, mais cette possibilité technique n'est pas une règle de design.
- Les stories configurent les argTypes `title`, `iconProps` et `className` avec descriptions.

## Anatomie

- Racine : `CardComponent` avec `as="fieldset"` et `className="af-fieldset"`.
- Légende : `ContentItemMonoCore as="legend"` avec `title`.
- Icône : `IconComponent aria-hidden="true"` passé en `leftComponent` si `iconProps` existe.
- Contenu : `<div className="af-fieldset__content">{children}</div>`.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Racine | `display: flex`, `margin: 0`, `padding: var(--rem-16)`, `border: none`, `flex-direction: column`, `gap: var(--rem-24)`. | OBSERVÉ |
| Légende | `> legend.af-content-item-mono { padding-top: var(--rem-16); }`. | OBSERVÉ |
| Contenu | `.af-fieldset__content` en flex column avec `gap: var(--rem-16)`. | OBSERVÉ |
| CSS Apollo | `FieldsetApollo.css` importe seulement `FieldsetCommon.css`. | OBSERVÉ |
| CSS LF | `FieldsetLF.css` importe seulement `FieldsetCommon.css`. | OBSERVÉ |
| Responsive | Aucune media query spécifique dans les CSS lus. | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Sous-composants | Utilise `IconApollo` et `CardApollo`. | Utilise `IconLF` et `CardLF`. | IMPLÉMENTÉ |
| CSS | Même CSS commun importé via `@axa-fr/canopee-css/prospect/Fieldset/FieldsetApollo.css`. | Même CSS commun importé via `@axa-fr/canopee-css/client/Fieldset/FieldsetLF.css`. | IMPLÉMENTÉ |
| Stories | Story `With radio buttons` et `With checkboxes`. | Story `With radios` et `With checkboxes`. | DOCUMENTÉ |

Les deux univers partagent le même composant commun et les mêmes règles CSS locales ; les différences viennent surtout des sous-composants `Card` et `Icon`.

## Accessibilité

- La racine est rendue en `fieldset` et le titre en `legend`, ce qui fournit une association native de groupe.
- L'icône optionnelle est masquée aux technologies d'assistance par `aria-hidden="true"`.
- Les labels des contrôles enfants ne sont pas gérés par `Fieldset`; ils doivent être fournis par les composants enfants.
- La conformité WCAG/RGAA n'est pas certifiée par les sources lues.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : conditions d'usage, nombre de contrôles recommandé, ordre et microcopy de la légende.
- `NON_CONFIRMÉ` : gestion visuelle d'erreur au niveau groupe ; aucune prop dédiée n'est exposée.
- Vérifier dans le Storybook de la version installée : rendu de `Card` selon l'univers et contenu enfant réel.
