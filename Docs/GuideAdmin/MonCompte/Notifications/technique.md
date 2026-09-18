---
title: "Référence technique"
parent: "Notifications"
grand_parent: "Guide d'administration"
nav_order: 1
---

Vue d'ensemble technique du module Notifications : tables, enums, classes
backend/frontend et routes. Pour l'usage côté back-office, voir la
[page fonctionnelle](notifications.md).

## Tables

### `notification`

| Colonne | Type | Description |
|---|---|---|
| `id` | int, PK | Identifiant |
| `user_id` | int, FK → `user.id` | Destinataire de la notification |
| `title` | varchar(255) | Clé de traduction du titre (domaine `notification`) |
| `content` | text | Clé de traduction du contenu |
| `level` | int | Niveau, voir l'enum `NotificationLevel` |
| `read` | bool | Statut lu / non lu |
| `created_at` | datetime | Date de création |
| `parameters` | text, nullable | Paramètres de traduction, encodés en JSON |
| `category` | varchar(100) | Catégorie, voir l'enum `NotificationCategory` |

La relation avec `user` est une ManyToOne côté `Notification`
(`User::$notifications` en OneToMany, `cascade: ['persist']`,
`orphanRemoval: true`) : sauvegarder le `User` suffit à persister les
nouvelles notifications qui lui ont été ajoutées. Voir le
[modèle de données complet](../../../Architecture/modele_donnees.md).

## Enums

