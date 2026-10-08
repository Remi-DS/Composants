# Composants — Design System Canopée

Base de connaissance destinée à l'exploitation par une IA de conception d'interfaces.
Chaque fiche décrit un composant Canopée : rôle, intégration React, propriétés, variantes,
états, anatomie, styles et tokens, écarts entre univers, accessibilité et limites.

## Principe de fiabilité

Chaque information est qualifiée par un statut, afin de ne jamais confondre une capacité
technique avec une règle de design :

| Statut | Signification |
|---|---|
| `DOCUMENTÉ` | Règle écrite explicitement dans la documentation d'une story (MDX). |
| `IMPLÉMENTÉ` | Comportement constaté dans le code React ou CSS publié. |
| `OBSERVÉ` | Valeur relevée dans le CSS ou les tokens. |
| `RECOMMANDATION` | Consigne d'intégration proposée par la fiche, et non une règle Canopée. |
| `NON_CONFIRMÉ` | Information absente des sources accessibles. |

Règles appliquées à l'ensemble des fiches :

- aucune règle de design n'est inventée ni déduite d'une possibilité de l'API ;
- les règles d'usage design (quand utiliser, cardinalité par page, microcopy) ne figurent pas
  dans les sources techniques publiques : elles sont marquées `NON_CONFIRMÉ` et relèvent du
  Zeroheight de l'univers concerné ;
- les contradictions entre sources ou entre univers sont conservées et signalées, jamais
  arbitrées.

## Sources

| Source | Portée |
|---|---|
| Dépôt `AxaFrance/design-system` | Code React (`packages/canopee-react`), CSS et tokens (`packages/canopee-css`), stories et MDX (`apps/apollo-stories` pour Prospect, `apps/look-and-feel-stories` pour Client). |
| Storybook Prospect | https://axafrance.github.io/design-system/prospect/react/latest/ |
| Storybook Client | https://axafrance.github.io/design-system/client/react/latest/ |
| Zeroheight | Règles d'usage design ; non accessibles publiquement, d'où les mentions `NON_CONFIRMÉ`. |

Collecte des sources techniques : 8 octobre 2026. Les sources sont en version `latest`, donc
mutables : revérifier le Storybook de la version installée après une mise à jour.

## Button Primary

La documentation du Button Primary suit un format antérieur, issu de Zeroheight et du Storybook :

- [Documentation complète](./button-primary.md)
- [Synthèse](./button-primary-synthese.md)

## Composants

`✅` indique que le composant est exporté par le point d'entrée de l'univers
(`@axa-fr/canopee-react/prospect` ou `@axa-fr/canopee-react/client`).

