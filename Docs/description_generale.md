## Description générale du projet

[Index](../index.md) > Description générale

le projet Natheo CMS est un CMS développé en [PHP/HTML/CSS/JS][1]. 
Ce projet n'a aucune prétention de vouloir faire mieux que les CMS actuels du marché, il est partie d'une idée simple, ai-je les connaissances techniques pour réaliser un CMS en PHP

Le CMS est découpé en 2 parties distinctes :
- L'Administration
- Le Front

### Administration
Cette partie du CMS permet de pouvoir gérer le paramétrage du CMS, son contenu ainsi que les membres et administrateurs

Liste des fonctionnalités disponibles
* [Mon profil](Fonctionnelles/Administration/mon_profil.md)
  * Gestion des données personnelles de l'utilisateur connecté
* [Mes notifications](Fonctionnelles/Administration/Global/notifications.md)
  * Gestion des notifications reçues par l'utilisateur
* Content
  * [Tags](Fonctionnelles/Administration/Content/Tag/tag.md)
    * Gestion des tags
  * [Médiathèque](Fonctionnelles/Administration/Content/Mediateque/mediatheque.md)
    * La médiathèque
  * [Pages](Fonctionnelles/Administration/Content/Page/page.md)
    * Gestion des pages du CMS
  * [Commentaires](Fonctionnelles/Administration/Content/Comment/comment.md)
    * Gestion des commentaires
  * [Menu](Fonctionnelles/Administration/Content/Menu/menu.md)
    * Gestion des menus du CMS
  * [FAQ](Fonctionnelles/Administration/Content/Faq/faq.md)
    * Gestion des Faqs
* Système
  * Fonctionnalités liées au fonctionnement du CMS
  * [Gestion des logs](Fonctionnelles/Administration/System/log.md)
    * Permet de consulter / supprimer les fichiers de logs de l'application
  * [Gestion des utilisateurs](Fonctionnelles/Administration/System/users/user.md) 
    * Permet de gérer (créer / éditer / supprimer) les personnes qui peuvent ce connecter à l'administration et participer l'ajout de contenu pour le CMS
  * [Options](Fonctionnelles/Administration/System/options_system.md)
    * Permet de modifier les options du CMS
  * [Sidebar](Fonctionnelles/Administration/System/sidebar.md)
    * Permet de gérer le menu 'Sidebar' de l'administration
  * [Traductions](Fonctionnelles/Administration/System/translation.md)
    * Permet de pouvoir éditer en ligne l'ensemble des traductions du site
  * [Mail](Fonctionnelles/Administration/System/mail.md)
    * Permet de pouvoir personnaliser le contenu des emails envoyés par le CMS
  * [Log](Fonctionnelles/Administration/System/log.md)
    * Permet de pouvoir consulter les logs du CMS
  * [Gestion des token](Fonctionnelles/Administration/System/ApiToken/apiToken.md)
    * Permet de pouvoir gérer les tokens qui donne accès aux API du CMS
* Tools
  * Options avancées
    * Permet de réinitialiser le CMS ou gérer le mode débug
  * Gestionnaire SQL
    * Permet d'exécuter des requêtes SQL simple directement depuis une interface web
  * Gestionnaire base de données
    * Permet d'avoir une vision de la base de données et de faire des sauvegardes de celle-ci

### Front
*TODO A faire*



[1]: Techniques/description.md