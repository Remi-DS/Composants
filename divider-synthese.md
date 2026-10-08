# Divider — Synthèse

Synthèse d'implémentation du composant `Divider` du design system Canopée (AXA France).
Sources lues le 8 octobre 2026 : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | Le MDX montre un séparateur entre deux contenus textuels dans un exemple `Hello` / `world!`. | DOCUMENTÉ |
| Quand l'utiliser | La règle de design précise n'est pas publiée dans les sources lues. | NON_CONFIRMÉ |
| Quand ne pas l'utiliser | Aucun cas d'exclusion n'est documenté. | NON_CONFIRMÉ |
| Cardinalité par page | Aucune cardinalité maximale n'est indiquée. | NON_CONFIRMÉ |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `Divider`.
- Exports associés : aucun type public dédié exporté depuis `prospect.ts` ou `client.ts`.
- Élément racine rendu et classe CSS de base : `<hr className="af-divider" />`.
- Dépendances internes : `getClassName`, `useMemo`.

```tsx
import { Divider } from "@axa-fr/canopee-react/prospect";

<div>
  <span>Hello</span>
  <Divider />
  <span>world!</span>
</div>;
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| `className` | `string` | `undefined` | Ajoutée à la classe de base `af-divider` via `getClassName`. | IMPLÉMENTÉ |

Pas d'héritage déclaré de props natives (`ComponentPropsWithoutRef<"hr">` absent) : seules les props typées ci-dessus sont exposées.

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|
| Aucune variante React | Non applicable | `af-divider` | IMPLÉMENTÉ |

Le composant n'expose pas de prop `variant` ni d'objet de variantes.

## États et comportements

- Aucun état interactif n'est implémenté.
- Aucun comportement JavaScript hors calcul de classe n'est présent.
- Le rendu est toujours un élément HTML `hr`.
- Les stories Prospect et Client utilisent le même scénario `Default`.

## Anatomie

- Racine unique : `<hr className={componentClassName} />`.
- Aucune sous-structure DOM n'est créée.
- Les classes additionnelles fournies par `className` sont fusionnées avec `af-divider`.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|
| Base | `margin: 0`, `border: 0`, `border-top: 1px solid var(--divider-border-color)`. | OBSERVÉ |
| Token Prospect | `--divider-border-color: var(--blue-200)`. | OBSERVÉ |
| Token Client | `--divider-border-color: var(--blue-200)`. | OBSERVÉ |
| Responsive | Aucune media query dans les CSS `DividerCommon`, `DividerApollo` ou `DividerLF`. | OBSERVÉ |

Les valeurs proviennent du CSS source et peuvent évoluer avec les tokens.

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|
| React | `DividerApollo.tsx` importe le CSS prospect puis réexporte `DividerCommon`. | `DividerLF.tsx` importe le CSS client puis réexporte `DividerCommon`. | IMPLÉMENTÉ |
| Couleur | `var(--blue-200)`. | `var(--blue-200)`. | OBSERVÉ |
| Stories | Import depuis `@axa-fr/canopee-react/prospect`. | Import depuis `@axa-fr/canopee-react/client`. | DOCUMENTÉ |

Les deux univers partagent le même composant commun ; l'écart constaté est le point d'entrée CSS importé.

## Accessibilité

- L'élément natif `hr` conserve sa sémantique de séparation thématique.
- Aucun attribut ARIA n'est ajouté ou modifié.
- Aucun mécanisme de focus ou clavier n'est implémenté.
- La conformité WCAG/RGAA n'est pas certifiée par les sources lues.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : règles d'usage, cas d'exclusion et cardinalité par page dans le Zeroheight de l'univers concerné.
- `NON_CONFIRMÉ` : règles d'espacement autour du séparateur ; le composant ne porte que sa ligne.
- Vérifier dans le Storybook de la version installée : rendu de la couleur `var(--blue-200)` selon le thème chargé.
