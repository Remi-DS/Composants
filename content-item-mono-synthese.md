# ContentItemMono — Synthèse

Synthèse d'implémentation du composant `ContentItemMono` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Item de contenu avec zone gauche de type image, icône ou stick, et textes titre / sous-titres. | IMPLÉMENTÉ |
| Quand l'utiliser | Les MDX contiennent seulement un exemple d'import, sans intention détaillée. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Non présent dans les sources techniques. | NON_CONFIRMÉ |
| Cardinalité par page | Non présent dans les sources techniques. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `ContentItemMono`.
- Exports associés : aucun type public dédié exporté par `prospect.ts`/`client.ts`.
- Élément racine rendu et classe CSS de base : par défaut `<div className="af-content-item-mono af-content-item-mono--medium">`.
- Dépendances internes : `ContentItemMonoCore`, `BasePicture`, `Icon`, `getClassName`, type utilitaire `AtLeastOne`.

```tsx
import { ContentItemMono } from "@axa-fr/canopee-react/prospect";

<ContentItemMono type="icon" title="Texte principale" subtitle1="Texte secondaire" />;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `type` | `"picture"` | — | Rend `BasePicture` avec `src=picture` et `alt=title`. | IMPLÉMENTÉ |
| `picture` | `string` | — | Source image pour `type="picture"`. | IMPLÉMENTÉ |
| `title` | `string` | requis selon variantes sauf icon optionnel | Rendu dans `span.af-content-item-mono__title` si fourni. | IMPLÉMENTÉ |
| `subtitle` | `string` | — | Pour `picture` et `stick`, rendu comme sous-titre secondaire. | IMPLÉMENTÉ |
| `type` | `"icon"` | — | Rend une icône si `iconProps` est fourni. | IMPLÉMENTÉ |
| `subtitle1` | `string` | — | Devient `primarySubtitle`. | IMPLÉMENTÉ |
| `subtitle2` | `string` | — | Devient `subtitle`. | IMPLÉMENTÉ |
| `iconProps` | `IconProps` | — | Transmis à l'icône avec `data-testid="icon"`. | IMPLÉMENTÉ |
| `type` | `"stick"` | — | Rend `div.af-content-item-mono__stick`. | IMPLÉMENTÉ |
| `size` | `"medium" \| "large"` | `"medium"` | Ajoute `af-content-item-mono--medium` ou `--large`. | IMPLÉMENTÉ |

Le coeur `ContentItemMonoCore` accepte aussi `as?: ElementType`, mais cette prop n'est pas exposée par `ContentItemMonoCommon` dans les types publics du composant.

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Type | `picture` | pas de classe spécifique ; présence `BasePicture` | IMPLÉMENTÉ |
| Type | `icon` | pas de classe spécifique ; présence `Icon` | IMPLÉMENTÉ |
| Type | `stick` | `af-content-item-mono__stick` | IMPLÉMENTÉ |
| Taille | `medium` | `af-content-item-mono--medium` | IMPLÉMENTÉ |
| Taille | `large` | `af-content-item-mono--large` | IMPLÉMENTÉ |

## États et comportements

`getContentItemCoreProps` mappe les trois types vers les props du coeur.
Pour `icon`, `title`, `subtitle1` et `subtitle2` sont tous optionnels côté type, mais le coeur exige au moins un champ texte via `AtLeastOne` après mapping.
Pour `picture`, l'attribut `alt` de `BasePicture` est le `title`.
Pour `stick`, le composant gauche est un div décoratif stylé par couleur.

## Anatomie

Structure coeur : racine configurable en interne > `leftComponent` éventuel > `div.af-content-item-mono__text-content` > spans titre, sous-titre primaire, sous-titre.
Les stories couvrent `Picture`, `PictureLarge`, `Icon`, `IconProps` et `Stick`.
Les MDX disent "To use the header", formulation probablement copiée ; contradiction conservée car le composant n'est pas un header dans le code.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Commun | `display: flex`, `align-items: center`, `gap: var(--rem-12)`. | OBSERVÉ |
| Stick | largeur `var(--rem-4)`, rayon `2px`, `align-self: stretch`, fond `--stick-background-color`. | OBSERVÉ |
| Text content | colonne, `gap: var(--rem-2)`. | OBSERVÉ |
| Medium base | titre `var(--rem-16)`, poids `600`, line-height `var(--rem-20)`. | OBSERVÉ |
| Large commun | titre `var(--rem-20)` puis desktop `var(--rem-24)`, sous-titre `var(--rem-16)` puis `var(--rem-18)`. | OBSERVÉ |
| Univers | `--stick-background-color: var(--blue-1000)`, titre `gray-1000`, sous-titre `gray-800`. | OBSERVÉ |
| Desktop | `@media (--desktop-small)` augmente titre, subtitle-primary et subtitle. | OBSERVÉ |

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| Icône interne | `IconApollo` | `IconLF` | IMPLÉMENTÉ |
| CSS | `ContentItemMonoApollo.css` | `ContentItemMonoLF.css` | IMPLÉMENTÉ |
| Valeurs CSS lues | mêmes variables et règles que LF | mêmes variables et règles que Apollo | OBSERVÉ |
| Story `IconProps` | `src` + `hasBackground: true` | `src`, `variant: "primary"`, `hasBackground: true`, `size: "L"` | DOCUMENTÉ |

## Accessibilité

Pour `picture`, l'image reçoit `alt={title}`.
Les icônes dépendent de `iconProps`; le composant n'ajoute pas `role="presentation"` par défaut.
Le stick est un `div` décoratif sans attribut ARIA.
La conformité WCAG/RGAA n'est pas certifiée.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : règles d'usage des types `picture`, `icon`, `stick`, cardinalité et microcopy.
- `NON_CONFIRMÉ` : accessibilité attendue des icônes décoratives versus informatives.
- Vérifier dans le Storybook de la version installée : rendu avec `type="icon"` sans titre, tailles d'icône Client et texte MDX "header".
