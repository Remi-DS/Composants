---
applyTo: "**/*.md,**/*.json"
---

# Documentation Design System

## Statuts de connaissance

Utiliser explicitement les statuts suivants :

- `DOCUMENTÉ` — règle explicitement documentée.
- `DOCUMENTÉ_VISUEL` — visible dans une source visuelle sans règle textuelle explicite.
- `CLARIFICATION_UTILISATEUR` — confirmé directement par l'équipe.
- `IMPLÉMENTÉ` — confirmé dans l'implémentation.
- `OBSERVÉ` — constaté sans être une règle normative.
- `RECOMMANDATION` — proposition, pas une règle existante.
- `NON_CONFIRMÉ` — information insuffisamment vérifiée.

## Principes

- Préserver les contradictions entre sources.
- Distinguer systématiquement design, implémentation et recommandation.
- Ne pas déduire une règle globale à partir d'un seul exemple visuel.
- Ne pas déduire une permission de design à partir d'une propriété d'API.
- Conserver la provenance des informations.

Les fichiers JSON doivent rester stables et router vers la documentation détaillée plutôt que la dupliquer.
