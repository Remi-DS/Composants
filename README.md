# Canopée — AI-ready Design System

Ce dépôt constitue la couche de connaissance exploitable par des agents IA pour concevoir des interfaces conformes au Design System Canopée.

## Rôle des sources

- **Storybook** : source de vérité pour l'implémentation et le rendu lorsque cette information y est disponible.
- **Zeroheight** : documentation humaine des règles de conception et d'usage.
- **Figma / exports de tokens** : source visuelle et données comparatives, lorsqu'elles sont explicitement identifiées.
- **Ce dépôt** : règles de raisonnement IA, registre des composants, provenance, contraintes et validations.

Une propriété technique disponible dans une API n'est pas automatiquement une permission de design.

## Règle fondamentale

> Une IA ne doit jamais inventer une règle du Design System.

En cas d'information absente, contradictoire ou non confirmée, elle doit le signaler plutôt que compléter par déduction.

## Structure

- `ai/component-registry.json` — registre machine-readable des composants.
- `ai/prototype-brief.md` — brief type pour générer un prototype.
- `ai/validation-checklist.md` — checklist de conformité.
- `.github/copilot-instructions.md` — règles générales pour les agents.
- `.github/instructions/design-system.instructions.md` — règles de documentation et de provenance.
- Documentation composant existante — référence détaillée sans duplication inutile.

## Statuts

Les connaissances doivent conserver leur niveau de certitude : documenté, implémenté, observé, recommandation ou non confirmé.

## Workflow recommandé

1. Identifier les composants nécessaires.
2. Consulter le registre et la documentation correspondante.
3. Générer en réutilisant les composants existants.
4. Vérifier variantes, états, responsive et accessibilité.
5. Signaler explicitement toute hypothèse ou information non confirmée.

Il est préférable de commencer avec 10–15 composants représentatifs avant d'étendre le registre à l'ensemble du Design System.