| Enum | Rôle |
|---|---|
| [`NotificationCategory`](https://github.com/counteraccro/natheo/blob/master/src/Enum/Admin/Global/Notification/NotificationCategory.php) | `comment`, `admin`, `SQL` — utilisée pour les onglets du centre de notifications |
| [`NotificationLevel`](https://github.com/counteraccro/natheo/blob/master/src/Enum/Admin/Global/Notification/NotificationLevel.php) | `INFO` (1), `WARNING` (2), `ALERTE` (3) |
| [`NotificationKeyConfig`](https://github.com/counteraccro/natheo/blob/master/src/Enum/Admin/Global/Notification/NotificationKeyConfig.php) | Clés du tableau de configuration d'un type de notification (`category`, `level`, `parameters`, `title`, `content`) |
| [`Notification`](https://github.com/counteraccro/natheo/blob/master/src/Enum/Admin/Global/Notification/Notification.php) | Catalogue des types de notification existants (voir ci-dessous) |

L'enum `Notification` associe à chaque clé une catégorie, un niveau, une
liste de paramètres de traduction par défaut, et les clés de traduction du
titre/contenu :

| Clé | Catégorie | Déclencheur | Destinataire(s) |
|---|---|---|---|
| `NOTIFICATION_WELCOME` | admin | Création d'un compte par un administrateur ([`UserController`](https://github.com/counteraccro/natheo/blob/master/src/Controller/Admin/System/UserController.php)) | Le nouvel utilisateur |
| `NOTIFICATION_SELF_DISABLED` | admin | Un utilisateur désactive lui-même son compte ([`UserController`](https://github.com/counteraccro/natheo/blob/master/src/Controller/Admin/System/UserController.php)) | Les super-administrateurs |
| `NOTIFICATION_SELF_DELETE` / `NOTIFICATION_SELF_ANONYMOUS` | admin | Un utilisateur supprime / anonymise lui-même son compte ([`UserController`](https://github.com/counteraccro/natheo/blob/master/src/Controller/Admin/System/UserController.php)) | Les super-administrateurs |
| `NOTIFICATION_DUMP_SQL` | SQL | Un dump SQL demandé est terminé ([`DumpSqlHandler`](https://github.com/counteraccro/natheo/blob/master/src/MessageHandler/Tools/DumpSqlHandler.php)) | L'utilisateur ayant lancé la sauvegarde |
| `NOTIFICATION_NEW_FONDATEUR` | admin | Fin de l'installation ([`InstallationService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Installation/InstallationService.php)) | Le compte fondateur |
| `new_comment` | comment | Nouveau commentaire déposé ([`ApiCommentService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Api/Content/ApiCommentService.php)) | L'auteur (propriétaire) de la page commentée |

> ⚠️ Toutes les clés suivent la convention `NOTIFICATION_XXX`, sauf
> `new_comment` (héritée d'une version antérieure).

> ⚠️ `NOTIFICATION_SELF_DISABLED`, `NOTIFICATION_SELF_DELETE` et
> `NOTIFICATION_SELF_ANONYMOUS` ne sont envoyées que si l'utilisateur qui agit
> sur son propre compte n'a pas le rôle `ROLE_SUPER_ADMIN` (condition
> `!$role->isSuperAdmin()` dans `UserController`). Les deux dernières exigent
> en plus que l'option système `OS_ALLOW_DELETE_DATA` (`canDelete()`) soit
> active.

## Backend

| Classe | Rôle |
|---|---|
| [`Notification`](https://github.com/counteraccro/natheo/blob/master/src/Entity/Admin/Notification.php) | Entité Doctrine (`notification`) |
| [`NotificationRepository`](https://github.com/counteraccro/natheo/blob/master/src/Repository/Admin/NotificationRepository.php) | Requêtes : comptage, pagination, purge (SQL brut), passage en lu de masse |
| [`NotificationService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/NotificationService.php) | Logique métier : création (`add`), lecture paginée avec traduction à la volée, statistiques, purge, passage en lu/suppression |
| [`NotificationController`](https://github.com/counteraccro/natheo/blob/master/src/Controller/Admin/NotificationController.php) | Routes (`/admin/{_locale}/notification/...`) |
| [`NotificationFactory`](https://github.com/counteraccro/natheo/blob/master/src/Utils/Notification/NotificationFactory.php) | Construit une entité `Notification` à partir d'une clé de l'enum `Notification` et d'un tableau de paramètres, sans flush — l'appelant sauvegarde ensuite le `User` (cascade persist) |
| [`NotificationTranslate`](https://github.com/counteraccro/natheo/blob/master/src/Utils/Translate/NotificationTranslate.php) | Construit le tableau de traductions (libellés de l'interface) passé au composant Vue `NotificationList` |

Le controller est protégé par `#[IsGranted('ROLE_USER')]` : contrairement aux
autres modules du back-office, chaque utilisateur authentifié gère ses
propres notifications, quel que soit son rôle.

Toute la fonctionnalité est conditionnée par
`OptionSystemService::canNotification()` (option `OS_NOTIFICATION`) :
`NotificationService::add()` et `addForFixture()` ne font rien si l'option
est désactivée, la route `index` redirige alors vers le dashboard, et
l'en-tête n'affiche pas l'icône (test `os_notification` dans
`templates/admin/includes/header.html.twig`).

### Routes principales

| Route | Méthode | Description |
|---|---|---|
| `admin_notification_index` | GET | Centre de notifications |
| `admin_notification_number` | GET | Nombre de notifications non lues de l'utilisateur (ajax, pastille de l'en-tête) |
| `admin_notification_list` | GET | Liste paginée des notifications de l'utilisateur (ajax) |
| `admin_notification_update` | POST | Passe une liste de notifications en lu / non lu (ajax) |
| `admin_notification_delete` | POST | Supprime une liste de notifications (ajax) |
| `admin_notification_purge` | POST | Purge les notifications lues trop anciennes (ajax) |
| `admin_notification_read_all` | GET | Passe toutes les notifications non lues en lu (ajax) |
| `admin_notification_statistics` | GET | Statistiques (non lu / aujourd'hui / total) (ajax) |

### Purge

La purge n'est pas une tâche planifiée : elle est déclenchée côté client à
chaque ouverture du centre de notifications (`NotificationList.vue`, hook
`mounted`), via un appel à `admin_notification_purge`. Le contrôleur lit le
nombre de jours dans l'option système `OS_PURGE_NOTIFICATION`, puis
`NotificationService::purge()` exécute une requête SQL brute (dépendante du
SGBD, via `RawQueryManager` / `RawMysqlQuery` / `RawPostgresQuery`) qui
supprime les notifications **lues** de l'utilisateur dont l'ancienneté dépasse
ce nombre de jours.

### Générer une notification

Depuis un service ou un contrôleur, le plus simple est d'utiliser
`NotificationService::add()`, qui vérifie lui-même que l'option
`OS_NOTIFICATION` est active :

```php
$notificationService->add($user, Notification::NEW_COMMENT->value, [
    'author' => $author,
    'status' => $status,
    'page' => $pageTitle,
    'id' => $commentId,
]);
```

Pour ajouter plusieurs notifications au même `User` avant un seul flush (cas
des fixtures, ou de l'installation), on peut passer directement par
`NotificationFactory`, comme le fait `ApiCommentService` ou
`InstallationService`.

## Frontend

| Composant | Rôle |
|---|---|
| `Admin/Notification/NotificationBadge` (`assets/vue/controllers/Admin/Notification/NotificationBadge.vue`) | Pastille rouge sur l'icône cloche de l'en-tête ; un seul appel à `admin_notification_number` au montage, pas de rafraîchissement automatique |
| `Admin/Notification/NotificationList` (`assets/vue/controllers/Admin/Notification/NotificationList.vue`) | Page complète : statistiques, onglets, sélection multiple, actions groupées, modale de confirmation, toasts |
| `Admin/Notification/Notification` (`assets/vue/controllers/Admin/Notification/Notification.vue`) | Une ligne de notification (icône, titre, contenu, temps relatif, actions marquer lu / supprimer) |

## Traductions

Toutes les chaînes — libellés de l'interface **et** titres/contenus des
notifications elles-mêmes (clés `notification.msg.*`) — sont dans le domaine
de traduction `notification` :
`translations/notification+intl-icu.{fr,en,es}.yaml`.

## Tests

| Fichier | Contenu |
|---|---|
| [`NotificationServiceTest`](https://github.com/counteraccro/natheo/blob/master/tests/Service/Admin/NotificationServiceTest.php) | Tests du service (ajout, pagination, statistiques, purge, lu/non lu) |
| [`NotificationFactoryTest`](https://github.com/counteraccro/natheo/blob/master/tests/Utils/Notification/NotificationFactoryTest.php) | Tests de la fabrique de notifications |
| [`NotificationControllerTest`](https://github.com/counteraccro/natheo/blob/master/tests/Controller/Admin/NotificationControllerTest.php) | Tests des routes du contrôleur |
| `NotificationFixturesTrait` (`tests/Helper/Fixtures/System/NotificationFixturesTrait.php`) | Aide de test pour créer une notification factice — il n'existe pas de fixtures de données dédiées (les notifications sont toujours générées dynamiquement, jamais en seed) |