| Composant | Fiche | Prospect | Client |
|---|---|---|---|
| `Accordion` | [accordion-synthese.md](./accordion-synthese.md) | ✅ | ✅ |
| `AccordionContextual` | [accordion-contextual-synthese.md](./accordion-contextual-synthese.md) | ✅ | ✅ |
| `AccordionCore` | [accordion-core-synthese.md](./accordion-core-synthese.md) | ✅ | ✅ |
| `AppName` | [app-name-synthese.md](./app-name-synthese.md) | ✅ | ✅ |
| `BasePicture` | [base-picture-synthese.md](./base-picture-synthese.md) | ✅ | ✅ |
| `Card` | [card-synthese.md](./card-synthese.md) | ✅ | ✅ |
| `CardCheckbox` | [card-checkbox-synthese.md](./card-checkbox-synthese.md) | ✅ | ✅ |
| `CardCheckboxOption` | [card-checkbox-option-synthese.md](./card-checkbox-option-synthese.md) | ✅ | ✅ |
| `CardMessage` | [card-message-synthese.md](./card-message-synthese.md) | ✅ | ✅ |
| `CardRadio` | [card-radio-synthese.md](./card-radio-synthese.md) | ✅ | ✅ |
| `CardRadioGroup` | [card-radio-group-synthese.md](./card-radio-group-synthese.md) | ✅ | ✅ |
| `Checkbox` | [checkbox-synthese.md](./checkbox-synthese.md) | ✅ | ✅ |
| `CheckboxText` | [checkbox-text-synthese.md](./checkbox-text-synthese.md) | ✅ | ✅ |
| `ClickIcon` | [click-icon-synthese.md](./click-icon-synthese.md) | ✅ | ✅ |
| `ClickItem` | [click-item-synthese.md](./click-item-synthese.md) | ✅ | ✅ |
| `ContentItemDuo` | [content-item-duo-synthese.md](./content-item-duo-synthese.md) | ✅ | ✅ |
| `ContentItemDuoAction` | [content-item-duo-action-synthese.md](./content-item-duo-action-synthese.md) | ✅ | ✅ |
| `ContentItemMono` | [content-item-mono-synthese.md](./content-item-mono-synthese.md) | ✅ | ✅ |
| `DataAgent` | [data-agent-synthese.md](./data-agent-synthese.md) | ✅ | ✅ |
| `Divider` | [divider-synthese.md](./divider-synthese.md) | ✅ | ✅ |
| `Dropdown` | [dropdown-synthese.md](./dropdown-synthese.md) | ✅ | ✅ |
| `Fieldset` | [fieldset-synthese.md](./fieldset-synthese.md) | ✅ | ✅ |
| `FileUpload` | [file-upload-synthese.md](./file-upload-synthese.md) | ✅ | ✅ |
| `Footer` | [footer-synthese.md](./footer-synthese.md) | ✅ | ✅ |
| `Header` | [header-synthese.md](./header-synthese.md) | ✅ | ✅ |
| `Heading` | [heading-synthese.md](./heading-synthese.md) | ✅ | ✅ |
| `Icon` | [icon-synthese.md](./icon-synthese.md) | ✅ | ✅ |
| `InputDate` | [input-date-synthese.md](./input-date-synthese.md) | ✅ | ✅ |
| `InputFile` | [input-file-synthese.md](./input-file-synthese.md) | ✅ | ✅ |
| `InputPhone` | [input-phone-synthese.md](./input-phone-synthese.md) | ✅ | ✅ |
| `InputText` | [input-text-synthese.md](./input-text-synthese.md) | ✅ | ✅ |
| `ItemFile` | [item-file-synthese.md](./item-file-synthese.md) | ✅ | ✅ |
| `ItemLabel` | [item-label-synthese.md](./item-label-synthese.md) | ✅ | ✅ |
| `ItemMenu` | [item-menu-synthese.md](./item-menu-synthese.md) | ✅ | ✅ |
| `ItemMessage` | [item-message-synthese.md](./item-message-synthese.md) | ✅ | ✅ |
| `ItemMultiSelect` | [item-multi-select-synthese.md](./item-multi-select-synthese.md) | ✅ | ✅ |
| `ItemTabBar` | [item-tab-bar-synthese.md](./item-tab-bar-synthese.md) | ✅ | ✅ |
| `LevelSelector` | [level-selector-synthese.md](./level-selector-synthese.md) | ✅ | ✅ |
| `Link` | [link-synthese.md](./link-synthese.md) | ✅ | ✅ |
| `List` | [list-synthese.md](./list-synthese.md) | ✅ | ✅ |
| `Loader` | [loader-synthese.md](./loader-synthese.md) | ✅ | ✅ |
| `MenuBurger` | [menu-burger-synthese.md](./menu-burger-synthese.md) | ✅ | ✅ |
| `Message` | [message-synthese.md](./message-synthese.md) | ✅ | ✅ |
| `MessageBar` | [message-bar-synthese.md](./message-bar-synthese.md) | ✅ | ✅ |
| `Modal` | [modal-synthese.md](./modal-synthese.md) | ✅ | ✅ |
| `MultiMessage` | [multi-message-synthese.md](./multi-message-synthese.md) | ✅ | ✅ |
| `MultiSelectList` | [multi-select-list-synthese.md](./multi-select-list-synthese.md) | ✅ | ✅ |
| `Pagination` | [pagination-synthese.md](./pagination-synthese.md) | ✅ | ✅ |
| `ProgressBar` | [progress-bar-synthese.md](./progress-bar-synthese.md) | ✅ | ✅ |
| `ProgressBarGroup` | [progress-bar-group-synthese.md](./progress-bar-group-synthese.md) | ✅ | ✅ |
| `Radio` | [radio-synthese.md](./radio-synthese.md) | ✅ | ✅ |
| `RadioText` | [radio-text-synthese.md](./radio-text-synthese.md) | ✅ | ✅ |
| `Skeleton` | [skeleton-synthese.md](./skeleton-synthese.md) | ✅ | ✅ |
| `SkeletonGrid` | [skeleton-grid-synthese.md](./skeleton-grid-synthese.md) | ✅ | ✅ |
| `SkeletonList` | [skeleton-list-synthese.md](./skeleton-list-synthese.md) | ✅ | ✅ |
| `Spinner` | [spinner-synthese.md](./spinner-synthese.md) | ✅ | ✅ |
| `Stepper` | [stepper-synthese.md](./stepper-synthese.md) | ✅ | ✅ |
| `Svg` | [svg-synthese.md](./svg-synthese.md) | ✅ | ✅ |
| `TabBar` | [tab-bar-synthese.md](./tab-bar-synthese.md) | ✅ | ✅ |
| `TabMenu` | [tab-menu-synthese.md](./tab-menu-synthese.md) | ✅ | ✅ |
| `Table` | [table-synthese.md](./table-synthese.md) | ✅ | ✅ |
| `TableMobileCard` | [table-mobile-card-synthese.md](./table-mobile-card-synthese.md) | ✅ | ✅ |
| `Tag` | [tag-synthese.md](./tag-synthese.md) | ✅ | ✅ |
| `TagList` | [tag-list-synthese.md](./tag-list-synthese.md) | ✅ | ✅ |
| `TextArea` | [text-area-synthese.md](./text-area-synthese.md) | ✅ | ✅ |
| `TimelineVertical` | [timeline-vertical-synthese.md](./timeline-vertical-synthese.md) | ✅ | ✅ |
| `Toggle` | [toggle-synthese.md](./toggle-synthese.md) | ✅ | ✅ |

## Limites de cette base

- Les règles d'usage, la microcopy et la cardinalité par composant restent à compléter depuis
  Zeroheight ; elles sont aujourd'hui majoritairement `NON_CONFIRMÉ`.
- Les fiches ne certifient aucune conformité WCAG ou RGAA : elles listent uniquement les
  mécanismes d'accessibilité réellement présents dans le code.
- Les valeurs de style proviennent du CSS source et peuvent évoluer d'une version à l'autre.
