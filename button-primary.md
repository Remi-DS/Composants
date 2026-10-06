# Button Primary — Documentation structurée

Version tabulaire de la note v4. Collecte et compléments utilisateur : 6 octobre 2026.
Réorganisation de forme uniquement : règles, exceptions, provenance, comportements techniques et limites sont conservés.

## À retenir avant toute utilisation

| Priorité | Règle de décision |
|---|---|
| 1 — Hiérarchie | Au maximum un Primary par page, tous styles confondus, même dans des sections distinctes. |
| 2 — Composition | Contenu centré ; aucune icône ou une seule. |
| 3 — Libellé | Une ligne ; 35 caractères, espaces compris ; 5 mots maximum ; exceptions limitées aux contextes documentés. |
| 4 — Intégration | Une possibilité de l’API n’annule jamais une interdiction de design. |
| 5 — Fiabilité | Conserver les réserves et faire valider les conflits non résolus plutôt que déduire une permission. |
| 6 — Source de référence — CLARIFICATION_UTILISATEUR, 2026-10-06 | Le Storybook est toujours la référence. En cas d'écart de valeurs visuelles avec le JSON Figma, retenir le Storybook ; conserver le JSON comme donnée comparative, sans effacer les écarts. Les règles d'usage et clarifications restent applicables : une capacité technique n'est pas une permission de design. |

## Mode de lecture

| Colonne | Signification |
|---|---|
| Sous-contexte | Présent uniquement quand un tableau contient plusieurs niveaux imbriqués ; ils sont séparés par `/`. |
| Élément | Propriété, contrainte, statut ou détail concerné. |
| Information / règle | Valeur ou formulation conservée de la note. |

Les indications de provenance précèdent les tableaux concernés. Une référence ou un statut plus précis dans une cellule s’applique à cette donnée. Les mentions NON_CONFIRMÉ et les contradictions restent des limites, pas des autorisations.

## Sommaire

