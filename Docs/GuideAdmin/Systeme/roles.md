---
title: "Les rôles"
nav_icon: "M9 12.75 11.25 15 15 9.75m-3-7.036A11.959 11.959 0 0 1 3.598 6 11.99 11.99 0 0 0 3 9.749c0 5.592 3.824 10.29 9 11.623 5.176-1.332 9-6.03 9-11.622 0-1.31-.21-2.571-.598-3.751h-.152c-3.196 0-6.1-1.248-8.25-3.285Z"
parent: "Système"
grand_parent: "Guide d'administration"
nav_order: 2
---

Chaque compte de l'administration possède un **rôle** qui détermine les écrans et actions auxquels il a accès.
Nathéo propose 4 rôles, du moins au plus puissant, chaque rôle héritant de tous les droits du rôle inférieur.

Il n'existe **pas d'écran de gestion des rôles** : la liste des rôles et leur hiérarchie sont fixées dans le code.
Le rôle d'un compte se choisit dans le [formulaire d'ajout / modification d'un utilisateur](Utilisateurs/ajouter_editer.md).

## Les 4 rôles

| Rôle | Code Symfony | Description affichée dans l'admin | Usage réel dans la V2 |
|---|---|---|---|
| **Utilisateur** | `ROLE_USER` | Donne le droit de se connecter à l'application (rôle par défaut) | Tableau de bord, notifications, gestion de son propre compte |
| **Contributeur** | `ROLE_CONTRIBUTEUR` | Donne le droit d'ajouter du contenu sur le site | Accès à **tout le menu Contenu** (pages, menus, FAQ, tags, médiathèque, commentaires), à la recherche globale et au bouton *Nouveau* |
| **Administrateur** | `ROLE_ADMIN` | Donne le droit d'administrer les contenus du site (commentaires, membres…) | **Aucun droit supplémentaire** par rapport au Contributeur pour l'instant (voir ci-dessous) |
| **Super Administrateur** | `ROLE_SUPER_ADMIN` | Permet de gérer l'ensemble du CMS | Menus **Système** et **Outils développeur**, prise de contrôle d'un autre compte |

## Qui accède à quoi

| Écran / fonctionnalité | Utilisateur | Contributeur | Administrateur | Super Admin |
|---|:---:|:---:|:---:|:---:|
| Tableau de bord | ✅ | ✅ | ✅ | ✅ |
| Tableau de bord : blocs *Derniers commentaires*, *Pages les plus vues*, *Dernières pages* | — | ✅ | ✅ | ✅ |
| [Notifications](../MonCompte/Notifications/notifications.md) | ✅ | ✅ | ✅ | ✅ |
| [Mon compte / profil](../MonCompte/profil.md) | ✅ | ✅ | ✅ | ✅ |
| [Recherche globale](../RechercheGlobale/recherche_globale.md) et bouton *Nouveau* du header | — | ✅ | ✅ | ✅ |
| [Pages](../Contenu/Pages/listing.md), [Menus](../Contenu/Menus/listing.md), [FAQ](../Contenu/Faq/listing.md), [Tags](../Contenu/Tags/listing.md), [Médiathèque](../Contenu/Mediatheque/mediatheque.md) | — | ✅ | ✅ | ✅ |
| [Commentaires](../Contenu/Commentaires/listing.md) (y compris modération) | — | ✅ | ✅ | ✅ |
| [Éditeur Markdown](../Modules/editeur_markdown.md) | — | ✅ | ✅ | ✅ |
| Système : [utilisateurs](Utilisateurs/listing.md), [sidebar](sidebar.md), [options](options.md), [jetons API](ApiToken/jetons_api.md), [emails](mail.md), [traductions](traductions.md), [logs](logs.md), informations | — | — | — | ✅ |
| Outils développeur : [options avancées](../Outils/options_avancees.md), [gestionnaire SQL](../Outils/gestionnaire_sql.md), [gestionnaire BDD](../Outils/gestionnaire_bdd.md), éléments HTML | — | — | — | ✅ |
| Se connecter en tant qu'un autre utilisateur | — | — | — | ✅ |

> 📝 Un Contributeur a accès à **tous** les contenus, pas seulement aux siens : le filtre « mes pages / mes
> commentaires… » des listings est une simple aide à l'affichage, pas une restriction de droits. Il peut donc
> modifier ou supprimer une page créée par un autre compte.

## Particularités à connaître

- **Chaque compte possède au minimum le rôle Utilisateur.** C'est pourquoi la colonne *Rôle* du
  [listing des utilisateurs](Utilisateurs/listing.md) affiche par exemple « Administrateur, Utilisateur ».
