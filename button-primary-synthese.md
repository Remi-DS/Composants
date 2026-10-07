# Button Primary — Synthèse

Résumé opérationnel de [la documentation complète](./button-primary.md). Collecte du 6 octobre 2026.

> **Référence visuelle :** le Storybook de l'univers et de la version utilisés fait autorité. Les valeurs du JSON Figma ci-dessous sont conservées à titre comparatif ; en cas d'écart, ne pas les substituer au rendu du Storybook. Les règles d'usage et clarifications utilisateur restent applicables.

## Règles essentielles

| Sujet | Règle |
|---|---|
| Rôle | Mettre en évidence l'action principale de la page. Un Button agit sur la page ; une navigation relève d'un Link. |
| Nombre | **0 ou 1 Button Primary maximum par page**, tous styles confondus (Default, Business, Inverse), même dans des sections distinctes. |
| Composition | Contenu centré ; au plus une icône, à gauche ou à droite. Bibliothèque indiquée : Material Icons ; l'icône doit illustrer l'action. |
| État initial | Default ou Disabled. Hover, Active et Focus sont des états d'interaction. |
| Libellé | Une ligne ; **35 caractères maximum** (espaces compris) et **5 mots maximum**. Majuscule initiale seulement, sans ponctuation sauf l'exception ci-dessous. |
| Formulation | En général, infinitif + complément, explicite et orienté action. Pas de « je », « votre » ni de verbe conjugué. « mon/ma/mes » uniquement pour l'Espace Client ; « notre/nos » en communication institutionnelle. |
| Exceptions | Parcours : « Commencer », « Précédent », « Suivant ». Modale : reprendre le terme du titre ; confirmation « Oui, [verbe] » (virgule autorisée uniquement dans ce cas), jamais « Oui » seul. |
| Autres actions | Utiliser Secondary, Tertiary ou Ghost ; ne pas ajouter un deuxième Primary. |
| Hiérarchie des sources | Une possibilité offerte par l'API n'est pas une autorisation de design. Vérifier à nouveau le Storybook après une mise à jour de version. |

## Styles et intégration

| Style design | Variante React Prospect | Usage |
|---|---|---|
| Default | `primary` | Action primaire générale. |
| Business | `primary-business` | Action primaire liée au business, par exemple souscription ou prise de contact. |
| Inverse | `primary-inverse` | Action primaire sur fond foncé ; vérifier le contraste sur le fond réel. |

- Package : `@axa-fr/canopee-react/prospect`, export `Button`.
- `variant` vaut `primary` par défaut ; le type HTML vaut `button` par défaut. Préciser `type="submit"` pour une soumission de formulaire.
- Icône : fournir `iconLeft` **ou** `iconRight`, jamais les deux.
- `disabled` désactive le bouton. Dans l'implémentation Prospect consultée, `loading` le désactive également et affiche un Spinner ; l'indicateur correspond à l'état Disabled, pas à un état activable supplémentaire.
- Les limites du libellé ne sont pas validées automatiquement par le composant.

## Dimensions et responsive observés dans le CSS Prospect

| Contexte | Typographie | Padding vertical / horizontal |
|---|---|---|
| Mobile et tablette, largeur ≤ 1023 px | 16 px / interligne 24 px | 16 px / 24 px |
| Desktop, largeur > 1023 px | 18 px / interligne 32 px | 12 px / 24 px |

- Hauteur observée pour un libellé sur une ligne : **56 px** ; gap entre contenu et icône : **12 px**.
- Typographie : Source Sans Pro, graisse 600. Icônes d'exemple : 24 × 24 px.
- Exemples de largeur inspectés dans le design : Prospect 144 px (Desktop et Mobile) ; Client 180 px (Desktop), 125 px (Mobile). Ce sont des exemples inspectés, **pas des largeurs fixes universelles**.
- Ne pas imposer le rayon Prospect au Client : les univers ont des silhouettes distinctes.

## Couleurs relevées dans le CSS publié — Prospect

Valeurs du déploiement Storybook inspecté ; elles peuvent évoluer. Pour un autre univers ou une autre version, consulter son Storybook.

| Style / état | Fond | Texte |
|---|---|---|
| Default — normal | `#00008f` | `#ffffff` |
| Default — hover | `#000070` | `#ffffff` |
| Default — active | `#1a1a99` | `#ffffff` |
| Default — disabled | `#f5f5f5` | `#999999` |
| Business — normal | `#c84d14` | `#ffffff` |
| Business — hover / focus | `#be4913` | `#ffffff` |
| Business — active | `#d57244` | `#ffffff` |
| Inverse — normal | `#ffffff` | `#00008f` |
| Inverse — hover / focus | `#e3e3e3` | `#000070` |
| Inverse — active | `#f5f5f5` | `#1a1a99` |

Disabled utilise `#f5f5f5` / `#999999` dans les tokens communs ; le rendu des combinaisons Business/Inverse doit être vérifié. Focus visible : outline de 2 px, décalé de 3 px ; ne pas le supprimer.

## Tokens JSON Figma — comparatif, non prioritaires sur le Storybook

Dimensions `Prospect / Client` : rayon `100 / 8 px` ; padding vertical `12 / 16 px` ; padding horizontal `24 / 16 px` ; hauteur `56 / 56 px` ; gap `12 / 12 px`.

Couleurs JSON `Default / Business / Inverse` :

| État | Default | Business | Inverse |
|---|---|---|---|
| Normal | `#00008f` / blanc | `#c94e14` / blanc | blanc / `#00008f` |
| Hover — Prospect | `#000072` / blanc | `#bf4a13` / blanc | `#e2e2e2` / `#000072` |
| Active — Prospect | `#3333a5` / blanc | `#d47143` / blanc | `#f5f5f5` / `#3333a5` |
| Disabled | `#f5f5f5` / `#999999` | `#f5f5f5` / `#999999` | `#f5f5f5` / `#999999` |

Dans chaque cellule, la première couleur est le fond et la seconde est le texte et l'icône. Les états Inverse Client diffèrent : hover blanc / `#00008f`, active blanc / `#3333a5`. Ne pas extrapoler les styles Prospect à Client.

**Écarts à retenir :** rayon Prospect JSON `100 px` contre `32 px` dans le CSS Prospect inspecté ; padding vertical JSON commun `12 px` contre `16 px` sur le CSS jusqu'à 1023 px ; plusieurs couleurs Hover/Active et Business diffèrent également. En cas de besoin d'une valeur d'implémentation, retenir le Storybook courant de l'univers concerné et garder le JSON comme trace comparative.

## Accessibilité et limites

- Conserver la sémantique native du bouton, l'accès clavier (Tab, Entrée, Espace) et l'indicateur de focus visible.
- Fournir un nom accessible explicite ; masquer aux technologies d'assistance une icône purement décorative si nécessaire.
- Vérifier les contrastes du texte, des icônes et du focus sur le fond réellement utilisé. Cette synthèse ne certifie pas la conformité WCAG/RGAA.
- Types TypeScript exacts, implémentation Client et comportements d'autres versions : à vérifier dans la version installée et le Storybook correspondant.

Pour les sources, la traçabilité détaillée, les contradictions et les scénarios complets, consulter [la documentation complète](./button-primary.md).
