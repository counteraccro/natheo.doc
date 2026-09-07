---
title: "Description générale"
parent: "Projet"
nav_order: 1
---

Le projet Nathéo CMS est un CMS développé en [PHP/HTML/CSS/JS][1].
Ce projet n'a aucune prétention de vouloir faire mieux que les CMS actuels du marché, il est parti d'une idée simple : ai-je les connaissances techniques pour réaliser un CMS en PHP ?

> 🚧 Le projet est actuellement en développement actif sur sa **V2** (refonte de
> l'interface d'administration, migration Vite/Vue 3/TypeScript/Tailwind 4). Cette
> page décrit l'organisation générale du CMS ; le détail fonctionnel de chaque écran
> est en cours de réécriture dans le [Guide d'administration](../GuideAdmin/index.md).

Le CMS est découpé en 2 parties distinctes :
- L'Administration
- Le Front

### Administration
Cette partie du CMS permet de pouvoir gérer le paramétrage du CMS, son contenu ainsi que les membres et administrateurs.

Liste des fonctionnalités disponibles, voir le [Guide d'administration](../GuideAdmin/index.md) :
* Mon compte et mes notifications
* Content
  * Tags
  * Médiathèque
  * Pages
  * Commentaires
  * Menus
  * FAQ
* Système
  * Gestion des utilisateurs et des rôles
  * Options système
  * Sidebar de l'administration
  * Traductions
  * Emails
  * Logs
  * Jetons d'accès API
* Outils
  * Options avancées (réinitialisation du CMS, mode debug)
  * Gestionnaire SQL (exécuter des requêtes SQL simples depuis une interface web)
  * Gestionnaire base de données (vision de la base et sauvegardes)

### Front
Partie encore peu développée du CMS, voir la page [Front](../Front/index.md).

[1]: ../Architecture/stack_technique.md
