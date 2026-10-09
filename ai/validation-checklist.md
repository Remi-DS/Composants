# Checklist — fidélité du prototype à la maquette

Cette checklist évalue un prototype de test, pas une certification complète du Design System.

## A — Référence et couverture

- [ ] Les maquettes sources réellement accessibles sont identifiées.
- [ ] Tous les écrans/frames demandés sont reproduits.
- [ ] Les états supplémentaires demandés sont couverts.
- [ ] Aucun écran, bloc ou étape n'a été ajouté sans demande.
- [ ] Les formats et dimensions de viewport attendus sont vérifiés.

## B — Fidélité visuelle

- [ ] Structure, ordre des sections et alignements comparés à la référence.
- [ ] Hiérarchie, tailles et styles de texte comparés.
- [ ] Textes, libellés, chiffres et contenus visibles reproduits.
- [ ] Couleurs, bordures, ombres, rayons et espacements comparés.
- [ ] Images, illustrations, logos et icônes correspondent aux ressources fournies.
- [ ] Dimensions et proportions des composants comparées.
- [ ] Les écarts liés aux ressources ou aux informations manquantes sont consignés.

## C — Composants Canopée

- [ ] Les composants retenus existent dans le registre ou leur absence est signalée.
- [ ] La synthèse de chaque composant utilisé a été consultée.
- [ ] L'univers et l'API réellement disponibles dans le projet ont été vérifiés.
- [ ] Aucune variante, valeur de style ou règle Canopée n'a été inventée.
- [ ] Les différences entre la maquette et Canopée sont signalées, pas corrigées silencieusement.
- [ ] Tout élément spécifique en HTML/CSS est justifié par l'indisponibilité d'un composant adapté.

## D — Interactions et comportement

- [ ] Chaque interaction demandée a été exécutée manuellement.
- [ ] Les liens et boutons conduisent au résultat/destination attendu.
- [ ] Les saisies et sélections restent utilisables.
- [ ] Les états de formulaire demandés sont reproduits.
- [ ] Navigation, retour, fermeture et progression fonctionnent lorsqu'ils sont prévus.
- [ ] Aucun comportement métier non fourni n'a été inventé.
- [ ] Les interactions non spécifiées sont listées comme non confirmées ou provisoires.

## E — Responsive et accessibilité pragmatique

- [ ] Chaque viewport demandé a été comparé.
- [ ] Aucun breakpoint ou comportement responsive non sourcé n'est présenté comme une règle Canopée.
- [ ] Le clavier permet d'utiliser les interactions principales.
- [ ] Le focus reste visible et l'ordre de tabulation est cohérent.
- [ ] Les contrôles ont un nom accessible lorsque nécessaire.
- [ ] Les problèmes connus sont consignés ; aucune certification WCAG/RGAA n'est revendiquée.

## F — Transparence

- [ ] Les sources effectivement consultées sont listées.
- [ ] Les écarts visuels et fonctionnels sont explicites.
- [ ] Les hypothèses provisoires sont séparées des faits documentés.
- [ ] Les questions bloquantes sont formulées clairement.

## Verdict de livraison

- FIDÈLE — écarts mineurs documentés
- FIDÈLE AVEC RÉSERVES
- ÉCARTS IMPORTANTS
- NON ÉVALUABLE — référence ou informations insuffisantes

Le verdict décrit la fidélité à la maquette, pas une conformité globale au Design System.
