# Template — Étape de parcours

> Statut : RECOMMANDATION de prototypage. Ce n'est pas un gabarit officiel Canopée.

## Intention
Modéliser une étape d'un parcours multi-étapes sans inventer de composant de navigation ou de progression.

## Structure proposée
Header → Heading → indicateur de progression si justifié → contenu de l'étape → messages d'état éventuels → actions de navigation.

## Composants candidats
Header, Heading, Stepper, Progress Bar, Input Text, Input Date, Input Phone, Dropdown, Radio, Checkbox, Item Message, Button Primary, Button Secondary.

## Règles IA
- Stepper et Progress Bar sont des candidats ; ne pas les utiliser simultanément sans justification.
- Ne pas inventer le nombre d'étapes, les libellés ou les règles métier.
- Les actions doivent respecter les règles documentées des boutons.
- Chaque état représenté doit être traçable à une source ou au brief.