---
title: "Référence technique"
parent: "Commentaires"
grand_parent: "Guide d'administration"
nav_order: 3
---

Vue d'ensemble technique du module Commentaires côté back-office : table,
enum, classes backend/frontend et routes. Pour l'usage, voir le
[listing](listing.md), la [modération d'un commentaire](moderation.md) et la
[modération globale](moderation_globale.md). Pour l'API publique utilisée par
le front pour poster/lister des commentaires, voir
[Ajouter un commentaire](../../../API/References/add_comment.md),
[Liste des commentaires d'une page](../../../API/References/comment_by_page.md)
et [Modération d'un commentaire (API)](../../../API/References/moderate_comment.md).

## Table

### `comment`

| Colonne | Type | Description |
|---|---|---|
| `id` | int, PK | Identifiant |
| `page_id` | int, FK → `page.id`, NOT NULL | Page sur laquelle le commentaire a été déposé |
| `user_moderation_id` | int, FK → `user.id`, nullable | Modérateur (renseigné uniquement quand `status = MODERATE`) |
| `author` | varchar(255) | Nom de l'auteur |
| `email` | varchar(255), nullable | E-mail de l'auteur |
| `comment` | text | Contenu du commentaire, en Markdown |
| `status` | int | Statut, voir l'enum `CommentStatus` |
| `disabled` | bool | Colonne présente en base (toujours `false` à la création) mais non exploitée par une action du back-office actuel |
| `moderation_comment` | text, nullable | Commentaire laissé par le modérateur |
| `ip` | varchar(255) | Adresse IP au moment du dépôt |
| `user_agent` | text | User-agent au moment du dépôt |
| `created_at` | datetime | Date de dépôt |
| `update_at` | datetime, nullable | Date de dernière modification |

`comment` est lié à `page` par une relation ManyToOne (`Page::$comments` en
OneToMany). Voir le
[modèle de données complet](../../../Architecture/modele_donnees.md).

## Enum

| Enum | Valeurs | Rôle |
|---|---|---|
| [`CommentStatus`](https://github.com/counteraccro/natheo/blob/master/src/Enum/Admin/Comment/CommentStatus.php) | `WAIT_VALIDATION` (1), `VALIDATE` (2), `MODERATE` (3) | Statut d'un commentaire ; fournit aussi la classe CSS du badge associé (`getClassCss()`) |

## Backend

| Classe | Rôle |
|---|---|
| [`Comment`](https://github.com/counteraccro/natheo/blob/master/src/Entity/Admin/Content/Comment/Comment.php) | Entité Doctrine (`comment`) |
| [`CommentRepository`](https://github.com/counteraccro/natheo/blob/master/src/Repository/Admin/Content/Comment/CommentRepository.php) | Requêtes : pagination/tri/recherche (`getAllPaginate`), filtre statut/page pour la modération globale (`getListCommentsByFilter`), comptage par statut (`getNbGroupByStatus`) |
| [`CommentService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/Content/Comment/CommentService.php) | Logique métier : formatage pour le Grid, statuts (texte/badge HTML), filtre de modération globale, mise à jour groupée (`updateMultipleComment`) |
| [`CommentController`](https://github.com/counteraccro/natheo/blob/master/src/Controller/Admin/Content/CommentController.php) | Routes admin (`/admin/{_locale}/comment/...`) |
| [`CommentTranslate`](https://github.com/counteraccro/natheo/blob/master/src/Utils/Translate/Content/CommentTranslate.php) | Construit les tableaux de traductions passés aux composants Vue `CommentEdit` et `CommentModeration` |
| [`CommentPopulate`](https://github.com/counteraccro/natheo/blob/master/src/Utils/Content/Comment/CommentPopulate.php) | Reporte un tableau de données (venant du composant Vue) sur une entité `Comment`, en excluant `id`/`page`/`userModeration`/`createdAt`/`updateAt` |

Le controller est protégé par `#[IsGranted('ROLE_CONTRIBUTEUR')]` (voir la
hiérarchie des rôles dans `config/packages/security.yaml`).

Le filtre **Moi / Tous** du listing (`admin_comment_load_grid_data`) restreint
la liste aux commentaires dont **`userModeration`** correspond à
l'utilisateur connecté — autrement dit, ceux que vous avez personnellement
modérés (pas les commentaires déposés sur vos propres pages).

### Routes principales

| Route | Méthode | Description |
|---|---|---|
| `admin_comment_index` | GET | Listing des commentaires |
| `admin_comment_load_grid_data` | GET | Données paginées du Grid (ajax) |
| `admin_comment_moderate_comments` | GET | Page de modération globale |
| `admin_comment_moderate_comments_filter` | GET | Liste filtrée (statut/page) pour la modération globale (ajax) |
| `admin_comment_see` | GET | Détail/modération d'un commentaire |
| `admin_comment_load` | GET | Données d'un commentaire (ajax) |
| `admin_comment_save` | PUT | Sauvegarde d'un commentaire (ajax) |
| `admin_comment_update_moderate_comment` | POST | Sauvegarde groupée depuis la modération globale (ajax) |

### Sauvegarde d'un commentaire

`CommentController::updateComment()` retire `statusStr` (calculé, jamais
persisté) du tableau reçu, force `status` en entier, puis passe le reste par
`CommentPopulate`. Si le nouveau statut est `MODERATE`, l'utilisateur courant
est enregistré comme modérateur ; sinon, `moderationComment` et
`userModeration` sont remis à `null`, même s'ils étaient déjà renseignés.
`CommentController::updateCommentModerate()` applique la même règle à toute
une sélection via `CommentService::updateMultipleComment()`.

## Frontend

| Composant | Rôle |
|---|---|
| `Admin/Content/Comment/CommentEdit` (`assets/vue/controllers/Admin/Content/Comment/CommentEdit.vue`) | Détail/édition d'un commentaire — voir [Modérer un commentaire](moderation.md) |
| `Admin/Content/Comment/CommentModeration` (`assets/vue/controllers/Admin/Content/Comment/CommentModeration.vue`) | Modération globale — voir [Modération globale](moderation_globale.md) |
| `Admin/GenericGrid` | Composant Grid générique réutilisé pour le listing — voir [Tableau GRID](../../../Architecture/composants/grid.md) |
| `MarkdownEditor` | Éditeur utilisé pour le contenu du commentaire dans `CommentEdit` — voir [Éditeur Markdown](../../Modules/editeur_markdown.md) |

## Traductions

Toutes les chaînes sont dans le domaine de traduction `comment` :
`translations/comment+intl-icu.{fr,en,es}.yaml`.

## Fixtures

| | |
|---|---|
| Fichier de données | `src/DataFixtures/data/content/comment/comment_fixtures_data.yaml` |
| Classe de fixture | [`CommentFixtures`](https://github.com/counteraccro/natheo/blob/master/src/DataFixtures/Admin/Content/Comment/CommentFixtures.php) |
| Groupes | `page`, `content`, `comment` |

## Tests

| Fichier | Contenu |
|---|---|
| [`CommentServiceTest`](https://github.com/counteraccro/natheo/blob/master/tests/Service/Admin/Content/Comment/CommentServiceTest.php) | Tests du service (Grid, statuts, filtre de modération, mise à jour groupée) |
| [`CommentPopulateTest`](https://github.com/counteraccro/natheo/blob/master/tests/Utils/Content/Comment/CommentPopulateTest.php) | Tests du report de données sur l'entité |
| [`CommentControllerTest`](https://github.com/counteraccro/natheo/blob/master/tests/Controller/Admin/Content/CommentControllerTest.php) | Tests des routes du contrôleur admin |
| `CommentFixturesTrait` (`tests/Helper/Fixtures/Content/CommentFixturesTrait.php`) | Aide de test pour créer des commentaires factices |
