# Composants utilisés

[Index](../../index.md) > [Documentation technique](index.md) > Composants

Cette page regroupe l'ensemble de la liste des composants/services/fixtures/Extension défini pour le projet.

### Services
- [AdminAppService](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/AppAdminService.php) 
  - Service global hérité de tous les services pour l'administration
- [SidebarElementService](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/SidebarElementService.php) 
  - Service regroupant le code fonctionnel pour l'entité [SidebarElementService](https://github.com/counteraccro/natheo/blob/master/src/Entity/Admin/SidebarElement.php)
- [OptionSystemService](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/OptionSystemService.php)
  - Service regroupant le code fonctionnel pour l'entité [OptionSystem](https://github.com/counteraccro/natheo/blob/master/src/Entity/Admin/OptionSystem.php)
- [GridService](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/GridService.php) 
  - Service regroupant les méthodes pour l'aide à la génération d'un tableau grid générique
- [UserService](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/UserService.php) 
  - Service regroupant le code fonctionnel pour l'entité [User](https://github.com/counteraccro/natheo/blob/master/src/Entity/Admin/User.php)
- [OptionUserService](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/OptionUserService.php) 
  - Service regroupant lregroupant le code fonctionnel pour l'entité [OptionUser](https://github.com/counteraccro/natheo/blob/master/src/Entity/Admin/OptionUser.php)
- [LoggerService](https://github.com/counteraccro/natheo/blob/master/src/Service/LoggerService.php) 
  - Service qui permet la centralisation, l'enregistrement, la lecture et suppression des logs de l'application
- [TranslateService](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/TranslateService.php)
  - Service qui permet de gérer les données lié aux traductions
- [CommandeService](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/CommandService.php)
  - Service qui regroupe l'ensemble des commandes executable via la console
- [MarkdownEditorService](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/MarkdownEditorService.php)
  - Service qui regroupe les traductions pour l'éditeur Markdown

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


### EventSubscriber
- [LocaleSubscriber](https://github.com/counteraccro/natheo/blob/master/src/EventSubscriber/LocaleSubscriber.php) 
  - Appelé avant chaque action du Controller pour setter la langue défini dans l'option user

### EventListener
- [DatabaseActivitySubscriber](https://github.com/counteraccro/natheo/blob/master/src/EventListener/DatabaseActivitySubscriber.php)
  - Appelé après chaque commit /update /delete d'une entité

### Fixtures
- [SidebarElementFixtures](https://github.com/counteraccro/natheo/blob/master/src/DataFixtures/Admin/SidebarElementFixtures.php)
  - Génère le jeu de données pour la sidebar de l'administration
- [OptionSystemFixtures](https://github.com/counteraccro/natheo/blob/master/src/DataFixtures/Admin/OptionSystemFixtures.php)
  - Génère le jeu de données des options systèmes
- [UserFixtures](https://github.com/counteraccro/natheo/blob/master/src/DataFixtures/Admin/UserFixtures.php) 
  - Génère le jeu de données pour les users

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