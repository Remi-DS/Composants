# Pattern / Template — Formulaire

> Statut : RECOMMANDATION de composition pour prototype. Ce fichier n'est pas une règle officielle Canopée.

## Intention
Permettre à un utilisateur de saisir ou sélectionner des informations dans un parcours.

## Structure proposée
1. Header si nécessaire.
2. Heading pour le titre de l'étape.
3. Champs nécessaires au parcours.
4. Item Message pour les messages d'état lorsqu'approprié.
5. Button Primary pour l'action principale.
6. Button Secondary uniquement si une action alternative est réellement nécessaire et conformément à sa documentation.

## Composants candidats
Input Text, Input Date, Input Phone, Dropdown, Radio / Radio Text, Checkbox / Checkbox Text, Fieldset, Item Label, Item Message, Button Primary, Button Secondary.

## Règles IA
- Ne pas ajouter tous les composants candidats par défaut.
- Choisir le contrôle selon la nature de la donnée et la documentation du composant.
- Vérifier la fiche complète de chaque composant sélectionné.
- Ne pas inventer de validation métier.
- Utiliser `ai/responsive-grid.md`.
- Fournir composants, états, hypothèses et points NON_CONFIRMÉ dans la validation.