- **Le compte fondateur** (créé à l'installation) est toujours Super Administrateur et bénéficie de protections
  que n'ont pas les autres Super Administrateurs : personne d'autre ne peut le modifier, le désactiver, le
  supprimer ou prendre son contrôle. Voir [Gestion des utilisateurs](Utilisateurs/listing.md).
- **N'importe quel Super Administrateur peut en créer un autre** ou promouvoir un compte existant à ce rôle, sans
  confirmation particulière.
- **Un compte anonymisé** est automatiquement ramené au rôle Utilisateur.
- **La sidebar filtre ses entrées selon le rôle** : chaque élément porte un rôle minimal (visible dans la colonne
  *Rôle* de la [gestion de la sidebar](sidebar.md)) et n'est affiché que si l'utilisateur connecté le possède. Ce
  rôle n'est pas modifiable depuis l'administration.

## Les rôles côté site et API

Les rôles ont aussi un effet en dehors de l'administration :

- **Connexion d'un utilisateur via l'API** (`POST /api/{version}/authentication/user`) : refusée pour un compte n'ayant que
  le rôle Utilisateur. Il faut être au moins Contributeur pour obtenir un jeton utilisateur.
- **Pages en brouillon** : un utilisateur authentifié au moins Contributeur peut consulter via l'API les pages en
  brouillon, en plus des pages publiées.
- **Commentaires** : un Contributeur connecté voit le texte réel des commentaires en attente de validation ou
  modérés (ainsi que le motif de modération) et peut modérer depuis le site ; les autres visiteurs voient un
  message générique à la place.

> 📝 Les **jetons API** ont leurs propres rôles, distincts de ceux des comptes : *Lecture* (`ROLE_READ_API`),
> *Lecture + Écriture* (`ROLE_WRITE_API`) et *Admin* (`ROLE_ADMIN_API`), chacun héritant du précédent. Voir
> [Jetons API](ApiToken/jetons_api.md).

## Référence technique

La hiérarchie est déclarée dans `config/packages/security.yaml` :

```yaml
role_hierarchy:
    ROLE_CONTRIBUTEUR: ROLE_USER
    ROLE_READ_API: ROLE_USER
    ROLE_WRITE_API: ROLE_READ_API
    ROLE_ADMIN_API: ROLE_WRITE_API
    ROLE_ADMIN: ROLE_CONTRIBUTEUR
    ROLE_SUPER_ADMIN: [ROLE_ADMIN, ROLE_ALLOWED_TO_SWITCH]
```

`ROLE_ALLOWED_TO_SWITCH` est le rôle Symfony qui autorise la prise de contrôle d'un compte (`_switch_user`),
réservée de fait aux Super Administrateurs. Tout le préfixe `/admin` exige au minimum `ROLE_USER`
(`access_control`).

| Classe / fichier | Rôle |
|---|---|
| [`Role`](https://github.com/counteraccro/natheo/blob/master/src/Utils/System/User/Role.php) | Constantes des 4 rôles, `getListRole()` (liste proposée dans les formulaires), tests `isSuperAdmin()`/`isAdmin()`… sur une instance `User` |
| [`User::getRoles()`](https://github.com/counteraccro/natheo/blob/master/src/Entity/Admin/System/User.php) | Retourne les rôles stockés (champ JSON `roles`) en ajoutant toujours `ROLE_USER` |
| Contrôleurs `src/Controller/Admin/**` | Protégés par `#[IsGranted(...)]` au niveau de la classe (ou de chaque action pour `UserController`) : `ROLE_CONTRIBUTEUR` pour le contenu, `ROLE_SUPER_ADMIN` pour Système et Outils |
| [`SidebarExtension`](https://github.com/counteraccro/natheo/blob/master/src/Twig/Extension/Admin/System/SidebarExtension.php) | Masque les entrées de sidebar dont le rôle n'est pas accordé |
| `templates/admin/includes/header.html.twig` | Recherche globale et bouton *Nouveau* conditionnés à `ROLE_CONTRIBUTEUR` |

> 📝 Les rôles sont vérifiés avec `isGranted()`, qui tient compte de la hiérarchie. En revanche,
> `Role::isAdmin()`/`isSuperAdmin()`… comparent seulement les rôles stockés sur le compte, **sans** hiérarchie :
> `isAdmin()` renvoie `false` pour un Super Administrateur.

## Voir aussi
- [Gestion des utilisateurs](Utilisateurs/listing.md)
- [Ajouter / modifier un utilisateur](Utilisateurs/ajouter_editer.md)
- [Gestion de la sidebar](sidebar.md)
- [Jetons API](ApiToken/jetons_api.md)
