# Workflow opérationnel — maquette vers prototype interactif

## Entrées

- Une maquette existante accessible (Figma, capture ou export).
- La liste des écrans/frames à reproduire.
- Les dimensions ou formats de viewport attendus.
- Le brief des interactions connues.
- Le contexte technique du prototype, si imposé.

Si la référence visuelle n'est pas accessible, demander un export ou une capture. Ne pas décrire comme inspectée une maquette inaccessible.

## Étape 1 — Inventorier la référence

Pour chaque écran, relever :
- taille de frame et viewport ;
- structure, zones, alignements et espacements visibles ;
- textes et contenus exacts ;
- composants apparents, images, icônes et ressources ;
- états visibles et liens vers les autres écrans ;
- différences entre les frames desktop/mobile, si les deux existent.

Ne pas déduire les comportements interactifs d'une simple capture.

## Étape 2 — Établir les interactions

Créer une liste explicite des actions attendues : élément déclencheur, action (clic/saisie/sélection), résultat, écran cible et état éventuel. Associer chaque information à sa source : maquette, brief ou clarification utilisateur.

Si une interaction critique n'est pas définie (par exemple erreur de formulaire ou destination d'un lien), demander une clarification. Ne pas inventer de règle métier.

## Étape 3 — Mapper les composants

Pour chaque élément visuel :
1. Chercher un composant candidat dans ai/component-registry.json.
2. Lire sa fiche de synthèse.
3. Vérifier sa disponibilité dans l'univers et le runtime du prototype.
4. Vérifier les variantes et comportements documentés.
5. En cas d'absence de composant adapté, construire un élément spécifique minimal et documenter pourquoi.

Le registre est un index, pas une preuve suffisante de compatibilité technique ou de conformité dans tous les univers.

## Étape 4 — Implémenter

- Construire les écrans dans l'ordre demandé.
- Reproduire d'abord la structure et les styles visibles, puis ajouter les interactions.
- Utiliser les ressources fournies ; éviter les remplacements arbitraires.
- Ne pas ajouter de pages, de contenu ou de sections simplement pour compléter un template.
- Garder les comportements provisoires simples et les identifier clairement.
- Ne pas connecter de backend ni envoyer de données réelles sans demande explicite.

## Étape 5 — Comparer visuellement

Comparer chaque écran avec la référence à une taille de viewport équivalente. Vérifier :
- dimensions, positionnement, alignements et densité ;
- typographie, couleurs, bordures et espacements ;
- textes, images, icônes et proportions ;
- composants et états visibles.

Documenter chaque écart, sa cause connue et s'il est bloquant. Ne pas corriger silencieusement une différence entre maquette et Canopée.

## Étape 6 — Tester les interactions

Exécuter chaque action du brief et vérifier le résultat réel : navigation, saisie, sélection, états demandés, retour/fermeture. Ne pas écrire « testé » pour un comportement qui n'a pas été exécuté.

## Étape 7 — Livrer

Fournir :
- écrans couverts ;
- interactions réellement testées ;
- composants et sources consultées ;
- écarts visuels et fonctionnels connus ;
- hypothèses provisoires et points non confirmés ;
- questions ou décisions restantes.

Utiliser ai/validation-checklist.md. Le verdict concerne la fidélité à la maquette et non une certification globale Canopée, WCAG ou RGAA.
