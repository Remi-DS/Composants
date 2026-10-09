# Instructions pour les agents IA — prototypes Canopée

## Mission

À partir d'une ou plusieurs maquettes existantes (Figma, capture ou export), produire un prototype interactif aussi fidèle que possible pour une revue ou un test utilisateur.

**Le prototype reproduit la maquette ; il ne redessine pas le produit.** Ne pas inventer une architecture de page, un parcours, du contenu métier, des règles de validation ou des interactions absentes du brief ou des sources. Si une information manque et empêche d'avancer fidèlement, poser une question ciblée ou signaler le point comme non confirmé.

## Ordre de priorité

1. Maquette de référence : structure, hiérarchie, contenu visible, composition et dimensions observables.
2. Brief utilisateur : écrans à couvrir et interactions explicitement attendues.
3. Documentation Zeroheight : règles d'usage et intentions documentées.
4. Fiches de synthèse et sources Storybook/code : API, rendu et comportements implémentés.
5. Registres IA : index, relations et recommandations, sans leur donner plus d'autorité que les sources.
6. Toute proposition non confirmée : la signaler, ne pas la présenter comme une règle Canopée.

Si la maquette contredit une règle ou une implémentation Canopée, ne pas la corriger silencieusement. Reproduire ce qui est demandé si c'est possible, et signaler explicitement l'écart.

## Workflow obligatoire

1. **Lire la référence** : identifier les écrans/états fournis, l'univers Prospect ou Client si connu, les formats Desktop/Mobile et les textes visibles. Ne pas prétendre avoir inspecté un fichier Figma inaccessible.
2. **Cartographier la maquette** : relever les zones, composants apparents, variantes potentiellement correspondantes, dimensions et liens entre écrans.
3. **Consulter les sources** : rechercher chaque composant candidat dans ai/component-registry.json, puis lire sa synthèse avant utilisation. Vérifier l'univers et le contexte technique réels du projet.
4. **Construire au plus près** : réutiliser les composants Canopée disponibles et adaptés. N'utiliser du HTML/CSS spécifique que si aucun composant adapté n'est disponible dans le contexte ; expliquer ce choix.
5. **Ajouter les interactions demandées** : clics, saisies, sélections, navigation entre écrans et états seulement selon la maquette ou le brief. Ne pas inventer de logique métier.
6. **Comparer et restituer** : vérifier visuellement chaque écran et tester chaque interaction demandée ; lister les écarts, hypothèses et questions restantes.

## Règles de fidélité

- Ne pas créer de nouvelle page ou d'étape de parcours simplement parce qu'un template existe.
- Ne pas changer les textes, la hiérarchie, les espacements, les couleurs ou la composition pour « améliorer » la maquette.
- Ne pas inventer de composants, variantes, tokens, breakpoints ou comportements.
- Une propriété d'API n'est pas une permission de design.
- Ne pas extrapoler une règle Prospect vers Client, ou Desktop vers Mobile, sans preuve.
- Si une image, une police, un token ou un composant n'est pas accessible, utiliser uniquement un remplacement explicitement identifié comme provisoire et documenter l'écart.
- Distinguer règle documentée, implémentation constatée, observation, recommandation et information non confirmée.

## Interactivité et ambiguïtés

- Implémenter les interactions explicitement demandées et celles dont le comportement est clairement visible dans la référence.
- Une maquette statique ne définit pas nécessairement le comportement d'erreur, la validation métier, le retour arrière ou la destination d'un lien.
- En cas d'ambiguïté, ne pas inventer de règle métier. Poser une question si la réponse change le prototype ; sinon utiliser un comportement provisoire minimal et le déclarer.
- Ne pas connecter de service réel ni envoyer de données utilisateur, sauf demande explicite et contexte technique prévu.

## Accessibilité et responsive

Préserver la référence tout en vérifiant les fondamentaux applicables au prototype : sémantique, navigation clavier, focus visible, nom accessible et utilisabilité. Contrôler les formats demandés. Ne pas prétendre certifier WCAG/RGAA.

## Compte rendu de livraison

Indiquer :
- les écrans et états reproduits ;
- les composants Canopée utilisés et les sources effectivement consultées ;
- les interactions implémentées et testées ;
- les écarts visuels connus ;
- les hypothèses provisoires et points non confirmés ;
- les questions bloquantes ou décisions attendues.

Ne pas déclarer une conformité globale au Design System. L'objectif est la fidélité du prototype à la maquette et la transparence sur les écarts.
