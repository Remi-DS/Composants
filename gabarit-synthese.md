# Gabarit de synthèse par composant — Design System Canopée

Fichier de travail interne. Chaque fichier produit s'appelle `<composant-kebab>-synthese.md`
et se place à la racine du dépôt `Composants`.

## Règles de rédaction impératives

1. **Aucune règle de design inventée.** Toute affirmation doit provenir d'une source réellement
   lue dans le dépôt source `AxaFrance/design-system` (code React, CSS, stories, MDX) ou du
   Storybook publié.
2. **Qualifier chaque information** avec l'un des statuts suivants, comme dans
   `button-primary.md` :
   - `IMPLÉMENTÉ` — comportement constaté dans le code React ou CSS publié ;
   - `DOCUMENTÉ` — règle écrite explicitement dans un MDX ou une story ;
   - `OBSERVÉ` — valeur relevée dans le CSS ou les tokens ;
   - `RECOMMANDATION` — consigne d'intégration proposée par la synthèse, jamais présentée
     comme une règle Canopée ;
   - `NON_CONFIRMÉ` — information absente des sources accessibles.
3. **Les règles d'usage design (quand utiliser / ne pas utiliser, cardinalité, microcopy)**
   ne sont pas publiées dans le dépôt source. Sauf mention explicite trouvée dans un MDX,
   elles doivent être marquées `NON_CONFIRMÉ` avec indication de la source à consulter
   (Zeroheight de l'univers concerné).
4. **Une capacité technique n'est pas une permission de design.** Le rappeler dans la section
   correspondante.
5. **Conserver les contradictions** entre univers Prospect et Client au lieu d'en choisir une.
6. Langue : français. Ton factuel. Pas de superlatif.

## Structure imposée du fichier

```markdown
# <Nom du composant> — Synthèse

Synthèse d'implémentation du composant `<Nom>` du design system Canopée (AXA France).
Sources lues le <date> : code React, CSS et stories du dépôt `AxaFrance/design-system`.

> **Portée :** cette synthèse décrit l'implémentation publiée. Les règles d'usage design
> (quand utiliser le composant, cardinalité, microcopy) ne figurent pas dans les sources
> techniques ; elles sont marquées `NON_CONFIRMÉ` et relèvent du Zeroheight de l'univers
> concerné. Une capacité offerte par l'API n'est pas une autorisation de design.

## Rôle et intention

| Sujet | Information | Statut |
|---|---|---|
| Rôle | … | DOCUMENTÉ / NON_CONFIRMÉ |
| Quand l'utiliser | … | NON_CONFIRMÉ si absent des sources |
| Quand ne pas l'utiliser | … | NON_CONFIRMÉ si absent des sources |
| Cardinalité par page | … | NON_CONFIRMÉ si absent des sources |

## Intégration React

- Package : `@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`.
- Export : `…`.
- Exports associés : `…` (objets de variantes, types).
- Élément racine rendu et classe CSS de base : `…`.
- Dépendances internes (composants utilisés) : `…`.

```tsx
// exemple d'import et d'usage repris des stories
```

## Propriétés

| Propriété | Type | Défaut | Comportement | Statut |
|---|---|---|---|---|
| … | … | … | … | IMPLÉMENTÉ |

Préciser l'héritage des props natives (`ComponentPropsWithoutRef<"…">`) le cas échéant.

## Variantes

| Variante | Valeur | Classe CSS | Statut |
|---|---|---|---|

Si le composant n'expose pas de variante, l'indiquer explicitement.

## États et comportements

Décrire les états réellement implémentés (disabled, loading, open/close, checked, error…)
et la mécanique correspondante dans le code.

## Anatomie

Décrire la structure DOM produite et les sous-éléments avec leurs classes.

## Styles, tokens et responsive

| Contexte | Valeur relevée | Statut |
|---|---|---|

Mentionner les variables CSS (`--…`), les points de rupture (`@media (--desktop-small)`, etc.)
et préciser que les valeurs proviennent du CSS source et peuvent évoluer.

## Différences Prospect / Client

| Aspect | Prospect (Apollo) | Client (Look & Feel) | Statut |
|---|---|---|---|

Si les deux univers partagent le même composant commun, le dire explicitement et ne lister
que les écarts de styles ou de sous-composants.

## Accessibilité

Lister uniquement ce qui est réellement implémenté (attributs ARIA, rôles, gestion du focus,
clavier) puis les points non vérifiés. Ne pas certifier la conformité WCAG/RGAA.

## Limites et points à confirmer

- `NON_CONFIRMÉ` : …
- Vérifier dans le Storybook de la version installée : …
```
