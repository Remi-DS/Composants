# CardCheckboxOption — Synthèse

Synthèse d'implémentation du composant `CardCheckboxOption` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Option individuelle de checkbox présentée sous forme de carte cliquable. | IMPLÉMENTÉ / DOCUMENTÉ |
| Composition | Peut afficher un label, une description, un sous-titre, une icône et une checkbox. | IMPLÉMENTÉ |
| Quand l'utiliser | Non publié au-delà des exemples ; vérifier le Zeroheight de l'univers. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Non publié dans les sources lues. | NON_CONFIRMÉ |
| Cardinalité par groupe | Non publiée dans les sources lues. | NON_CONFIRMÉ |

## Intégration React

- Package Prospect : `@axa-fr/canopee-react/prospect`, export `CardCheckboxOption`.
- Package Client : `@axa-fr/canopee-react/client`, export `CardCheckboxOption`.
- Fichiers : `CardCheckboxOptionCommon.tsx`, `CardCheckboxOptionApollo.tsx`, `CardCheckboxOptionLF.tsx`.
- Élément racine : `<label class="af-card-checkbox-option">`.
- Dépendances internes : `Checkbox`, `Icon`.

```tsx
import { CardCheckboxOption } from "@axa-fr/canopee-react/prospect";

<CardCheckboxOption
  label="Titre"
  description="Sous-titre 1"
  subtitle="Sous-titre 2"
  name="foo"
  value="bar"
/>;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `label` | `ReactNode` | — | Rendu dans `af-card-checkbox-option__label`. | IMPLÉMENTÉ |
| `type` | `"vertical" \| "horizontal"` | `"vertical"` | Ajoute `af-card-checkbox-option--horizontal` seulement si horizontal. | IMPLÉMENTÉ |
| `description` | `ReactNode` | `undefined` | Rend `af-card-checkbox-option__description` si truthy. | IMPLÉMENTÉ |
| `subtitle` | `ReactNode` | `undefined` | Rend `af-card-checkbox-option__subtitle` si truthy. | IMPLÉMENTÉ |
| `icon` | `IconProps["src"]` | `undefined` | Rend une icône décorative `role="presentation"` si fourni. | IMPLÉMENTÉ |
| `className` | `string` | `undefined` | Ajoutée à la classe du `<label>`. | IMPLÉMENTÉ |
| Props héritées | `CheckboxProps` | — | Inclut props d'`input` sauf `disabled` et `type`, plus `errorId?` déprécié et `variant?`. | IMPLÉMENTÉ |

`CheckboxProps` force `type="checkbox"` dans le composant `Checkbox`; `disabled` est omis du type lu.

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Verticale | `type` absent ou `"vertical"` | `af-card-checkbox-option` | IMPLÉMENTÉ |
| Horizontale | `type="horizontal"` | `af-card-checkbox-option af-card-checkbox-option--horizontal` | IMPLÉMENTÉ |
| État coché | `input:checked` | Sélecteur CSS `:has(input:checked)` | OBSERVÉ |
| État invalide | `input[aria-invalid="true"]` | Sélecteur CSS `:has(input[aria-invalid="true"])` | OBSERVÉ |

## États et comportements

- Le `<label>` enveloppe toute l'option ; cliquer sur la carte active la checkbox interne.
- La checkbox est rendue après le contenu dans le DOM, mais en horizontal le CSS lui applique `order: -1`.
- Le label passe en graisse `600` quand l'input est coché.
- L'état invalide non coché augmente l'épaisseur de bordure à `2px`, puis `3px` au hover/focus.
- Les stories Apollo proposent les icônes `homeIcon`, `accountBalanceIcon` ou `none`.

## Anatomie

- Racine : `<label class="af-card-checkbox-option [af-card-checkbox-option--horizontal]">`.
- Icône optionnelle : composant `Icon`.
- Contenu : `<div class="af-card-checkbox-option__content">`.
- Textes : `af-card-checkbox-option__label`, `__description`, `__subtitle`.
- Contrôle : composant `Checkbox` avec les props restantes.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Commun | `position: relative`, `display: flex`, `padding: 1rem`, `cursor: pointer`. | OBSERVÉ |
| Commun | `outline` piloté par `--checkbox-option-border-width` et `--checkbox-option-border-color`. | OBSERVÉ |
| Icône | `--icon-size: var(--icon-size-medium)`, puis `large` à `@media (--desktop-small)`. | OBSERVÉ |
| Label | Taille `var(--rem-16)`, puis `var(--rem-18)` desktop. | OBSERVÉ |
| Description/sous-titre | Taille `var(--rem-14)`, puis `1rem` desktop. | OBSERVÉ |
| Horizontal | `flex-direction: row`, contenu aligné à gauche, checkbox en premier. | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Bordure initiale | `var(--blue-650)`. | `var(--gray-800)`. | OBSERVÉ |
| Titre | `var(--blue-1000)`. | `var(--gray-1000)`. | OBSERVÉ |
| Radius | `var(--radius-8)`. | `var(--radius-4)`. | OBSERVÉ |
| Erreur | Bordure `var(--red-alert-1000)`, fond `var(--red-040)`, titre `var(--gray-800)`. | Bordure `var(--red-alert-1200)`, pas de fond d'erreur lu. | OBSERVÉ |
| Checkbox verticale | Marge haute `var(--rem-8)` côté Apollo. | Position absolue top/left `var(--rem-12)`, puis `var(--rem-16)` desktop. | OBSERVÉ |

## Accessibilité

- La racine `<label>` associe implicitement le texte et la checkbox.
- L'icône est décorative avec `role="presentation"`.
- Les états `aria-invalid` et `aria-errormessage` peuvent être transmis via les props héritées ou par `CardCheckbox`.
- Aucun contrôle de conformité WCAG/RGAA n'est documenté.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : critères de choix entre option verticale et horizontale.
- `NON_CONFIRMÉ` : obligation ou non d'icône, description et sous-titre.
- `NON_CONFIRMÉ` : absence de stories Look & Feel listées pour ce composant dans l'index fourni.
- Vérifier dans le Storybook de la version installée : rendu Client de l'état erreur sans fond rouge.
