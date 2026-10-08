# Canopée — Descriptions des composants sur Zeroheight

Sources consultées le 8 octobre 2026 :
- [Index Client](https://zeroheight.com/49b6215d6/v/latest/p/817fe8-client)
- [Index Prospect](https://zeroheight.com/49b6215d6/v/latest/p/39afc9-prospects)

## Portée du relevé

57 entrées de composants recensées dans les deux index, regroupées ci-dessous. Les descriptions sont des reformulations courtes des introductions des pages, et non des descriptions déduites du nom des composants.

`DOCUMENTÉ` signifie ici **écrit dans Zeroheight**, pas constaté dans le code. Les liens Client et Prospect restent distincts même lorsque leur description est équivalente. Ce relevé ne vérifie ni les API React, ni les styles publiés, ni la conformité d'accessibilité.

Les introductions et les pages d'index ont été consultées. Les onglets Guidelines d'utilisation, Interaction et animation, Style, Contenu, Accessibilité et Code ne sont pas couverts par ce relevé. Les illustrations ne sont pas interprétées comme des règles supplémentaires.

Pour alimenter le gabarit de synthèse, ces informations peuvent compléter **Rôle et intention** et **Différences Prospect / Client**. Les autres sections nécessitent leurs propres sources. Une capacité technique n'est pas une permission de design. L'absence d'une règle dans ce relevé ne signifie pas son absence dans les autres onglets de Zeroheight.

## Atomes

| Composant | Description reformulée | Statut | Client | Prospect |
|---|---|---|---|---|
| Base Picture | Représentation visuelle identifiant un interlocuteur AXA : conseiller, marque ou autre interlocuteur. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/336fbe) | [Source](https://zeroheight.com/49b6215d6/p/90fcb0) |
| Checkbox | Sélection ou désélection d'un ou plusieurs choix. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/569b16) | [Source](https://zeroheight.com/49b6215d6/p/249028) |
| Divider | Séparation visuelle des sections pour structurer le contenu de l'interface. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/768aae) | [Source](https://zeroheight.com/49b6215d6/p/78bfac) |
| Icon | Complément d'information visuel associé à un élément, interactif ou non. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/637589) | [Source](https://zeroheight.com/49b6215d6/p/50cb72) |
| Item Card Message | Message visuel informatif ou contextuel destiné à prévenir l'utilisateur. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/462f07) | [Source](https://zeroheight.com/49b6215d6/p/4535fc) |
| Item File | Présentation des fichiers téléchargés par l'utilisateur et possibilité de les supprimer, selon le vocabulaire des pages. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/04744a) | [Source](https://zeroheight.com/49b6215d6/p/2172c1) |
| Item Label | Texte explicitant l'action attendue dans un champ de saisie ou un élément interactif associé. L'index Client le nomme « Label ». | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/0954f8) | [Source](https://zeroheight.com/49b6215d6/p/676cc2) |
| Item Message | Information de statut liée à une action, pour guider ou alerter l'utilisateur. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/00259e) | [Source](https://zeroheight.com/49b6215d6/p/904df7) |
| Item Pagination | Prospect : navigation entre les pages d'une liste de contenus. Son introduction emploie le nom « Pagination ». Client : le lien de l'index mène à la page Pagination, sans description distincte d'Item Pagination confirmée. | DOCUMENTÉ (Prospect) / NON_CONFIRMÉ (Client distinct) | [Lien de l'index](https://zeroheight.com/49b6215d6/p/6365cc) | [Source](https://zeroheight.com/49b6215d6/p/63736e) |
| Item Tab Bar | Passage entre les sections d'une même page sans rechargement de l'ensemble du contenu. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/36f561) | [Source](https://zeroheight.com/49b6215d6/p/00ab68) |
| Item Menu | Navigation vers une catégorie ou une autre page, avec chargement d'une nouvelle page. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/413b6d) | [Source](https://zeroheight.com/49b6215d6/p/073df2) |
| Progress Bar | Indication visuelle de l'avancement d'une action en cours. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/51550d) | [Source](https://zeroheight.com/49b6215d6/p/733c53) |
| Toggle | Sélection d'un état activé ou désactivé, avec retour visuel immédiat pour une fonctionnalité ou un réglage. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/874205) | [Source](https://zeroheight.com/49b6215d6/p/3686a3) |
| Radio | Sélection d'un choix parmi plusieurs propositions. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/145cf6) | [Source](https://zeroheight.com/49b6215d6/p/45945a) |
| Tag | Information contextuelle précise rattachée à un groupe d'éléments de l'interface. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/454498) | [Source](https://zeroheight.com/49b6215d6/p/36d352) |
| Level Bar | Client : sélection d'un niveau sur une échelle graduée. Prospect : élément en forme de pilule associant sélection d'un niveau par clic et retour visuel par remplissage. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/3352bd) | [Source](https://zeroheight.com/49b6215d6/p/5013ce) |

## Actions

| Composant | Description reformulée | Statut | Client | Prospect |
|---|---|---|---|---|
| Button Primary | Proposition de l'action principale de l'interface. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/891165) | [Source](https://zeroheight.com/49b6215d6/p/921e28) |
| Button Secondary | Proposition d'une action alternative. Les deux introductions précisent qu'il accompagne toujours un Button Primary. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/4446ea) | [Source](https://zeroheight.com/49b6215d6/p/55b509) |
| Button Tertiary | Proposition d'une action mineure et optionnelle. Les deux introductions précisent qu'il accompagne toujours les Button Primary et Secondary. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/571bf9) | [Source](https://zeroheight.com/49b6215d6/p/14437f) |
| Card | Description non récupérable par le lien de l'index Client : redirection vers l'accueil du guide. Aucune entrée Card autonome relevée dans l'index Prospect consulté. | NON_CONFIRMÉ | [Lien de l'index](https://zeroheight.com/49b6215d6/p/99120a) | Non recensé dans l'index |
| Click Icon | Réalisation d'une action spécifique rapide. L'introduction Client le qualifie d'interactif ; celle de Prospect, de visuel. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/5317c7) | [Source](https://zeroheight.com/49b6215d6/p/667c1e) |
| Click Item | Une ou plusieurs actions rapides associées à un élément précis ; utilisable seul ou dans des listes, menus ou tableaux. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/645989) | [Source](https://zeroheight.com/49b6215d6/p/78a9de) |
| Ghost Button | Redirection vers un contenu complémentaire, avec une intention rapprochée de celle de Link. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/68bcdb) | [Source](https://zeroheight.com/49b6215d6/p/886a12) |
| Link | Accès à un contenu complémentaire : nouvelle page ou élément à télécharger. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/08eef9) | [Source](https://zeroheight.com/49b6215d6/p/72e373) |

## Contenu

| Composant | Description reformulée | Statut | Client | Prospect |
|---|---|---|---|---|
| Content Item Duo | Deux informations textuelles placées face à face ; l'élément de droite propose toujours des informations supplémentaires. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/5803c2) | [Source](https://zeroheight.com/49b6215d6/p/45f002) |
| Content Item Duo Action | Deux informations textuelles placées face à face ; l'élément de droite propose toujours une action liée à celui de gauche. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/73e31f) | [Source](https://zeroheight.com/49b6215d6/p/639056) |
| Content Item Mono | Informations textuelles structurées autour d'un même sujet, éventuellement accompagnées d'un visuel ou d'une icône. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/88ae3f) | [Source](https://zeroheight.com/49b6215d6/p/0543f3) |
| Data Agent | Informations textuelles et visuelles sur un interlocuteur AXA, présentées en carte ou en liste structurée : conseiller, partenaire ou distributeur. Les deux pages portent la mention « To come ». | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/30a4a8) | [Source](https://zeroheight.com/49b6215d6/p/14be8e) |
| Message | Transmission d'informations contextuelles importantes par un élément visuel, parfois interactif. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/48bfc2) | [Source](https://zeroheight.com/49b6215d6/p/91cf9d) |
| Multi Message | Regroupement de plusieurs informations contextuelles dans une même zone, à travers des éléments visuels et interactifs. Les deux titres affichent un pictogramme de chantier. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/187cea) | [Source](https://zeroheight.com/49b6215d6/p/19b6e6) |
| Message Bar | Communication de nouveautés ou d'offres, ou alerte sur l'arrivée d'un événement climatique. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/06e007) | [Source](https://zeroheight.com/49b6215d6/p/39b5b0) |

## Formulaires

| Composant | Description reformulée | Statut | Client | Prospect |
|---|---|---|---|---|
| Card Checkbox | Client : sélection d'une ou plusieurs options dans une carte interactive. Prospect : association d'une case à cocher et d'une carte visuelle pour sélectionner des options. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/840e54) | [Source](https://zeroheight.com/49b6215d6/p/325d29) |
| Card Checkbox Group | Groupe interactif associant Card et Item Message pour sélectionner un ou plusieurs éléments accompagnés de multiples informations. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/4906df) | [Source](https://zeroheight.com/49b6215d6/p/949e86) |
| Card Radio | Client : association de Card et Radio pour sélectionner une option dans un groupe avec des informations visuelles ou textuelles. Prospect : la description parle de « Card Radio Group », malgré le titre Card Radio ; elle évoque une option comportant plusieurs informations. Cette incohérence n'est pas corrigée par supposition. | DOCUMENTÉ ; correspondance de la description Prospect NON_CONFIRMÉ | [Source](https://zeroheight.com/49b6215d6/p/17d443) | [Source](https://zeroheight.com/49b6215d6/p/517213) |
| Card Radio Group | Sélection d'une option accompagnée de multiples informations ; Prospect précise que les choix sont présentés sous forme de cartes enrichies. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/57f362) | [Source](https://zeroheight.com/49b6215d6/p/24557f) |
| Checkbox Text | Sélection ou non d'une option. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/259ae4) | [Source](https://zeroheight.com/49b6215d6/p/2020e9) |
| Radio Text | Sélection d'une seule option parmi plusieurs choix. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/53a217) | [Source](https://zeroheight.com/49b6215d6/p/392b34) |
| Dropdown | Sélection d'une donnée dans une liste déroulante. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/587380) | [Source](https://zeroheight.com/49b6215d6/p/341ffe) |
| File Upload | Import de fichiers depuis l'appareil de l'utilisateur vers une application ou un serveur. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/403c7e) | [Source](https://zeroheight.com/49b6215d6/p/388d90) |
| Input Date | Saisie ou sélection d'une date dans un formulaire. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/10d436) | [Source](https://zeroheight.com/49b6215d6/p/6274aa) |
| Input Phone | Saisie d'un numéro de téléphone dans un formulaire, quel que soit le pays de l'utilisateur selon les introductions. Cette intention ne constitue pas une vérification technique des pays pris en charge. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/965868) | [Source](https://zeroheight.com/49b6215d6/p/67af00) |
| Input Text | Saisie de données alphabétiques dans un formulaire, selon les introductions. Aucune restriction technique de saisie n'est déduite de cette formulation. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/44ccac) | [Source](https://zeroheight.com/49b6215d6/p/52f510) |
| Input Text Area | Saisie d'une quantité de texte ; aucune limite chiffrée n'est indiquée dans les introductions consultées. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/74fca8) | [Source](https://zeroheight.com/49b6215d6/p/87cbba) |
| Level Selector | Choix d'un niveau par barres cliquables et boutons d'ajustement, et visualisation du niveau choisi. Les introductions citent les interfaces multi-étapes de configuration de protection ou de prise en charge. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/0551c7) | [Source](https://zeroheight.com/49b6215d6/p/31f310) |

## Navigation

| Composant | Description reformulée | Statut | Client | Prospect |
|---|---|---|---|---|
| Footer | Navigation de bas de page regroupant des liens hors navigation principale : mentions légales, cookies, accessibilité et réseaux sociaux. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/414ba3) | [Source](https://zeroheight.com/49b6215d6/p/52fb4d) |
| Header | Éléments interactifs placés en haut des pages pour conserver l'accès aux actions clés. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/52c806) | [Source](https://zeroheight.com/49b6215d6/p/55db4f) |
| Menu Burger | Commande compacte d'affichage ou de masquage du menu principal, notamment sur mobile lorsque l'espace est limité. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/20a3ed) | [Source](https://zeroheight.com/49b6215d6/p/1353db) |
| Pagination | Organisation et navigation dans un grand ensemble de contenus, notamment les listes, tableaux et galeries d'images. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/6365cc) | [Source](https://zeroheight.com/49b6215d6/p/695543) |

## Progression

| Composant | Description reformulée | Statut | Client | Prospect |
|---|---|---|---|---|
| Spinner | Indication visuelle qu'une action est en cours de chargement. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/68e689) | [Source](https://zeroheight.com/49b6215d6/p/21e870) |
| Stepper | Indication visuelle de l'avancement du parcours de l'utilisateur. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/80a36e) | [Source](https://zeroheight.com/49b6215d6/p/820dfa) |
| Timeline verticale | Présentation des étapes dans l'ordre chronologique, de haut en bas. Aucune entrée relevée dans l'index Client consulté. | DOCUMENTÉ (Prospect) | Non recensé dans l'index | [Source](https://zeroheight.com/49b6215d6/p/23f34a) |
| Loader | Indication visuelle qu'un élément de l'interface est en cours de chargement. Aucune entrée relevée dans l'index Client consulté. | DOCUMENTÉ (Prospect) | Non recensé dans l'index | [Source](https://zeroheight.com/49b6215d6/p/373642) |

## Structure

| Composant | Description reformulée | Statut | Client | Prospect |
|---|---|---|---|---|
| Accordion | Affichage ou masquage d'une section de contenu complémentaire. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/6500d2) | [Source](https://zeroheight.com/49b6215d6/p/49feb4) |
| Accordion Contextual | Affichage ou masquage d'un contenu complémentaire lié à un élément précis. L'index Client emploie « Accordion Contextuel ». | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/13e12a) | [Source](https://zeroheight.com/49b6215d6/p/32c1fc) |
| Heading | Hiérarchisation typographique de l'information et identification des sections et sous-sections ; les introductions citent H1, H2 et H3. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/48f870) | [Source](https://zeroheight.com/49b6215d6/p/3478ad) |
| Modal | Client : contenu contextuel superposé à l'interface, avec ou sans action immédiate. Prospect : contenu essentiel superposé à l'interface, avec une action immédiate. Les intentions restent distinctes. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/8724ad) | [Source](https://zeroheight.com/49b6215d6/p/6203e4) |
| Tab Bar | Ensemble de navigation permettant de changer de section au sein d'une page sans recharger tout le contenu. | DOCUMENTÉ | [Source](https://zeroheight.com/49b6215d6/p/3352de) | [Source](https://zeroheight.com/49b6215d6/p/24889d) |

## Réserves et compléments à demander

- **Card Client — NON_CONFIRMÉ :** le lien recensé redirige vers l'accueil. Un export JSON de la page Card, ou un lien corrigé, permettrait de compléter sa description.
- **Item Pagination Client — NON_CONFIRMÉ :** le lien est identique à celui de Pagination. Un export spécifique permettrait de déterminer s'il existe une documentation distincte.
- **Card Radio Prospect — NON_CONFIRMÉ :** l'introduction nomme Card Radio Group. Une clarification de la source est nécessaire ; un export identique à la page ne résoudrait pas, à lui seul, cette incohérence.
- **Data Agent et Multi Message :** les mentions « To come » et le pictogramme de chantier sont relevés comme éléments documentaires, sans conclusion sur la disponibilité dans les packages.
- **Univers non recensé :** signifie absent de l'index consulté, pas absent du code, ni interdit dans cet univers.
- **Règles d'usage complémentaires :** cardinalité par page, microcopy, interdictions et recommandations non relevées ici restent à confirmer dans les onglets concernés. Les obligations explicites présentes dans les introductions, notamment l'accompagnement des boutons Secondary et Tertiary, sont déjà `DOCUMENTÉ`.
- **Synthèses techniques complètes :** les sections React, propriétés, variantes implémentées, DOM, tokens, responsive et accessibilité doivent être renseignées depuis le dépôt et le Storybook, sans transformer ces descriptions Zeroheight en preuve d'implémentation.
