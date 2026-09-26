---
title: "Composants"
parent: "Architecture"
has_children: true
nav_order: 10
---

Cette page regroupe l'ensemble des composants, services, extensions Twig,
listeners et fixtures définis dans le projet.

## Composants documentés

Certains composants transverses, plus riches, ont leur propre page dédiée :

- [Tableau GRID](grid.md) — composant générique de tableau paginé, trié et
  filtrable, utilisé par tous les listings de l'administration
- [Éditeur Markdown](editeur_markdown.md) — éditeur riche en Vue 3
- [Notification](notification.md) — comment déclencher une notification
  depuis le code (voir aussi le [Guide d'administration](../../GuideAdmin/MonCompte/Notifications/technique.md)
  pour la référence technique complète du module)

## Services

### Socle applicatif

Chaque contexte (admin, API, front) repose sur un service global et une
classe *Handler* qui charge en autowire les services nécessaires à ce
contexte, pour éviter d'injecter des dizaines de dépendances dans chaque
contrôleur.

| Service | Rôle |
|---|---|
| [`AppService`](https://github.com/counteraccro/natheo/blob/master/src/Service/AppService.php) | Service global, socle commun à toute l'application |
| [`AppAdminService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/AppAdminService.php) | Service global pour l'administration |
| [`AppAdminHandlerService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/AppAdminHandlerService.php) | Charge en autowire les services nécessaires côté admin |
| [`AppApiService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Api/AppApiService.php) | Service global pour les API |
| [`AppApiHandlerService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Api/AppApiHandlerService.php) | Charge en autowire les services nécessaires pour l'API front |
| [`AppFrontService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Front/AppFrontService.php) | Service global pour le front |
| [`AppFrontHandlerService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Front/AppFrontHandlerService.php) | Charge en autowire les services nécessaires pour le front |

### Utilitaires globaux

| Service | Rôle |
|---|---|
| [`LoggerService`](https://github.com/counteraccro/natheo/blob/master/src/Service/LoggerService.php) | Centralise l'enregistrement, la lecture et la suppression des logs de l'application |
| [`SecurityService`](https://github.com/counteraccro/natheo/blob/master/src/Service/SecurityService.php) | Gère les actions liées à la sécurité |
| [`DateService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Global/DateService.php) | Manipulation de dates et de leur format |

### Installation

| Service | Rôle |
|---|---|
| [`InstallationService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Installation/InstallationService.php) | Gère l'installation du site |
| [`InstallRequirementsChecker`](https://github.com/counteraccro/natheo/blob/master/src/Service/Installation/InstallRequirementsChecker.php) | Vérifie les prérequis avant installation |

### Admin — Contenu

| Service | Rôle |
|---|---|
| [`TagService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/Content/Tag/TagService.php) | Gère les tags de l'application — voir [référence technique](../../GuideAdmin/Contenu/Tags/technique.md) |
| [`PageService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/Content/Page/PageService.php) | Gère la création et l'édition des pages |
| [`MenuService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/Content/Menu/MenuService.php) | Gère les menus |
| [`FaqService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/Content/Faq/FaqService.php) | Gère la FAQ |
| [`CommentService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/Content/Comment/CommentService.php) | Gère les commentaires |
| [`MediaService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/Content/Media/MediaService.php) | Gère les médias |
| [`MediaFolderService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/Content/Media/MediaFolderService.php) | Gère les dossiers de la médiathèque |

### Admin — Système

| Service | Rôle |
|---|---|
| [`UserService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/System/User/UserService.php) | Gère l'entité `User` |
| [`UserDataService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/System/User/UserDataService.php) | Gère les données rattachées à l'utilisateur |
| [`OptionSystemService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/System/OptionSystemService.php) | Gère les options système |
| [`OptionUserService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/System/OptionUserService.php) | Gère les options utilisateur |
| [`SidebarElementService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/System/SidebarElementService.php) | Gère l'entité `SidebarElement` |
| [`TranslateService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/System/TranslateService.php) | Traitement des données liées aux traductions |
| [`MailService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/System/MailService.php) | Gère l'entité `Mail` et l'envoi d'emails |
| [`ApiTokenService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/System/ApiTokenService.php) | Gère l'entité `ApiToken` |
| [`InformationService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/System/InformationService.php) | Informations sur le CMS |

### Admin — Outils

| Service | Rôle |
|---|---|
| [`CommandService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/CommandService.php) | Exécute une commande console depuis l'administration |
| [`DatabaseManagerService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/Tools/DatabaseManagerService.php) | Support du gestionnaire de base de données (dumps SQL) |
| [`SqlManagerService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/Tools/SqlManagerService.php) | Support du SQL Manager |
| [`GitService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/Dev/GitService.php) | Informations Git, utilisées en développement |

### Admin — Autres

| Service | Rôle |
|---|---|
| [`DashboardService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/DashboardService.php) | Alimente le tableau de bord |
| [`GridService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/GridService.php) | Génère les données du [Tableau GRID](grid.md) |
| [`MarkdownEditorService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/MarkdownEditorService.php) | Traductions pour l'[éditeur Markdown](editeur_markdown.md) |
| [`NotificationService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/NotificationService.php) | Gère les [notifications](notification.md) |
| [`GlobalSearchService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/GlobalSearchService.php) | Alimente la recherche globale de l'administration |
| [`StatisticsService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/StatisticsService.php) | Statistiques de l'application |

### API

| Service | Rôle |
|---|---|
| [`ApiService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Api/ApiService.php) | Socle commun aux services API |
| [`ApiCommentService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Api/Content/ApiCommentService.php) | Commentaires via l'API |
| [`ApiMenuService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Api/Content/ApiMenuService.php) | Menus via l'API |
| [`ApiPageService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Api/Content/Page/ApiPageService.php) | Pages via l'API |
| [`ApiPageContentService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Api/Content/Page/ApiPageContentService.php) | Formate le contenu (`pageContents`) d'une page pour l'API |
| [`ApiSitemapService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Api/Global/ApiSitemapService.php) | Génère la sitemap via l'API |
| [`ApiOptionSystemService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Api/System/ApiOptionSystemService.php) | Options système via l'API |
| [`ApiUserService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Api/System/User/ApiUserService.php) | Utilisateurs via l'API |

### Front

| Service | Rôle |
|---|---|
| [`OptionSystemFrontService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Front/OptionSystemFrontService.php) | Options système côté front |

## Extensions Twig

Depuis la V2, les fonctions Twig sont déclarées directement sur la classe
d'extension via l'attribut `#[AsTwigFunction]` (Symfony 7+), sans classe
`*Runtime` séparée. `AppAdminExtension` et `AppExtension` sont les classes de
base (chargement des services communs) dont héritent les autres extensions
admin.

| Extension | Rôle |
|---|---|
| [`AppAdminExtension`](https://github.com/counteraccro/natheo/blob/master/src/Twig/Extension/Admin/AppAdminExtension.php) | Classe globale pour les extensions Twig admin |
| [`AppExtension`](https://github.com/counteraccro/natheo/blob/master/src/Twig/Extension/AppExtension.php) | Classe globale pour les extensions Twig |
| [`BreadcrumbExtension`](https://github.com/counteraccro/natheo/blob/master/src/Twig/Extension/Admin/BreadcrumbExtension.php) | Génère les fils d'Ariane (`breadcrumb()`) |
| [`SidebarExtension`](https://github.com/counteraccro/natheo/blob/master/src/Twig/Extension/Admin/System/SidebarExtension.php) | Génère le menu sidebar de l'administration |
| [`OptionExtension`](https://github.com/counteraccro/natheo/blob/master/src/Twig/Extension/Admin/System/OptionExtension.php) | Génère le formulaire de saisie des options système et utilisateur |
| [`OptionUserExtension`](https://github.com/counteraccro/natheo/blob/master/src/Twig/Extension/Admin/System/OptionUserExtension.php) | Génère le formulaire de saisie des options utilisateur |
| [`DevExtension`](https://github.com/counteraccro/natheo/blob/master/src/Twig/Extension/Admin/DevExtension.php) | Informations utiles en développement |
| [`DateExtension`](https://github.com/counteraccro/natheo/blob/master/src/Twig/Extension/DateExtension.php) | Manipulation des dates dans les templates |
| [`ViteEntryCssSourceExtension`](https://github.com/counteraccro/natheo/blob/master/src/Twig/Extension/ViteEntryCssSourceExtension.php) | Convertit en inline le CSS compilé par Vite (remplace l'ancien `EncoreEntryCssSourceExtension`, Webpack Encore ayant été abandonné au profit de Vite) |

### Composant Twig

| Composant | Rôle |
|---|---|
| [`ModaleComponent`](https://github.com/counteraccro/natheo/blob/master/src/Twig/Components/Admin/ModaleComponent.php) | Génère une modale Bootstrap 5.3 (`TwigComponent`) |

## EventSubscriber / EventListener

| Classe | Rôle |
|---|---|
| [`LocaleSubscriber`](https://github.com/counteraccro/natheo/blob/master/src/EventSubscriber/LocaleSubscriber.php) | Force la locale en fonction des options de l'utilisateur |
| [`TwigGlobalListener`](https://github.com/counteraccro/natheo/blob/master/src/EventListener/TwigGlobalListener.php) | Expose certaines clés d'options système/utilisateur comme globales Twig (`os_notification`, `os_site_name`…) |
| [`DatabaseActivityListener`](https://github.com/counteraccro/natheo/blob/master/src/EventListener/DatabaseActivityListener.php) | Intercepte les évènements Doctrine (création/mise à jour/suppression d'entité) |
| [`DatabaseTablePrefixListener`](https://github.com/counteraccro/natheo/blob/master/src/EventListener/DatabaseTablePrefixListener.php) | Ajoute un préfixe et un schéma aux tables de la base de données |
| [`ExceptionListener`](https://github.com/counteraccro/natheo/blob/master/src/EventListener/ExceptionListener.php) | Intercepte les exceptions non gérées |
| [`OverwriteListener`](https://github.com/counteraccro/natheo/blob/master/src/EventListener/OverwriteListener.php) | Surcharge un contrôleur en fonction de ce qui est défini dans `overwrite.yaml` — voir [Surcharge de controller](../surcharge_controller.md) |

## Fixtures

Chaque fixture hérite de [`AppFixtures`](https://github.com/counteraccro/natheo/blob/master/src/DataFixtures/AppFixtures.php),
lit son jeu de données depuis un fichier YAML dans `src/DataFixtures/data/`,
et déclare un ou plusieurs groupes (`FixtureGroupInterface`) permettant de
charger uniquement un sous-ensemble de données.

| Fixture | Entité(s) | Fichier de données | Groupes |
|---|---|---|---|
| [`UserFixtures`](https://github.com/counteraccro/natheo/blob/master/src/DataFixtures/Admin/System/UserFixtures.php) | `User`, `OptionUser` | `system/user_fixtures_data.yaml` (ou `user_demo_fixtures_data.yaml` en environnement de démo) | `registered`, `user` |
| [`OptionSystemFixtures`](https://github.com/counteraccro/natheo/blob/master/src/DataFixtures/Admin/System/OptionSystemFixtures.php) | `OptionSystem` | `system/option_system_fixtures_data.yaml` | `system`, `option_system`, `registered` |
| [`SidebarElementFixtures`](https://github.com/counteraccro/natheo/blob/master/src/DataFixtures/Admin/System/SidebarElementFixtures.php) | `SidebarElement` | `system/sidebar_element_fixtures_data.yaml` | `devTools`, `sidebarElement` |
| [`MailFixtures`](https://github.com/counteraccro/natheo/blob/master/src/DataFixtures/Admin/System/MailFixtures.php) | `Mail` | `system/mail_fixtures_data.yaml` | `mail`, `system` |
| [`ApiTokenFixture`](https://github.com/counteraccro/natheo/blob/master/src/DataFixtures/Admin/System/ApiTokenFixture.php) | `ApiToken` | `system/api_token_fixtures_data.yaml` | `system`, `api_token` |
| [`TagFixtures`](https://github.com/counteraccro/natheo/blob/master/src/DataFixtures/Admin/Content/TagFixtures.php) | `Tag`, `TagTranslation` | `content/tag_fixtures_data.yaml` | `tag`, `content` |
| [`FaqFixtures`](https://github.com/counteraccro/natheo/blob/master/src/DataFixtures/Admin/Content/Faq/FaqFixtures.php) | `Faq` | `content/faq/faq_fixtures_data.yaml` | `faq`, `content` |
| [`MediaFixtures`](https://github.com/counteraccro/natheo/blob/master/src/DataFixtures/Admin/Content/Media/MediaFixtures.php) | `Media` | `content/media/media_fixtures_data.yaml` | `media`, `content` |
| [`MediaFolderFixtures`](https://github.com/counteraccro/natheo/blob/master/src/DataFixtures/Admin/Content/Media/MediaFolderFixtures.php) | `MediaFolder` | `content/media/media_folder_fixtures_data.yaml` | `media`, `content` |
| [`MenuFixtures`](https://github.com/counteraccro/natheo/blob/master/src/DataFixtures/Admin/Content/Menu/MenuFixtures.php) | `Menu` | `content/menu/menu_fixtures_data.yaml` | `menu`, `content` |
| [`PageFixtures`](https://github.com/counteraccro/natheo/blob/master/src/DataFixtures/Admin/Content/Page/PageFixtures.php) | `Page` | `content/page/page_fixtures_data.yaml` | `page`, `content` |
| [`PageMenuFixtures`](https://github.com/counteraccro/natheo/blob/master/src/DataFixtures/Admin/Content/Page/PageMenuFixtures.php) | Rattache les pages aux menus | `content/page/page_menu_fixtures_data.yaml` | `page`, `menu`, `content` |
| [`CommentFixtures`](https://github.com/counteraccro/natheo/blob/master/src/DataFixtures/Admin/Content/Comment/CommentFixtures.php) | `Comment` | `content/comment/comment_fixtures_data.yaml` | `page`, `content`, `comment` |
| [`SqlManagerFixtures`](https://github.com/counteraccro/natheo/blob/master/src/DataFixtures/Admin/Tools/SqlManagerFixtures.php) | `SqlManager` | `tools/sql_manager_fixtures_data.yaml` | `tools`, `sql_manager` |

Charger un groupe précis :

```bash
php bin/console doctrine:fixtures:load --group=tag
```

### Groupes existants

| Groupe | Fixtures concernées |
|---|---|
| `registered` | `UserFixtures`, `OptionSystemFixtures` |
| `user` | `UserFixtures` |
| `system` | `OptionSystemFixtures`, `MailFixtures`, `ApiTokenFixture` |
| `option_system` | `OptionSystemFixtures` |
| `devTools` | `SidebarElementFixtures` |
| `sidebarElement` | `SidebarElementFixtures` |
| `mail` | `MailFixtures` |
| `api_token` | `ApiTokenFixture` |
| `content` | `TagFixtures`, `FaqFixtures`, `MediaFixtures`, `MediaFolderFixtures`, `MenuFixtures`, `PageFixtures`, `PageMenuFixtures`, `CommentFixtures` |
| `tag` | `TagFixtures` |
| `faq` | `FaqFixtures` |
| `media` | `MediaFixtures`, `MediaFolderFixtures` |
| `menu` | `MenuFixtures`, `PageMenuFixtures` |
| `page` | `PageFixtures`, `PageMenuFixtures`, `CommentFixtures` |
| `comment` | `CommentFixtures` |
| `tools` | `SqlManagerFixtures` |
| `sql_manager` | `SqlManagerFixtures` |
