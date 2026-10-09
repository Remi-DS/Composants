# Règles de composition pour un prototype à partir d'une maquette

## Objectif

Aider l'agent à reproduire une interface existante en s'appuyant sur Canopée. Le besoin n'est pas de générer une architecture de page ou un parcours à partir de zéro.

## Hiérarchie des sources

1. Maquette accessible : composition, contenu visible, ordre, proportions et hiérarchie visuelle.
2. Brief utilisateur : périmètre, écrans, interactions et résultats attendus.
3. Zeroheight : règles d'usage et intentions documentées.
4. Fiches de synthèse et Storybook/code : API, rendu et comportements effectivement implémentés.
5. Registres et patterns : index et aides à la décision ; un élément marqué RECOMMANDATION n'est pas une règle officielle.
6. Informations absentes ou contradictoires : NON_CONFIRMÉ, à clarifier ou à documenter comme hypothèse.

## Reproduire avant d'interpréter

- Suivre la composition de la maquette, même si une autre architecture semble plus conventionnelle.
- Ne pas ajouter, retirer ou réordonner une zone, une étape ou une action sans demande.
- Ne pas remplacer le contenu de référence par de la microcopy inventée.
- Ne pas imposer un template du dossier ai/templates/ : ces exemples sont facultatifs et ne doivent jamais dicter la structure du prototype.
- Si la maquette n'est pas accessible, demander une capture/export ou signaler précisément ce qui empêche une reproduction fiable.

## Choix et usage des composants

- Consulter ai/component-registry.json, puis lire la fiche de synthèse de chaque composant candidat.
- Vérifier que le composant est disponible dans l'univers et l'environnement techniques du prototype.
- Réutiliser un composant Canopée adapté ; si aucun n'est disponible, un élément spécifique est possible avec justification.
- Une relation technique ne constitue pas automatiquement une règle de design.
- Ne pas inventer une variante, un token ou une règle d'usage.
- Si la maquette diffère de la documentation Canopée, conserver et signaler la différence au lieu de la normaliser silencieusement.

## Interactions

- Implémenter les interactions mentionnées dans le brief ou clairement visibles/établies par la référence.
- Une capture statique ne suffit pas à définir la validation d'un formulaire, une erreur métier, la destination d'un lien ou la logique de retour.
- Si un comportement manquant a un impact sur le test, demander une clarification. Sinon, garder le prototype minimal et documenter toute hypothèse.
- Ne pas connecter de service réel ou de données réelles sans demande explicite.

## Responsive

Reproduire les formats fournis. Utiliser ai/responsive-grid.md et les règles propres aux composants seulement lorsque ces sources s'appliquent au projet. Si une version mobile n'est pas fournie, ne pas présenter une adaptation imaginée comme une règle officielle ; la signaler comme proposition à valider.

## Sortie attendue

Pour chaque prototype, fournir les écrans couverts, les composants et sources consultés, les interactions testées, les écarts visuels/fonctionnels, les hypothèses provisoires et les questions restantes.

## Contradictions

Quand la maquette, le brief et la documentation se contredisent, ne pas arbitrer silencieusement. Décrire le conflit et demander une décision si elle modifie sensiblement le rendu ou le comportement.