- [Métadonnées](#section-metadonnees)
- [Contrat prioritaire pour l’IA](#section-contrat)
- [01. PÉRIMÈTRE ET FIABILITÉ](#section-01)
- [02. SOURCES ET TRAÇABILITÉ](#section-02)
- [03. IDENTITÉ ET INTENTION](#section-03)
- [04. ANATOMIE ET COMPOSITION](#section-04)
- [05. VARIANTES ET CORRESPONDANCES](#section-05)
- [06. RÈGLES D'USAGE ET INTERDITS](#section-06)
- [07. CONTENU ET MICROCOPY](#section-07)
- [08. CONTRAT D'INTÉGRATION REACT](#section-08)
- [09. INTERACTIONS ET ÉTATS](#section-09)
- [10. STYLE, TOKENS ET RESPONSIVE](#section-10)
- [11. ACCESSIBILITÉ ET SÉMANTIQUE](#section-11)
- [12. SCÉNARIOS D'INTÉGRATION TEXTUELS](#section-12)
- [13. CONSIGNES D'EXPLOITATION POUR UNE IA](#section-13)
- [14. CHECKLIST DE VALIDATION](#section-14)
- [15. CONTRADICTIONS ET PRIORITÉ DES RÈGLES](#section-15)
- [16. COUVERTURE DE LA DESCRIPTION ET CONTRÔLES POUR L'IA](#section-16)
- [17. Compléments utilisateur et tokens JSON Figma](#section-17)

<a id="section-metadonnees"></a>

## Métadonnées

| Élément | Information / règle |
|---|---|
| NOTE_COMPOSANT | BUTTON PRIMARY |
| FORMAT | texte structuré destiné à une IA |
| LANGUE | français |
| DATE_DE_COLLECTE | 2026-10-06 |
| VERSION_DE_LA_NOTE | 4 |

<a id="section-contrat"></a>

## Contrat prioritaire pour l’IA

| Élément | Information / règle |
|---|---|
| PORTÉE | règles de conception à appliquer, indépendamment de ce que l'API permet techniquement. |
| CARDINALITÉ_PAGE | 0 ou 1 Button Primary, tous styles confondus. |
| CARDINALITÉ_ICÔNE | 0 ou 1 icône. |
| STYLE | Default ; Business ; Inverse. |
| ÉTAT_INITIAL | Default ou Disabled uniquement. |
| ALIGNEMENT_CONTENU | centré. |
| LIGNES_LIBELLÉ | 1 maximum. |
| CARACTÈRES_LIBELLÉ | 35 maximum, espaces compris. |
| MOTS_LIBELLÉ | 5 maximum. |
| CASSE_LIBELLÉ | majuscule initiale uniquement. |
| STRUCTURE_LIBELLÉ | infinitif + complément, sauf exceptions contextuelles explicites de la section 07. |
| DIMENSIONS | conserver celles du composant officiel pour l'univers et l'appareil concernés. |
| AUTRES_ACTIONS | Secondary, Tertiary ou Ghost ; jamais un second Primary. |
| INTERPRÉTATION | une capacité technique ne constitue pas une permission de design. |
| EXCEPTIONS | ne pas inventer d'exception à une interdiction ; appliquer uniquement les exceptions contextuelles consignées. |
| CONFLITS | appliquer la clarification utilisateur pour la cardinalité ; pour les autres contradictions non résolues, conserver le conflit et demander une validation plutôt que choisir arbitrairement. |

<a id="section-01"></a>

## 01. PÉRIMÈTRE ET FIABILITÉ

| Élément | Information / règle |
|---|---|
| Nom fonctionnel | Button Primary. |
| Design system | Canopée, AXA France. |
| Univers documentaire | Client et Prospect. |
| Implémentation technique examinée | React, Prospect. |
| Composant React | Button. |
| Variante principale | primary. |
| Variantes primaires associées | primary-business ; primary-inverse. |
| Hors périmètre technique | implémentation React Client ; implémentations autres que React ; variantes non primaires. |
| Périmètre design | Prospect et Client, y compris les illustrations et les quatre structures inspectables de l'onglet Styles. |
| Statut de la page design fournie | page intitulée « Test V2 Button Primary », marquée comme en chantier. |
| Statut des variables dans l'onglet Styles | en cours de conception. |
| Version des sources | latest, donc mutable. |
| Version sémantique exacte de la bibliothèque déployée | non déterminée. |

### Convention de lecture

| Élément | Information / règle |
|---|---|
| DOCUMENTÉ | règle ou donnée explicitement présente dans Zeroheight ou Storybook. |
| DOCUMENTÉ_VISUEL | donnée lisible dans une illustration ou l'inspecteur de design Zeroheight ; sa portée et ses éventuels conflits sont indiqués. |
| CLARIFICATION_UTILISATEUR | règle explicitement précisée par l'utilisateur, à appliquer même si les textes extraits des sources sont moins explicites. |
| IMPLÉMENTÉ | comportement constaté dans le JavaScript ou le CSS publiés par Storybook. |
| OBSERVÉ | donnée mesurée dans le navigateur sur le Storybook. |
| RECOMMANDATION | consigne d'intégration ou de vérification proposée par cette note ; ne constitue pas une citation du design system. |
| NON_CONFIRMÉ | donnée absente, inaccessible sous forme textuelle ou non vérifiée. |

### Couverture et limites

| Information / règle |
|---|
| Cette note consigne les règles textuelles des six onglets Zeroheight, les règles illustrées Do/Don't/Caution, l'anatomie, les variantes et les attributs racines des quatre structures inspectables. |
| Les illustrations sont reformulées en données textuelles ; les images elles-mêmes ne sont pas incorporées. |
| Les captures de pages applicatives sont des exemples de contexte, pas des autorisations implicites ni des spécifications de leurs autres composants. |
| Les détails non lisibles des captures et les propriétés des sous-calques non inspectés ne sont pas inventés. |
| Les signatures TypeScript originales n'ont pas été récupérées ; les types exacts doivent être vérifiés dans la version de bibliothèque utilisée. |
| Ne pas interpréter cette note comme une certification d'accessibilité ou une spécification définitive. |

<a id="section-02"></a>

## 02. SOURCES ET TRAÇABILITÉ

### S1 — Storybook fourni

| Élément | Information / règle |
|---|---|
| URL | https://axafrance.github.io/design-system/prospect/react/latest/?path=/docs/components-button--button |
| Contenu | import, propriétés exposées, variantes, exemples. |

### S2 — Zeroheight fourni

| Élément | Information / règle |
|---|---|
| URL | https://zeroheight.com/49b6215d6/v/latest/p/311401 |
| URL résolue | https://zeroheight.com/49b6215d6/v/latest/p/311401--test-v2-button-primary |
| Contenu | description, anatomie, variantes et propriétés design. |

### S3 — Guidelines d'utilisation

| Élément | Information / règle |
|---|---|
| URL | https://zeroheight.com/49b6215d6/v/latest/p/311401--test-v2-button-primary/b/901482 |

### S4 — Styles

| Élément | Information / règle |
|---|---|
| URL | https://zeroheight.com/49b6215d6/v/latest/p/311401--test-v2-button-primary/b/96d34e |

### S5 — Contenu

| Élément | Information / règle |
|---|---|
| URL | https://zeroheight.com/49b6215d6/v/latest/p/311401--test-v2-button-primary/b/867485 |

### S6 — Accessibilité

| Élément | Information / règle |
|---|---|
| URL | https://zeroheight.com/49b6215d6/v/latest/p/311401--test-v2-button-primary/b/15589b |

### S7 — Code

| Élément | Information / règle |
|---|---|
| URL | https://zeroheight.com/49b6215d6/v/latest/p/311401--test-v2-button-primary/b/47deef |
| Observation | distingue Prospect et Client ; les exemples exécutables exploités ici proviennent de S1. |

### S8 — Implémentation JavaScript publiée

| Élément | Information / règle |
|---|---|
| URL | https://axafrance.github.io/design-system/prospect/react/latest/assets/prospect-D-Y_ORHe.js |

### S9 — Styles du composant publiés

| Élément | Information / règle |
|---|---|
| URL | https://axafrance.github.io/design-system/prospect/react/latest/assets/prospect-BUEXi3sf.css |

### S10 — Tokens CSS publiés

| Élément | Information / règle |
|---|---|
| URL | https://axafrance.github.io/design-system/prospect/react/latest/assets/iframe-B-Lhb827.css |

### S11 — Rendu utilisé pour les observations

| Élément | Information / règle |
|---|---|
| URL | https://axafrance.github.io/design-system/prospect/react/latest/iframe.html?id=components-button--button&viewMode=docs |

### Note de provenance

| Information / règle |
|---|
| Les noms de fichiers compilés contiennent des empreintes propres au déploiement consulté. |
| Ils peuvent changer lors d'une publication ; S1 et S2 restent les points d'entrée de référence. |

### S12 — Structures Figma accessibles depuis l'inspecteur Styles

| Élément | Information / règle |
|---|---|
| Prospect Desktop | https://www.figma.com/design/vwprvN2ELfI50pjU6MK1Ea/?node-id=28349:12758 |
| Prospect Mobile | https://www.figma.com/design/vwprvN2ELfI50pjU6MK1Ea/?node-id=36:4413 |
| Client Desktop | https://www.figma.com/design/vwprvN2ELfI50pjU6MK1Ea/?node-id=64:39467 |
| Client Mobile | https://www.figma.com/design/vwprvN2ELfI50pjU6MK1Ea/?node-id=64:39509 |
| Provenance des valeurs | inspecteur Zeroheight ; aucune inspection directe supplémentaire dans Figma. |

<a id="section-03"></a>

## 03. IDENTITÉ ET INTENTION

DOCUMENTÉ — Sources : S2, S3.

| Élément | Information / règle |
|---|---|
| Catégorie | Action. |
| Nature | élément interactif. |
| Rôle | mettre en évidence l'action principale d'une interface et inciter à l'effectuer. |
| Objectif | rendre l'action importante identifiable et clarifier ce qu'elle déclenche. |
| Exemples de fonctions | soumettre ; enregistrer ; finaliser ; continuer. |

### Cas spécifiques documentés

| Information / règle |
|---|
| Confirmer une action critique dans une modale d'interruption ou de reprise de parcours. |
| Valider un tarif en fin de parcours. |
| Valider un rendez-vous avec un conseiller en fin de parcours. |

### Distinction sémantique — Source : S6.

| Élément | Information / règle |
|---|---|
| Précision | Une action qui agit sur la page elle-même relève d'un Button. |
| Précision | Button et Link doivent rester distincts. |
| RECOMMANDATION | une navigation vers une autre ressource relève d'un lien, même si son apparence est celle d'un bouton. |
| RECOMMANDATION | ne pas substituer un élément non interactif à un bouton natif. |

<a id="section-04"></a>

## 04. ANATOMIE ET COMPOSITION

DOCUMENTÉ — Source : S2.

| Élément | Information / règle |
|---|---|
| Parties identifiées | Background ; Text ; Icon. |
| Texte | libellé visible décrivant l'action. |
| Icône | facultative. |
| Positions design de l'icône | Left ; Right ; Masqué. |
| Bibliothèque d'icônes — CLARIFICATION_UTILISATEUR, 2026-10-06 | Material Icons. |
| Choix de l'icône — CLARIFICATION_UTILISATEUR | Aucune liste spécifique d'icônes autorisées pour le Button Primary ; choisir dans Material Icons une icône en rapport avec l'action déclenchée. |
| Limite d'interprétation | Ne pas inventer de liste restreinte ni imposer une icône précise pour une action ; préserver les contraintes de nombre, de position et d'accessibilité. |
| Alignement | contenu toujours centré. |
| Nombre maximal d'icônes autorisé par les guidelines | une. |
| Sans icône | libellé centré. |
| Avec icône gauche | icône avant le libellé. |
| Avec icône droite | icône après le libellé. |

DOCUMENTÉ_VISUEL — Source : S2.

### Repères du schéma

| Élément | Information / règle |
|---|---|
| 1 | fond du bouton. |
| 2 | texte du bouton. |
| 3 | icône. |

### Différence entre univers

| Élément | Information / règle |
|---|---|
| Prospect | silhouette pilule, fortement arrondie. |
| Client | rectangle à coins arrondis. |
| Précision | Les deux univers présentent Desktop/Mobile, Default/Business/Inverse et les cinq états. |
| Précision | Ne pas appliquer automatiquement le rayon Prospect à l'univers Client. |

IMPLÉMENTÉ — Source : S8.

| Élément | Information / règle |
|---|---|
| Élément racine | button HTML natif. |
| Ordre des enfants | iconLeft ; children ; iconRight ; indicateur de chargement conditionnel. |
| Enveloppe de texte dédiée | aucune dans le rendu de base inspecté. |
| Classe de base | af-btn-client. |
| Classe de variante primaire | af-btn-client--primary. |
| Classe Business | af-btn-client--primary-business. |
| Classe Inverse | af-btn-client--primary-inverse. |
| Classe personnalisée | className est ajoutée aux classes du composant. |

### Écart à connaître

| Élément | Information / règle |
|---|---|
| Précision | L'implémentation accepte simultanément iconLeft et iconRight. |
| Précision | Les guidelines interdisent plusieurs icônes. |
| RECOMMANDATION | fournir au maximum l'une de ces deux propriétés. |

<a id="section-05"></a>

## 05. VARIANTES ET CORRESPONDANCES

DOCUMENTÉ — Source : S2.

### Propriété design State

| Élément | Information / règle |
|---|---|
| Valeurs | Default ; Hover ; Active ; Disabled ; Focus. |

### Propriété design Device

| Élément | Information / règle |
|---|---|
| Valeurs | Desktop ; Mobile. |

### Propriété design Style

| Élément | Information / règle |
|---|---|
| Valeurs | Default ; Business ; Inverse. |

### Propriété design Icon

| Élément | Information / règle |
|---|---|
| Valeurs | Right ; Left ; Masqué. |

### Correspondances avec React — Sources : S1, S8.

| Élément | Information / règle |
|---|---|
| Style Default | variant = primary. |
| Style Business | variant = primary-business. |
| Style Inverse | variant = primary-inverse. |
| Icône Left | iconLeft renseignée, iconRight absente. |
| Icône Right | iconRight renseignée, iconLeft absente. |
| Icône Masqué | iconLeft et iconRight absentes. |
| Device | adaptation par CSS responsive ; aucune propriété Device identifiée. |
| Hover, Active, Focus | états CSS de l'interaction ; aucune propriété State identifiée. |
| Disabled | propriété disabled. |

### Choix du style — Source : S3.

| Élément | Information / règle |
|---|---|
| Default | actions primaires générales. |
| Business | actions primaires liées au business, notamment souscription et prise de contact. |
| Inverse — CLARIFICATION_UTILISATEUR, 2026-10-06 | Mettre en avant l'action primaire sur un fond foncé afin de préserver son accessibilité. Contrôler le contraste sur le fond réellement utilisé ; le style seul ne certifie pas la conformité. |
| OBSERVÉ — Source S1 | les exemples de variantes Inverse sont présentés sur fond bleu. |
| RECOMMANDATION | sélectionner Inverse sur fond foncé, après contrôle du contraste. |

### Autres variantes du composant Button — Source : S1.

| Information / règle |
|---|
| secondary ; secondary-inverse ; tertiary ; ghost. |
| Ces variantes ne sont pas des styles du Button Primary. |
| Elles peuvent servir à hiérarchiser des actions complémentaires. |

<a id="section-06"></a>

## 06. RÈGLES D'USAGE ET INTERDITS

DOCUMENTÉ — Source : S3.

### Règles

| Information / règle |
|---|
| Mettre en avant une action importante. |
| Utiliser un état de base Default ou Disabled. |
| Réserver Hover, Active et Focus aux interactions correspondantes. |
| Centrer le contenu. |
| Utiliser une icône uniquement pour illustrer l'action. |
| Associer, si nécessaire, l'action primaire à une action Secondary, Tertiary ou Ghost. |
| Respecter les dimensions du composant. |

### Interdits

| Information / règle |
|---|
| Plusieurs icônes dans un même Button Primary. |
| Deux Button Primary côte à côte. |
| Contenu non centré. |
| Texte sur deux lignes. |
| Modification arbitraire de la taille. |
| État de base autre que Default ou Disabled. |
| Libellé entièrement en majuscules. |
| Majuscule à chaque mot. |

### DOCUMENTÉ_VISUEL — Source : S3.

| Information / règle |
|---|
| L'exemple interdit de deux Primary montre aussi deux boutons empilés verticalement. |
| L'interdiction ne se limite donc pas à un placement horizontal. |
| L'exemple autorisé associe « Envoyer » en Primary à « Fermer » en Secondary. |
| Les boutons multiples dans un schéma comparatif présentent des variantes ; ils ne constituent pas un modèle de page à reproduire. |

### Règle de cardinalité — CLARIFICATION_UTILISATEUR, 2026-10-06

| Élément | Information / règle |
|---|---|
| Précision | Une même page ne peut contenir qu'un seul Button Primary au maximum. |
| Précision | Cette limite s'applique aussi lorsque les boutons seraient dans des sections distinctes. |
| Elle concerne l'ensemble des styles primaires | Default, Business et Inverse. |
| Précision | Les autres actions utilisent une hiérarchie Secondary, Tertiary ou Ghost selon le contexte. |
| Précision | La formulation « deux Button Primary côte à côte » des textes extraits ne constitue pas une autorisation d'en placer plusieurs ailleurs sur la page. |
| Précision | Cette règle globale a été précisée par l'utilisateur ; ne pas l'attribuer à une formulation explicitement relevée dans Zeroheight. |

<a id="section-07"></a>

## 07. CONTENU ET MICROCOPY

DOCUMENTÉ — Sources : S3, S5.

| Élément | Information / règle |
|---|---|
| Construction habituelle | verbe à l'infinitif suivi d'un complément. |
| Exemples documentés | Obtenir un tarif ; Contacter un conseiller ; Trouver une agence. |
| Sens | décrire clairement l'action déclenchée. |
| Précision | éviter les formulations génériques ; ajouter du contexte quand il aide l'utilisateur. |
| Nombre de lignes | une. |
| Longueur maximale | 35 caractères, espaces compris. |
| Nombre maximal de mots | 5. |
| Casse | une majuscule initiale uniquement. |
| Ponctuation | aucune. |

### Pronoms et exceptions

| Élément | Information / règle |
|---|---|
| mon ; ma ; mes | uniquement pour un renvoi vers l'Espace Client. |
| notre ; nos | uniquement dans une communication institutionnelle. |
| nous | possible pour un contact, notamment « Nous contacter », ou certains renvois marketing vers une offre ou un service. |
| je | interdit dans le libellé. |
| votre | interdit dans le libellé. |
| Verbe conjugué | interdit dans le libellé. |
| Verbe à l'infinitif sans complément | permis pour des actions habituelles telles que Annuler, Fermer ou Continuer. |

### Règles contextuelles des parcours à étapes — DOCUMENTÉ_VISUEL, source S5

| Élément | Information / règle |
|---|---|
| Début d'un parcours | utiliser « Commencer ». |
| Retour à l'étape précédente | utiliser « Précédent ». |
| Passage à l'étape suivante | utiliser « Suivant ». |
| Précision | Ces libellés contextuels sont des exceptions explicites à la construction infinitif + complément. |
| Précision | L'illustration associe l'action de retour non primaire à l'action d'avancement primaire. |
| Précision | Ne pas transformer « Précédent » en un second Primary. |

### Règles contextuelles des modales — DOCUMENTÉ_VISUEL, source S5

| Élément | Information / règle |
|---|---|
| Précision | Reprendre le terme d'action ou l'objet utilisé dans le titre de la modale. |
| Précision | Ne pas remplacer ce terme par un synonyme qui crée une différence avec le titre. |
| Exemple cohérent | titre évoquant des modifications ; libellé « Voir les modifications ». |
| Contre-exemple | même titre ; libellé « Voir les changements ». |
| Confirmation | utiliser « Oui » accompagné du verbe de l'action évoquée dans le titre. |
| Exemple illustré | titre demandant de quitter ; libellé « Oui, quitter ». |
| Précision | Ne pas utiliser « Oui » seul. |
| Précision | L'action alternative illustrée est « Non, reprendre », avec une hiérarchie non primaire. |
| Précision | Ces illustrations sont des exceptions contextuelles à la structure infinitif + complément. |
| CLARIFICATION_UTILISATEUR, 2026-10-06 | La virgule est autorisée uniquement dans les confirmations de type « Oui, quitter ». La règle générale reste sans ponctuation ; ne pas étendre cette exception à tous les boutons. |

### Exemples éditoriaux consignés — DOCUMENTÉ_VISUEL, source S5

| Élément | Information / règle |
|---|---|
| CONFORME | « Trouver une agence » ; action à l'infinitif avec complément. |
| INTERDIT | « Trouvez une agence » ; verbe conjugué. |
| INTERDIT | « Payer votre cotisation » ; emploi de votre. |
| INTERDIT | « Je paye ma cotisation » ; emploi de je et verbe conjugué. |
| CONDITIONNEL | « Payer ma cotisation » ; uniquement pour l'Espace Client connecté. |
| CONDITIONNEL | « Découvrir nos engagements » ; uniquement en communication institutionnelle. |
| CONFORME | libellé qui nomme l'offre à découvrir plutôt qu'un « Découvrir » générique. |
| À PRÉCISER | « Découvrir » ; compléter avec le contexte lorsque possible. |
| INTERDIT COMME MODÈLE | « Tous les conseils » ; ne décrit pas clairement une action. |
| INTERDIT COMME MODÈLE | « Ok » ; trop générique. |
| INTERDIT COMME MODÈLE | « En savoir plus » ; ne précise pas suffisamment l'action. |

### Exemples de casse — DOCUMENTÉ_VISUEL, source S3

| Élément | Information / règle |
|---|---|
| CONFORME | « Envoyer » ; « Envoyer mon document ». |
| INTERDIT | « ENVOYER » ; « Envoyer Mon Document ». |
| Précision | L'exemple de casse « Envoyer mon document » n'annule pas la restriction contextuelle sur mon/ma/mes. |

### Limite d'implémentation — Source : S8.

| Élément | Information / règle |
|---|---|
| Précision | Aucune validation automatique de longueur, de nombre de mots, de casse ou de ponctuation identifiée dans Button. |
| Précision | Aucun mécanisme de réécriture automatique du libellé identifié. |
| RECOMMANDATION | contrôler ces contraintes avant de fournir children. |
| RECOMMANDATION | compter les caractères affichés, espaces inclus ; la convention Unicode précise de comptage n'est pas spécifiée par la documentation. |
| RECOMMANDATION | raccourcir le libellé plutôt que réduire la typographie ou forcer une seconde ligne. |

<a id="section-08"></a>

## 08. CONTRAT D'INTÉGRATION REACT

Sources : S1, S8.

| Élément | Information / règle |
|---|---|
| Package et point d'entrée | @axa-fr/canopee-react/prospect. |
| Export à utiliser | Button. |
| Variante par défaut implémentée | primary. |
| Type HTML par défaut implémenté | button. |
| Transmission des propriétés | les propriétés restantes sont transmises au bouton HTML natif. |
| Effet | type peut notamment être remplacé par submit ou reset. |

### Propriété children

| Élément | Information / règle |
|---|---|
| Rôle | contenu rendu au centre du bouton. |
| Storybook | contrôle présenté comme string. |
| Implémentation | contenu rendu directement comme enfant React. |
| Type TypeScript exact et caractère obligatoire | NON_CONFIRMÉS. |
| Consigne d'usage | fournir un libellé textuel conforme à la section 07. |

### Propriété variant

| Élément | Information / règle |
|---|---|
| Rôle | sélection de l'apparence. |
| Valeur par défaut | primary. |
| Valeurs primaires connues | primary ; primary-business ; primary-inverse. |
| Ensemble de variantes connues | primary ; primary-business ; primary-inverse ; secondary ; secondary-inverse ; tertiary ; ghost. |
| Documentation | une valeur personnalisée produit une classe af-btn-client-- suivie du modificateur. |
| Type TypeScript exact et autorisation typée des valeurs personnalisées | NON_CONFIRMÉS. |
| RECOMMANDATION | utiliser une variante officielle ; ne pas inventer une variante sans vérifier le contrat et ses styles. |

### Propriété onClick

| Élément | Information / règle |
|---|---|
| Rôle | gestion de l'activation. |
| Storybook | action onClick exposée. |
| Implémentation | propriété transmise au bouton natif. |
| Signature TypeScript exacte | NON_CONFIRMÉE. |

### Propriétés iconLeft et iconRight

| Élément | Information / règle |
|---|---|
| Rôle | contenu d'icône avant ou après le libellé. |
| Storybook | contrôles textuels. |
| Exemples | éléments Svg React. |
| Implémentation | contenu rendu directement. |
| Type TypeScript exact | NON_CONFIRMÉ. |
| Limite design | une seule icône au total. |
| RECOMMANDATION | ne pas déduire du contrôle textuel que l'API attend exclusivement une chaîne ou une URL. |

### Propriété disabled

| Élément | Information / règle |
|---|---|
| Rôle | désactivation native. |
| Évaluation implémentée | disabled ou loading. |
| Absence de valeur | pas de désactivation due à cette propriété. |
| Effet supplémentaire actuel | affiche aussi le Spinner. |

### Propriété loading

| Élément | Information / règle |
|---|---|
| Rôle | état de chargement implémenté. |
| Effets | désactive le bouton et affiche le Spinner. |
| Présence dans la table des contrôles consultée | non exposée. |
| Type TypeScript exact | NON_CONFIRMÉ. |

### Propriété className

| Élément | Information / règle |
|---|---|
| Rôle | ajout d'une classe au bouton. |
| Effet | préserve les classes de base et de variante. |
| RECOMMANDATION | ne pas employer cette extension pour contourner les règles de taille ou d'alignement. |

### Propriétés HTML et ARIA

| Élément | Information / règle |
|---|---|
| Exemples applicables | type ; title ; id ; name ; value ; aria-label ; aria-labelledby ; aria-describedby. |
| Transmission au bouton natif | IMPLÉMENTÉE. |
| Validation de leurs valeurs | responsabilité de l'intégration et du contrat de types. |
| Propriété href | ne transforme pas Button en lien. |

### SpinnerComponent

| Élément | Information / règle |
|---|---|
| Présence | paramètre de l'implémentation interne commune. |
| Composant Prospect exporté | injecte lui-même son Spinner. |
| RECOMMANDATION | ne pas traiter SpinnerComponent comme une API publique personnalisable sans vérification des sources typées. |

<a id="section-09"></a>

## 09. INTERACTIONS ET ÉTATS

Sources : S6, S8, S9.

### Default

| Information / règle |
|---|
| Bouton activable lorsqu'aucune désactivation n'est demandée. |

### Hover

| Information / règle |
|---|
| Changement de fond au survol. |

### Active

| Information / règle |
|---|
| Changement de fond pendant l'activation. |

### Focus

| Information / règle |
|---|
| Focus natif ; indicateur visible via focus-visible. |

### Disabled

| Information / règle |
|---|
| disabled natif lorsque disabled ou loading est vrai. |
| Style désactivé et suppression des interactions de pointeur via CSS. |
| Spinner également présent dans l'implémentation inspectée. |

### Loading

| Information / règle |
|---|
| État technique supplémentaire ; ne figure pas dans les cinq états design listés. |
| Libellé visible conservé. |
| Spinner ajouté après les autres enfants. |
| Bouton désactivé. |

### Indicateur de chargement — Source : S8.

| Élément | Information / règle |
|---|---|
| Taille injectée | 24. |
| Variante injectée | gray. |
| Élément | div. |
| Rôle | alert. |
| aria-busy | true. |
| aria-live | assertive. |
| aria-label | Chargement en cours. |

### Point de vigilance majeur

| Élément | Information / règle |
|---|---|
| Précision | disabled et loading déclenchent le même indicateur. |
| CLARIFICATION_UTILISATEUR, 2026-10-06 | L'indicateur de chargement s'affiche uniquement dans l'état Disabled. Un chargement rend donc le bouton désactivé ; ce n'est pas un sixième état visuel activable. |
| Précision | Ne pas documenter un Disabled sans Spinner comme le comportement actuel de ce déploiement. |
| RECOMMANDATION | vérifier ce comportement dans la version installée et les annonces accessibles dans leur contexte. La clarification porte sur l'état visuel, pas sur la suppression de la propriété technique loading constatée. |

### aria-disabled seul

| Élément | Information / règle |
|---|---|
| IMPLÉMENTÉ — Source S9 | déclenche le style désactivé et pointer-events: none. |
| Précision | Ne produit pas à lui seul l'attribut disabled natif dans l'implémentation inspectée. |
| RECOMMANDATION | ne pas l'utiliser comme remplacement de disabled sans gérer explicitement l'activation clavier et les événements. |

### Gestion applicative

| Élément | Information / règle |
|---|---|
| Précision | Aucun traitement métier, navigation, appel réseau ou état asynchrone interne identifié. |
| RECOMMANDATION | fournir le gestionnaire d'action et piloter loading depuis l'application. |
| RECOMMANDATION | traiter et signaler les erreurs de l'action ; ne pas considérer l'affichage du bouton comme une confirmation de succès. |

<a id="section-10"></a>

## 10. STYLE, TOKENS ET RESPONSIVE

Sources : S9, S10 ; observations dans S11.

| Information / règle |
|---|
| Les valeurs en pixels ci-dessous supposent une taille racine de 16 px. |
| Les unités rem doivent rester adaptables à la configuration de l'application. |

### Structures design inspectées — DOCUMENTÉ_VISUEL, sources S4, S12

| Sous-contexte | Élément | Information / règle |
|---|---|---|
| Structures design inspectées — DOCUMENTÉ_VISUEL, sources S4, S12 | État inspecté | Default. |
| Structures design inspectées — DOCUMENTÉ_VISUEL, sources S4, S12 | Style inspecté | Default. |
| Structures design inspectées — DOCUMENTÉ_VISUEL, sources S4, S12 | Position racine des quatre exemples | X = 0 px ; Y = 0 px. |
| Structures design inspectées — DOCUMENTÉ_VISUEL, sources S4, S12 | Opacité racine des quatre exemples | 100 %. |
| Structures design inspectées — DOCUMENTÉ_VISUEL, sources S4, S12 | Fond racine des quatre exemples | #00008F. |
| Structures design inspectées — DOCUMENTÉ_VISUEL, sources S4, S12 / Prospect Desktop | Identité | Thème=Prospect, Device=--desk, Style=--default, State=--default. |
| Structures design inspectées — DOCUMENTÉ_VISUEL, sources S4, S12 / Prospect Desktop | Dimensions de l'exemplaire | 144 × 56 px. |
| Structures design inspectées — DOCUMENTÉ_VISUEL, sources S4, S12 / Prospect Desktop | Border radius déclaré | 100 px. |
| Structures design inspectées — DOCUMENTÉ_VISUEL, sources S4, S12 / Prospect Mobile | Identité | Thème=Prospect, Device=--mob, Style=--default, State=--default. |
| Structures design inspectées — DOCUMENTÉ_VISUEL, sources S4, S12 / Prospect Mobile | Dimensions de l'exemplaire | 144 × 56 px. |
| Structures design inspectées — DOCUMENTÉ_VISUEL, sources S4, S12 / Prospect Mobile | Border radius déclaré | 100 px. |
| Structures design inspectées — DOCUMENTÉ_VISUEL, sources S4, S12 / Client Desktop | Identité | Thème=Client, Device=--desk, Style=--default, State=--default. |
| Structures design inspectées — DOCUMENTÉ_VISUEL, sources S4, S12 / Client Desktop | Dimensions de l'exemplaire | 180 × 56 px. |
| Structures design inspectées — DOCUMENTÉ_VISUEL, sources S4, S12 / Client Desktop | Border radius déclaré | 8 px. |
| Structures design inspectées — DOCUMENTÉ_VISUEL, sources S4, S12 / Client Mobile | Identité | Thème=Client, Device=--mob, Style=--default, State=--default. |
| Structures design inspectées — DOCUMENTÉ_VISUEL, sources S4, S12 / Client Mobile | Dimensions de l'exemplaire | 125 × 56 px. |
| Structures design inspectées — DOCUMENTÉ_VISUEL, sources S4, S12 / Client Mobile | Border radius déclaré | 8 px. |
| Structures design inspectées — DOCUMENTÉ_VISUEL, sources S4, S12 / Token lié affiché pour Prospect Desktop, Prospect Mobile et Client Mobile | Précision | Actions.Button Primary.Primary.Default.background. |
| Structures design inspectées — DOCUMENTÉ_VISUEL, sources S4, S12 | Token lié Client Desktop | non affiché dans les attributs collectés. |
| Structures design inspectées — DOCUMENTÉ_VISUEL, sources S4, S12 | Portée | dimensions des exemplaires inspectés, pas une largeur fixe universelle à imposer à chaque libellé. |
| Structures design inspectées — DOCUMENTÉ_VISUEL, sources S4, S12 | Précision | Le statut « variables en cours de conception » reste présent malgré ces tokens liés. |
| Structures design inspectées — DOCUMENTÉ_VISUEL, sources S4, S12 | Précision | Le rayon design Prospect de 100 px diffère du token CSS publié de 32 px ; les deux donnent ici une silhouette pilule, mais ne sont pas des valeurs interchangeables à documenter comme identiques. |

### Structure CSS commune

| Élément | Information / règle |
|---|---|
| display | flex. |
| align-items | center. |
| justify-content | center. |
| gap | var(--rem-12), observé à 12 px. |
| border | 0. |
| border-radius | var(--radius-32), observé à 32 px. |
| font-family | var(--font-family-base). |
| Famille résolue | Source Sans Pro, arial, sans-serif. |
| font-weight | 600. |
| cursor | pointer. |
| user-select | none. |
| transition-duration | var(--transition-duration, .15s). |
| transition-timing-function | linear. |
| transition-property | width ; height ; border ; color ; background-color ; box-shadow. |
| Largeur fixe | aucune règle identifiée sur le bouton de base. |
| Largeur pleine automatique | non identifiée. |
| Hauteur CSS fixe | non identifiée ; la hauteur résulte du contenu et des espacements. |
| Hauteur observée pour un libellé sur une ligne | 56 px. |

### Mobile et tablette jusqu'à 1023 px inclus

| Élément | Information / règle |
|---|---|
| font-size | var(--rem-16), observé à 16 px. |
| line-height | var(--rem-24), observé à 24 px. |
| padding vertical | var(--rem-16), observé à 16 px. |
| padding horizontal | var(--rem-24), observé à 24 px. |

### Desktop au-delà de 1023 px

| Élément | Information / règle |
|---|---|
| Condition CSS exacte | screen and (width &gt; 1023px). |
| font-size | var(--rem-18), observé à 18 px. |
| line-height | var(--rem-32), observé à 32 px. |
| padding vertical | var(--rem-12), observé à 12 px. |
| padding horizontal | var(--rem-24), observé à 24 px. |

### Icônes

| Élément | Information / règle |
|---|---|
| SVG directement enfants du bouton | aspect-ratio 1 ; fill currentColor. |
| Taille observée des icônes des exemples | 24 × 24 px. |
| Taille imposée à tout contenu iconLeft ou iconRight par Button lui-même | NON_CONFIRMÉE. |
| RECOMMANDATION | utiliser les composants et dimensions d'icônes du design system. |

### Primary / Default

| Élément | Information / règle |
|---|---|
| Fond normal | --blue-1000 = #00008f. |
| Texte normal | --white-1000 = #ffffff. |
| Fond Hover | --blue-1200 = #000070. |
| Fond Active | --blue-900 = #1a1a99. |
| Couleur de l'indicateur Focus | --blue-650 = #5858b6. |
| Fond Disabled | --gray-050 = #f5f5f5. |
| Texte Disabled | --gray-500 = #999999. |

### Primary / Business

| Élément | Information / règle |
|---|---|
| Fond normal | --orange-1000 = #c84d14. |
| Texte normal | --white-1000 = #ffffff. |
| Fond Hover et Focus | --orange-1050 = #be4913. |
| Fond Active | --orange-800 = #d57244. |
| Couleur de l'indicateur Focus | héritée de la règle commune, --blue-650. |

### Primary / Inverse

| Élément | Information / règle |
|---|---|
| Fond normal | --white-1000 = #ffffff. |
| Texte normal | --blue-1000 = #00008f. |
| Fond Hover et Focus | --gray-140 = #e3e3e3. |
| Texte Hover et Focus | --blue-1200 = #000070. |
| Fond Active | --gray-050 = #f5f5f5. |
| Texte Active | --blue-900 = #1a1a99. |
| Couleur de l'indicateur Focus | héritée de la règle commune, --blue-650. |

### Indicateur de focus commun

| Élément | Information / règle |
|---|---|
| focus | couleur d'outline transparente hors focus-visible. |
| focus-visible | outline de 2 px, solid, var(--button-outline-color). |
| outline-offset | 3 px. |
| Précision | Ne pas supprimer ou dégrader cet indicateur. |

### Précautions de cascade

| Information / règle |
|---|
| Les règles de variante sont déclarées après certaines règles communes d'état. |
| Les combinaisons Disabled avec Business ou Inverse ne sont pas déduites des seuls tokens Disabled communs. |
| Leur rendu exact doit être vérifié avant de figer une spécification de ces combinaisons. |
| Les tokens relevés sont ceux du CSS publié, pas des variables Figma officiellement finalisées. |

<a id="section-11"></a>

## 11. ACCESSIBILITÉ ET SÉMANTIQUE

DOCUMENTÉ — Source : S6.

### Navigation

| Information / règle |
|---|
| Atteindre les boutons activables par Tab. |
| Activer par Entrée ou Espace. |
| Respecter un ordre de tabulation séquentiel et logique. |
| Préserver l'indicateur de focus. |
| Regrouper les éléments associés lorsque cela aide leur compréhension. |

### Nom accessible

| Élément | Information / règle |
|---|---|
| Précision | Utiliser aria-label si l'intitulé n'est pas explicite. |
| Précision | Utiliser aria-labelledby lorsqu'une description textuelle externe est nécessaire, notamment dans le cas évoqué d'icônes sans texte visible. |
| RECOMMANDATION | privilégier un libellé visible explicite et conserver ce libellé dans le nom accessible. |
| RECOMMANDATION | aria-labelledby doit référencer un identifiant d'élément existant. |
| RECOMMANDATION | un attribut ARIA ne garantit pas l'affichage d'une infobulle au survol. |

### Icônes

| Élément | Information / règle |
|---|---|
| RECOMMANDATION | si l'icône est décorative, la masquer aux technologies d'assistance pour éviter une annonce redondante. |
| RECOMMANDATION | ne pas utiliser un Button Primary sans texte par défaut ; faire valider ce cas particulier. |
| Précision | La mention documentaire d'icônes sans texte ne définit pas à elle seule une variante icon-only de Button Primary. |

### Désactivation

| Information / règle |
|---|
| Un bouton avec disabled natif n'est normalement pas atteint par Tab. |
| La consigne de navigation clavier concerne les boutons activables. |
| Vérifier les annonces du Spinner dans les états Disabled et Loading. |

### Contrastes

| Élément | Information / règle |
|---|---|
| Précision | Couleurs relevées dans la section 10. |
| Rapport de contraste et conformité WCAG/RGAA | NON_CERTIFIÉS par cette note. |
| RECOMMANDATION | vérifier les contrastes du texte, des icônes et du focus sur le fond réel, pour chaque variante et état. |

### Ressource citée par Zeroheight

| Information / règle |
|---|
| https://www.w3.org/WAI/ARIA/apg/patterns/button/ |

<a id="section-12"></a>

## 12. SCÉNARIOS D'INTÉGRATION TEXTUELS

### Scénario : action primaire générale.

| Élément | Information / règle |
|---|---|
| Composant | Button. |
| variant | primary. |
| children | libellé d'action explicite. |
| type | button, sauf soumission intentionnelle d'un formulaire. |
| onClick | gestionnaire applicatif de l'action. |

### Scénario : soumission d'un formulaire.

| Élément | Information / règle |
|---|---|
| Composant | Button. |
| variant | primary ou primary-business selon la finalité. |
| type | submit explicite. |
| Traitement | gérer la soumission dans le formulaire. |
| RECOMMANDATION | éviter de déclencher deux fois l'action via onClick et le traitement de soumission. |

### Scénario : action business.

| Élément | Information / règle |
|---|---|
| Composant | Button. |
| variant | primary-business. |
| Exemple de libellé | Obtenir un tarif. |

### Scénario : action primaire sur fond adapté à Inverse.

| Élément | Information / règle |
|---|---|
| Composant | Button. |
| variant | primary-inverse. |
| Validation préalable | fond foncé ; contraste du texte, des icônes et du focus sur ce fond. |

### Scénario : action avec illustration.

| Élément | Information / règle |
|---|---|
| iconLeft ou iconRight | une seule icône. |
| children | texte conservé. |
| Validation | cohérence entre l'icône et l'action. |

### Scénario : opération asynchrone.

| Élément | Information / règle |
|---|---|
| loading | piloté par l'application pendant l'opération, sous réserve de disponibilité dans les types installés. |
| Effet publié | bouton désactivé ; texte conservé ; Spinner affiché. |
| Fin d'opération | rétablir l'état approprié et présenter le résultat ou l'erreur dans l'interface. |

<a id="section-13"></a>

## 13. CONSIGNES D'EXPLOITATION POUR UNE IA

### RECOMMANDATIONS issues des données précédentes

| Information / règle |
|---|
| Réutiliser Button du point d'entrée Prospect plutôt que recréer son HTML et son CSS. |
| Ne pas mélanger les API Client et Prospect. |
| Choisir une variante primaire officielle. |
| Déterminer la sémantique action ou navigation avant de sélectionner Button ou Link. |
| Ne pas inventer de propriétés size, device, state, fullWidth, iconPosition ou href pour Button. |
| Vérifier les déclarations TypeScript de la version utilisée avant de générer des appels. |
| Respecter une seule icône et un contenu centré. |
| Respecter un libellé d'une ligne, de 35 caractères maximum et de 5 mots maximum. |
| Appliquer les règles de casse, de ponctuation et de pronoms. |
| Appliquer les libellés spécifiques au début et à la navigation d'un parcours à étapes. |
| Dans une modale, reprendre le vocabulaire du titre et préciser l'action de confirmation. |
| Ne pas proposer « Oui » seul ni un libellé générique interdit comme modèle. |
| Limiter la page à un seul Button Primary au maximum, tous styles primaires confondus, même dans des sections distinctes. |
| Ne pas confondre variant primary, utilisé par défaut, et l'état d'interaction Default. |
| Laisser CSS gérer Hover, Active, Focus et l'adaptation Desktop/Mobile. |
| Ne pas interpréter le contrôle Storybook des icônes comme une définition de leur type. |
| Ne pas supposer que disabled ne montre pas de Spinner. |
| Ne pas utiliser aria-disabled seul comme désactivation fonctionnelle complète. |
| Ne pas supprimer le focus visible. |
| Ne pas inventer une valeur manquante ; conserver NON_CONFIRMÉ ou demander la donnée. |
| Réexaminer les sources après changement de version ou publication de latest. |

<a id="section-14"></a>

## 14. CHECKLIST DE VALIDATION

### Contrôles à appliquer à une intégration

| Information / règle |
|---|
| Le package, l'univers Prospect et la version sont identifiés. |
| Les propriétés utilisées existent dans les types de la version installée. |
| L'action principale et le style choisi sont cohérents. |
| La sémantique button ou link correspond au résultat de l'activation. |
| Le type button ou submit est intentionnel. |
| Le libellé décrit l'action, tient sur une ligne et respecte les limites. |
| Les exceptions de libellé sont limitées au contexte documenté. |
| Dans un parcours à étapes, les actions Commencer, Précédent et Suivant correspondent à leur rôle. |
| Dans une modale, le libellé reprend le terme du titre et rend la confirmation explicite. |
| Les pronoms respectent le contexte Espace Client ou institutionnel autorisé. |
| Les contradictions de source pertinentes sont signalées, pas résolues arbitrairement. |
| Il n'y a qu'une icône au maximum. |
| Le contenu est centré. |
| La page contient au maximum un Button Primary, tous styles primaires confondus. |
| Les rendus Mobile et Desktop restent conformes. |
| Tab, Entrée et Espace fonctionnent pour un bouton activable. |
| Le focus reste visible et non masqué par le conteneur. |
| Le nom accessible est explicite. |
| Les icônes décoratives ne produisent pas d'annonce redondante. |
| Disabled et Loading empêchent l'action. |
| Les annonces du Spinner correspondent au contexte. |
| aria-disabled seul ne laisse pas une action clavier involontaire. |
| Les contrastes sont vérifiés sur les fonds réellement utilisés. |
| Une action asynchrone gère ses erreurs et ne peut pas être soumise involontairement plusieurs fois. |

### Vérifications réalisées pour cette note

| Élément | Information / règle |
|---|---|
| Lecture des six onglets documentaires | réalisée. |
| Lecture de la documentation et des exemples Storybook | réalisée. |
| Inspection du comportement publié de Button et Spinner | réalisée. |
| Extraction des règles CSS et des tokens pertinents | réalisée. |
| Mesure du Primary à 1023 px et 1024 px | réalisée. |
| Mesure du Hover et du Focus visible Primary | réalisée. |
| Observation du Disabled Primary avec Spinner | réalisée. |
| Observation des icônes d'exemple à 24 × 24 px | réalisée. |
| Lecture visuelle des six schémas d'anatomie et de variantes | réalisée. |
| Lecture des illustrations Do/Don't/Caution de Guidelines et Contenu | réalisée. |
| Inspection racine des quatre structures Desktop/Mobile Prospect/Client | réalisée. |
| Intégration des règles spécifiques aux parcours à étapes et aux modales | réalisée. |

### Vérifications non réalisées

| Information / règle |
|---|
| Compilation TypeScript d'une intégration applicative. |
| Audit complet avec lecteur d'écran. |
| Certification des contrastes ou de la conformité WCAG/RGAA. |
| Audit des combinaisons d'états Business et Inverse. |
| Inspection exhaustive des sous-calques Figma et des détails des captures applicatives. |
| Vérification de l'implémentation Client. |

<a id="section-15"></a>

## 15. CONTRADICTIONS ET PRIORITÉ DES RÈGLES

### Principe

| Information / règle |
|---|
| Les règles de design définissent ce qui est autorisé. |
| Le JavaScript décrit ce qui est actuellement possible, pas ce qu'il faut systématiquement faire. |
| Les observations ne remplacent pas les contraintes d'usage. |
| Une illustration de variantes ou une capture de contexte ne crée pas d'exception implicite. |

### C01 — Nombre de Primary

| Élément | Information / règle |
|---|---|
| Texte Zeroheight | interdit deux Primary l'un à côté de l'autre. |
| Illustration | contre-exemple de deux Primary empilés. |
| Clarification utilisateur | au maximum un Primary pour toute la page. |
| Décision à appliquer | cardinalité maximale de 1, sans exception pour des sections distinctes ou pour un changement de style. |

### C02 — Pronoms dans les légendes

| Élément | Information / règle |
|---|---|
| Précision | Une illustration légendée comme interdiction de « je » montre « Payer votre cotisation ». |
| Précision | Une illustration légendée comme interdiction de « votre » montre « Je paye ma cotisation ». |
| Décision à appliquer | conserver les deux interdictions ; ne pas tirer une règle opposée de ces légendes inversées. |

### C03 — Ponctuation en modale

| Élément | Information / règle |
|---|---|
| Règle textuelle générale | pas de ponctuation. |
| Illustration contextuelle | « Oui, quitter » et « Non, reprendre ». |
| Décision à appliquer — CLARIFICATION_UTILISATEUR, 2026-10-06 | Autoriser la virgule uniquement dans les confirmations de type « Oui, quitter » ; conserver l'interdiction générale de ponctuation dans les autres contextes. |
| Statut de l'arbitrage | Résolu par l'utilisateur ; ne pas présenter cette exception comme une question encore ouverte. |
| Précision | Le principe certain reste de préciser l'action plutôt que présenter « Oui » seul. |

### C04 — Construction grammaticale

| Élément | Information / règle |
|---|---|
| Règle générale | infinitif + complément. |
| Exceptions documentées | infinitif seul pour les actions habituelles ; Commencer ; Précédent ; Suivant ; confirmation Oui + verbe. |
| Décision à appliquer | accepter ces exceptions dans leurs contextes uniquement. |

### C05 — Rayons design et code

| Élément | Information / règle |
|---|---|
| Prospect, inspecteur design | 100 px. |
| Prospect, CSS publié | 32 px. |
| Client, inspecteur design | 8 px. |
| Décision à appliquer — CLARIFICATION_UTILISATEUR | Le Storybook fait référence : retenir son rayon pour l'implémentation de l'univers concerné, sans fusionner les valeurs. L'implémentation Client n'a pas été examinée ; ne pas lui appliquer le rayon Prospect par défaut. |

### C06 — États Disabled

| Élément | Information / règle |
|---|---|
| Schéma des cinq états | Disabled avec indicateur circulaire. |
| Exemple Do des états de base | Disabled sans indicateur visible. |
| JavaScript Prospect publié | disabled ou loading ajoute le Spinner. |
| Décision à appliquer | Clarification utilisateur : l'indicateur de chargement est réservé à l'état Disabled. Conserver les différences entre illustrations et le fait technique que loading rend le bouton disabled ; ne pas en déduire que l'indicateur est permis dans un état activable. |

### C07 — Plusieurs icônes

| Élément | Information / règle |
|---|---|
| API | peut rendre iconLeft et iconRight ensemble. |
| Design | interdit plusieurs icônes. |
| Décision à appliquer | jamais les deux propriétés d'icône à la fois. |

### C08 — Limites du libellé et dimensions

| Élément | Information / règle |
|---|---|
| API | n'impose pas les 35 caractères, les 5 mots ou la ligne unique. |
| Design | impose ces limites et interdit de modifier la taille. |
| Décision à appliquer | valider et raccourcir le contenu ; ne pas contourner les contraintes par du CSS personnalisé. |

<a id="section-16"></a>

## 16. COUVERTURE DE LA DESCRIPTION ET CONTRÔLES POUR L'IA

### Design

| Élément | Information / règle |
|---|---|
| Description et objectif | section 03. |
| Anatomie et différences Client/Prospect | section 04. |
| Device, Style, State et Icon | section 05. |
| Visuels de tailles, styles, états et positions | sections 04, 05, 09, 10. |

### Guidelines d'utilisation

| Élément | Information / règle |
|---|---|
| Action principale, modales critiques et validation en fin de parcours | section 03. |
| Choix Default ou Business | section 05. |
| Toutes les règles Do/Don't de composition, d'état initial, de longueur, de hiérarchie, de taille et de casse | sections 06, 07. |
| Exemples illustrés « Trouver une agence » et « Payer en ligne » | actions explicites ; sections 03, 07. |
| Captures Prospect/Client | exemples de formulaire, d'action de parcours, de modale et d'espace client ; ne pas déduire de leurs autres composants de nouvelles règles pour Button Primary. |

### Styles

| Élément | Information / règle |
|---|---|
| Quatre structures inspectables et liens Figma | sections 02, 10. |
| Variables en cours de conception | sections 01, 10. |
| Différence entre valeurs design et tokens CSS | sections 10, 15. |

### Contenu

| Élément | Information / règle |
|---|---|
| Construction, pronoms, contextualisation et contre-exemples | section 07. |
| Parcours à étapes | Commencer ; Précédent ; Suivant, section 07. |
| Modales | cohérence avec le titre et confirmation explicite, section 07. |
| Incohérences de légendes et de ponctuation | section 15. |

### Accessibilité

| Élément | Information / règle |
|---|---|
| Sémantique, nom accessible, clavier, focus, ordre de tabulation et regroupement | section 11. |
| Précision | Un regroupement logique peut être annoncé comme une liste de deux éléments ; cela n'autorise pas deux Primary. |
| Ressource WAI-ARIA | section 11. |

### Code

| Élément | Information / règle |
|---|---|
| Distinction Prospect/Client dans l'onglet | section 02. |
| Storybook embarqué accessible | React Prospect. |
| Contrat et comportements de ce déploiement | sections 08, 09. |
| Précision | Ne pas inventer une API React Client à partir des seules illustrations Client. |

### Cas de contrôle obligatoires

| Élément | Information / règle |
|---|---|
| Deux Primary dans des sections séparées | NON_CONFORME. |
| Un Default et un Business sur la même page | NON_CONFORME. |
| Deux Primary empilés | NON_CONFORME. |
| Un Primary accompagné d'un Secondary | conforme à la règle de hiérarchie, sous réserve des autres contraintes. |
| Une icône de chaque côté du texte | NON_CONFORME. |
| Un libellé de 36 caractères ou de 6 mots | NON_CONFORME. |
| Un libellé qui respecte les limites mais passe sur deux lignes | NON_CONFORME ; réécrire le libellé. |
| « Suivant » dans un parcours à étapes | exception contextuelle documentée. |
| « Oui » seul dans une modale de confirmation | NON_CONFORME. |
| Un libellé d'action qui change le terme utilisé dans le titre de la modale | NON_CONFORME. |
| « Payer ma cotisation » sans renvoi vers l'Espace Client | NON_CONFORME. |
| Une possibilité offerte par l'API mais interdite par les guidelines | NON_CONFORME. |

<a id="section-17"></a>

## 17. Compléments utilisateur et tokens JSON Figma

### Provenance et portée

| Élément | Donnée de référence |
|---|---|
| S13 — Source | JSON Figma transmis par l'utilisateur, pièce jointe « Pasted text #1 », le 6 octobre 2026. |
| Priorité — CLARIFICATION_UTILISATEUR, 2026-10-06 | Le Storybook est toujours la référence ; le JSON est complémentaire et comparatif. En cas d'écart visuel, retenir le Storybook de l'univers et de la version concernés. |
| Nature | Export de tokens de plusieurs composants ; ce n'est pas un arbre de calques Figma. |
| Périmètre extrait | `thèmes.prospect.button_primary` et `thèmes.client.button_primary` : 82 valeurs, toutes consignées ci-dessous. |
| Autres composants | Exclus des tokens Button Primary. Les tokens `spinner` ne prouvent pas à eux seuls quelle variante est liée au bouton. |
| Valeurs globales | Les cinq tokens au niveau racine de chaque thème correspondent aux cinq dimensions communes du Button Primary ci-dessous ; la racine nomme l'espacement `spacing`, le composant le nomme `gap`. |
| Typographie et icônes | Taille, famille, graisse, interligne et taille d'icône ne sont pas définis dans les branches `button_primary`. Ne pas les inventer à partir du JSON. |
| Responsive | Branche commune nommée `-commun-dm` ; aucune branche séparée Desktop/Mobile dans les tokens Button Primary fournis. |
| Focus | Aucun token d'état Focus dans ces branches. L'état reste documenté dans Zeroheight ; conserver séparément les observations du CSS. |
| Version | Version de publication et date d'export du JSON non indiquées ; ne pas supposer qu'il correspond exactement au déploiement `latest` consulté. |
| Règle d'usage Inverse | Sur fond foncé, pour mettre en avant l'action primaire et préserver son accessibilité ; contrôler les contrastes réels. |
| Règle d'usage chargement | Indicateur visible uniquement dans l'état Disabled. Le code observé peut atteindre cet état par `disabled` ou `loading`. |

### Dimensions communes du Button Primary

Chaque ligne correspond à `thèmes.<thème>.button_primary.-commun-dm.<token>.value`.

| Token JSON exact | Prospect | Client |
|---|---|---|
| `radius` | 100px | 8px |
| `padding-top-&-bottom` | 12px | 16px |
| `padding-left-&-right` | 24px | 16px |
| `height` | 56px | 56px |
| `gap` | 12px | 12px |

### Couleurs par thème, style et état

Chaque cellule correspond à `thèmes.<thème>.button_primary.<style>.<état>.<background/icon/text>.value`. La clé JSON `primary` correspond au style design Default ; `business` à Business ; `inverse` à Inverse.

| Thème | Style JSON | État JSON | Fond (`background`) | Icône (`icon`) | Texte (`text`) |
|---|---|---|---|---|---|
| prospect | primary | default | #00008f | #ffffff | #ffffff |
| prospect | primary | hover | #000072 | #ffffff | #ffffff |
| prospect | primary | active | #3333a5 | #ffffff | #ffffff |
| prospect | primary | disabled | #f5f5f5 | #999999 | #999999 |
| prospect | business | default | #c94e14 | #ffffff | #ffffff |
| prospect | business | hover | #bf4a13 | #ffffff | #ffffff |
| prospect | business | active | #d47143 | #ffffff | #ffffff |
| prospect | business | disabled | #f5f5f5 | #999999 | #999999 |
| prospect | inverse | default | #ffffff | #00008f | #00008f |
| prospect | inverse | hover | #e2e2e2 | #000072 | #000072 |
| prospect | inverse | active | #f5f5f5 | #3333a5 | #3333a5 |
| prospect | inverse | disabled | #f5f5f5 | #999999 | #999999 |
| client | primary | default | #00008f | #ffffff | #ffffff |
| client | primary | hover | #000072 | #ffffff | #ffffff |
| client | primary | active | #3333a5 | #ffffff | #ffffff |
| client | primary | disabled | #f5f5f5 | #999999 | #999999 |
| client | business | default | #c94e14 | #ffffff | #ffffff |
| client | business | hover | #bf4a13 | #ffffff | #ffffff |
| client | business | active | #d47143 | #ffffff | #ffffff |
| client | business | disabled | #f5f5f5 | #999999 | #999999 |
| client | inverse | default | #ffffff | #00008f | #00008f |
| client | inverse | hover | #ffffff | #00008f | #00008f |
| client | inverse | active | #ffffff | #3333a5 | #3333a5 |
| client | inverse | disabled | #f5f5f5 | #999999 | #999999 |

### Écarts à conserver entre design JSON et implémentation inspectée

| Sujet | JSON fourni | CSS Prospect précédemment inspecté | Consigne |
|---|---|---|---|
| Rayon Prospect | 100px | 32px | Conserver les deux provenances ; ne pas déclarer les valeurs identiques. |
| Padding vertical Prospect | 12px dans la branche commune | 12px Desktop ; 16px jusqu'à 1023px | Ne pas inventer une branche Mobile dans le JSON ni effacer le responsive du code. |
| Primary Hover | #000072 | #000070 | Écart réel de tokens. |
| Primary Active | #3333a5 | #1a1a99 | Écart réel de tokens. |
| Business Default / Hover / Active | #c94e14 / #bf4a13 / #d47143 | #c84d14 / #be4913 / #d57244 | Écarts réels de tokens. |
| Inverse Hover Prospect | Fond #e2e2e2 ; texte/icône #000072 | Fond #e3e3e3 ; texte #000070 | Ne pas appliquer les couleurs du déploiement comme si elles provenaient du JSON. |
| Inverse Active Prospect | Fond #f5f5f5 ; texte/icône #3333a5 | Fond #f5f5f5 ; texte #1a1a99 | Conserver l'écart de couleur. |
| Inverse Client | Hover blanc/bleu ; Active blanc/#3333a5 | Implémentation Client non examinée | Ne pas copier les états Prospect vers Client. |
| Disabled des trois styles | Fond #f5f5f5 ; texte/icône #999999 pour les deux thèmes | Cascade Business/Inverse non auditée | Les tokens design sont désormais connus ; le rendu réel reste à vérifier. |

Règle de résolution — CLARIFICATION_UTILISATEUR : pour tous les écarts de ce tableau, le Storybook fait autorité. Si le rendu pertinent n'a pas été examiné, consulter le Storybook de l'univers concerné ; ne pas substituer une valeur JSON ni extrapoler celle d'un autre univers.

### Complétude après réception du JSON

| Acquis | Encore non confirmé |
|---|---|
| 82 valeurs Button Primary ; dimensions, fonds, textes et icônes pour deux thèmes, trois styles et quatre états | Sous-calques, auto-layout, contraintes de redimensionnement, typographie et taille d'icône Figma. |
| Règle Inverse sur fond foncé ; indicateur de chargement réservé à Disabled ; virgule autorisée uniquement dans les confirmations | Types React exacts, implémentation Client, version stable et certification d'accessibilité. |

FIN_DE_NOTE
