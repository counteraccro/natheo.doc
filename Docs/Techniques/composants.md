# Composants utilisés

[Index](../../index.md) > [Documentation technique](index.md) > Composants

Cette page regroupe l'ensemble de la liste des composants/services/fixtures/Extension défini pour le projet.

### Services
- [AdminAppService](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/AppAdminService.php) 
  - Service global hérité de tous les services pour l'administration
- [SidebarElementService](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/System/SidebarElementService.php) 
  - Service regroupant le code fonctionnel pour l'entité [SidebarElementService](https://github.com/counteraccro/natheo/blob/master/src/Entity/Admin/System/SidebarElement.php)
- [OptionSystemService](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/System/OptionSystemService.php)
  - Service regroupant le code fonctionnel pour l'entité [OptionSystem](https://github.com/counteraccro/natheo/blob/master/src/Entity/Admin/System/OptionSystem.php)
- [GridService](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/GridService.php) 
  - Service regroupant les méthodes pour l'aide à la génération d'un tableau grid générique
- [UserService](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/System/User/UserService.php) 
  - Service regroupant le code fonctionnel pour l'entité [User](https://github.com/counteraccro/natheo/blob/master/src/Entity/Admin/System/User.php)
- [Userdata](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/System/User/UserDataService.php)
  - Service regroupant les données rattachés au user
- [OptionUserService](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/OptionUserService.php) 
  - Service regroupant lregroupant le code fonctionnel pour l'entité [OptionUser](https://github.com/counteraccro/natheo/blob/master/src/Entity/Admin/OptionUser.php)
- [LoggerService](https://github.com/counteraccro/natheo/blob/master/src/Service/LoggerService.php) 
  - Service qui permet la centralisation, l'enregistrement, la lecture et suppression des logs de l'application
- [TranslateService](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/System/TranslateService.php)
  - Service qui permet de gérer les données lié aux traductions
- [CommandeService](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/CommandService.php)
  - Service qui regroupe l'ensemble des commandes executable via la console
- [MarkdownEditorService](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/MarkdownEditorService.php)
  - Service qui regroupe les traductions pour l'éditeur Markdown
- [Mailservice](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/System/MailService.php)
  - Service qui regroupe l'ensemble des fonctions liées aux emails
- [NotificationService](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/NotificationService.php)
  - Service qui regroupe l'ensemble des fonctions liées aux notifications
- [TagService](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/Content/TagService.php)
  - Service qui regroupe l'ensemble des fonctions liées aux Tags
- [MediaFolderService](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/Content/Media/MediaFolderService.php)
  - Service qui gère l'ensemble des fonctions liées au dossier de la médiathèque
- [PageService](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/Content/Page/PageService.php)
  - Service qui gère l'ensemble des fonctions liées à la gestion des pages

### Extension Twig
- [SidebarExtension](https://github.com/counteraccro/natheo/blob/master/src/Twig/Runtime/Admin/SidebarExtensionRuntime.php) 
  - Permet de générer le menu sidebar de l'administration
- [BreadcrumbExtension](https://github.com/counteraccro/natheo/blob/master/src/Twig/Runtime/Admin/BreadcrumbExtensionRuntime.php) 
  - Permet de générer de façon automatique les fils d'Ariane
- [OptionExtension](https://github.com/counteraccro/natheo/blob/master/src/Twig/Runtime/Admin/OptionExtensionRuntime.php)
  - Permet de générer les options système du CMS 
  - Permet de générer les options système du user 
  - Permet de récupérer une option système en fonction de sa clé
- [OptionUserExtension](https://github.com/counteraccro/natheo/blob/master/src/Twig/Runtime/Admin/OptionUserExtensionRuntime.php)
  - Extension qui permet de récupérer une option user en fonction de sa clé
- [EncoreEntryCssSourceExtensionRuntime](https://github.com/counteraccro/natheo/blob/master/src/Twig/Runtime/EncoreEntryCssSourceExtensionRuntime.php)
  - Transforme du CSS compilé par Webpack en ressource pour le convertir en inline
- [DateExtensionRuntime](https://github.com/counteraccro/natheo/blob/master/src/Twig/Runtime/DateExtensionRuntime.php)
  - Affichage la différence entre 2 dates sous la forme "il y'a x annee x mois x jours x heures x minutes x secondes"


### EventSubscriber
- [LocaleSubscriber](https://github.com/counteraccro/natheo/blob/master/src/EventSubscriber/LocaleSubscriber.php) 
  - Appelé avant chaque action du Controller pour setter la langue défini dans l'option user

### EventListener
- [DatabaseActivitySubscriber](https://github.com/counteraccro/natheo/blob/master/src/EventListener/DatabaseActivitySubscriber.php)
  - Appelé après chaque commit /update /delete d'une entité

### Fixtures
- [SidebarElementFixtures](https://github.com/counteraccro/natheo/blob/master/src/DataFixtures/Admin/System/SidebarElementFixtures.php)
  - Génère le jeu de données pour la sidebar de l'administration
- [OptionSystemFixtures](https://github.com/counteraccro/natheo/blob/master/src/DataFixtures/Admin/System/OptionSystemFixtures.php)
  - Génère le jeu de données des options systèmes
- [UserFixtures](https://github.com/counteraccro/natheo/blob/master/src/DataFixtures/Admin/System/UserFixtures.php) 
  - Génère le jeu de données pour les users et options_users
- [MailFixtures](https://github.com/counteraccro/natheo/blob/master/src/DataFixtures/Admin/System/MailFixtures.php)
  - Génère le jeu de données pour les emails
- [TagFixtures](https://github.com/counteraccro/natheo/blob/master/src/DataFixtures/Admin/Content/TagFixtures.php)
  - Génère le jeu de données pour les tags

#### Groupe de fixtures existant
- devTools 
  - Ensemble des fixtures liées au développement pur
  - Données insérées
    - sidebarElement
- system 
  - Ensemble des fixtures liées au Système du CMS
  - Données insérées 
    - option_system
- Registered 
  - Ensemble des fixtures liées au membres / users
  - Données insérées
    - user
    - option_user
- content
  - Ensemble des fixtures liées au contenu du site
  - Données insérées
    - tag
    - media
- mail
  - Ensemble des fixtures liées au contenu du mail du site
  - Données insérées
    - mail