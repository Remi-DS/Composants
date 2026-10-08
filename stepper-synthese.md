# Stepper — Synthèse

Synthèse d'implémentation du composant `Stepper` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Afficher un titre d'étape, une progression via `ProgressBarGroup`, un helper et un message. | IMPLÉMENTÉ |
| Quand l'utiliser | Le MDX présente un stepper qui rend son titre via `Heading`; aucune règle métier de parcours n'est fournie. | DOCUMENTÉ / NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Aucune restriction explicitée dans les MDX lus. | NON_CONFIRMÉ |
| Cardinalité par page | Aucune limite publiée. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `Stepper`.
- Exports associés : aucun type `StepperProps` dans les exports publics `prospect.ts` et `client.ts`.
- Élément racine rendu et classe CSS de base : `<div className="af-stepper">`.
- Dépendances internes : `ProgressBarGroup`, `Heading`, `ItemMessage`.

```tsx
import { Stepper } from "@axa-fr/canopee-react/prospect";

<Stepper currentTitle="Titre étape" currentStep={2} nbSteps={8} currentStepProgress={50} />;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `currentStep` | `number` | requis par type | Transmis à `ProgressBarGroupComponent`. | IMPLÉMENTÉ |
| `nbSteps` | `2 \| 3 \| 4 \| 5 \| 6 \| 7 \| 8` | `undefined` | Transmis comme `stepsCount`. | IMPLÉMENTÉ |
| `currentStepProgress` | `number` | `undefined` | Transmis à `ProgressBarGroupComponent`. | IMPLÉMENTÉ |
| `currentTitle` | `ReactNode` | `undefined` | Enfant du `HeadingComponent`. | IMPLÉMENTÉ |
| `currentSubtitle` | `ReactNode` | `undefined` | Transmis à `HeadingComponent` comme `firstSubtitle`. | IMPLÉMENTÉ |
| `helper` | `string` | `undefined` | Si truthy, rend `span.af-stepper__helper`. | IMPLÉMENTÉ |
| `icon` | `string` | `undefined` | Transmis au `HeadingComponent`. | IMPLÉMENTÉ |
| `iconProps` | `HeadingCommonProps["iconProps"]` | `undefined` | Transmis au `HeadingComponent`. | IMPLÉMENTÉ |
| `message` | `string` | `undefined` | Transmis à `ItemMessageComponent`. | IMPLÉMENTÉ |
| `messageType` | `ItemMessageProps["messageType"]` | `"success"` | Transmis à `ItemMessageComponent`. | IMPLÉMENTÉ |
| `titleLevel` | `HeadingLevel` | `2` | Contrôle le niveau du heading via `Heading`. | IMPLÉMENTÉ / DOCUMENTÉ |
| Props natives | `Omit<HTMLAttributes<HTMLDivElement>, "role" \| "title">` | selon HTML | Transmises au `div`, mais `tabIndex` est forcé à `undefined`. | IMPLÉMENTÉ |

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Message | `messageType`, options story `error`, `success` | classes internes `ItemMessage` | DOCUMENTÉ |
| Niveau titre | `titleLevel`, options story `1`, `2`, `3`, `4` | dépend de `Heading` | DOCUMENTÉ |

Le composant n'expose pas de variante visuelle dédiée.

## États et comportements

- `useId()` produit l'identifiant du header, utilisé par `aria-labelledby` sur `ProgressBarGroupComponent`.
- `ItemMessageComponent` est rendu même si `message` est absent ; son comportement dépend de `ItemMessage`.
- Le `className` reçu n'est pas appliqué au wrapper `af-stepper`, mais transmis à `ProgressBarGroupComponent`.
- `role` et `title` sont exclus du type de props natives.

## Anatomie

- `div.af-stepper`.
- `div.af-stepper__header#id` contenant `HeadingComponent`.
- `ProgressBarGroupComponent` avec `currentStep`, `stepsCount`, `currentStepProgress`.
- `span.af-stepper__helper` conditionnel.
- `ItemMessageComponent`.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Commun | `display:flex`, `width:100%`, `flex-direction:column`, `gap:var(--stepper-gap)`. | OBSERVÉ |
| Commun helper | `font-weight:400`, `line-height:125%`, `color:var(--helper-color)`. | OBSERVÉ |
| Prospect | `--stepper-gap: var(--rem-8)` puis `var(--rem-16)` en `@media (--desktop-small)`. | OBSERVÉ |
| Prospect header | `column-reverse`, gap `0`, puis `var(--rem-4)` desktop. | OBSERVÉ |
| Client | gap `var(--rem-12)` puis `var(--rem-16)` desktop ; header `column`. | OBSERVÉ |
| Client helper | `var(--font-family-sans-serif)`, `var(--rem-14)` puis `var(--rem-16)` desktop. | OBSERVÉ |

Les valeurs proviennent du CSS source et peuvent évoluer.

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Sous-composants | `HeadingApollo`, `ProgressBarGroupApollo`. | `HeadingLF`, `ProgressBarGroupLF`. | IMPLÉMENTÉ |
| Ordre header | `column-reverse`. | `column`. | OBSERVÉ |
| Props story | `nbSteps: 8` fourni dans la story Apollo. | `nbSteps` absent de la story Client. | DOCUMENTÉ |

## Accessibilité

- `aria-labelledby` relie le groupe de progression au header généré.
- Le wrapper reçoit `tabIndex={undefined}` malgré les props restantes.
- Aucun handler clavier propre à `Stepper` n'est implémenté.
- La conformité WCAG/RGAA n'est pas certifiée.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : règles de nombre d'étapes, libellés, sous-titres et helper.
- `NON_CONFIRMÉ` : usage du message de succès/erreur dans un parcours.
- Vérifier dans le Storybook de la version installée : rendu exact de `Heading`, `ProgressBarGroup` et `ItemMessage`.
