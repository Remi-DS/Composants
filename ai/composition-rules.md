# Règles de composition — Canopée

## Objectif

Définir comment un agent passe de composants isolés à une interface, sans transformer une hypothèse de composition en règle officielle Canopée.

## Hiérarchie des décisions

1. Règle explicitement documentée dans Zeroheight.
2. Comportement implémenté et confirmé dans la fiche du composant.
3. Relation explicitement enregistrée dans `ai/component-relations.json`.
4. Pattern enregistré dans `ai/pattern-registry.json`.
5. Recommandation de prototype clairement marquée `RECOMMANDATION`.
6. Si aucune source ne permet de décider : `NON_CONFIRMÉ`.

## Choix d'un composant

Avant de sélectionner un composant, l'agent identifie l'intention utilisateur, le type d'information ou d'action, l'interaction attendue, l'univers Prospect ou Client, le device et l'état nécessaire.

L'agent privilégie le composant dont l'intention documentée correspond au besoin. Une capacité API ne constitue pas une permission de design.

## Composition

- Réutiliser les composants existants avant de proposer une nouvelle combinaison.
- Respecter les relations enregistrées.
- Ne pas déduire qu'une relation technique implique une relation design.
- Ne pas inventer une variante pour résoudre un besoin de composition.
- Ne pas transformer un template en règle globale s'il est marqué `RECOMMANDATION`.

## Responsive

Utiliser `ai/responsive-grid.md` comme règle globale. Les règles responsive propres à un composant priment lorsqu'elles sont explicitement documentées dans sa fiche. Ne pas inventer de breakpoint intermédiaire.

## États

Pour chaque composant interactif, identifier les états nécessaires au prototype uniquement lorsqu'ils sont réellement disponibles ou documentés.

## Sortie attendue

Toute génération de page doit pouvoir fournir : composants utilisés, variantes, univers, états, règles responsive appliquées, sources consultées, hypothèses, éléments `NON_CONFIRMÉ` et questions nécessitant validation humaine.

## Contradictions

Quand deux sources se contredisent, conserver la contradiction et ne pas arbitrer silencieusement.