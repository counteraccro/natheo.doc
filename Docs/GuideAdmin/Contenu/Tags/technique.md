---
title: "Référence technique"
parent: "Tags"
grand_parent: "Guide d'administration"
nav_order: 2
---

Vue d'ensemble technique du module Tags : tables, classes backend/frontend et
routes. Pour l'usage côté back-office, voir le [listing](listing.md) et le
[formulaire d'ajout/édition](ajouter_editer.md).

## Tables

### `tag`

| Colonne | Type | Description |
|---|---|---|
| `id` | int, PK | Identifiant |
| `color` | varchar(7) | Couleur au format `#rrggbb` |
| `disabled` | bool | Visibilité du tag (voir le [listing](listing.md)) |
| `created_at` | datetime | Date de création |
| `update_at` | datetime | Date de dernière modification |

### `tag_translation`

| Colonne | Type | Description |
|---|---|---|
| `id` | int, PK | Identifiant |
| `tag_id` | int, FK → `tag.id` | Tag parent |
| `locale` | varchar(10) | Langue (`fr`, `en`, `es`) |
| `label` | varchar(255) | Libellé du tag dans cette langue |

### Relation avec `page`

`tag` est lié à `page` par une relation N—N via la table de jointure
`page_tag` (côté propriétaire : `Page::$tags`). Voir le
[modèle de données complet](../../../Architecture/modele_donnees.md) pour la
vue d'ensemble de toutes les tables du CMS.

## Backend

| Classe | Rôle |
|---|---|
| [`Tag`](https://github.com/counteraccro/natheo/blob/master/src/Entity/Admin/Content/Tag/Tag.php) | Entité Doctrine (`tag`) |
| [`TagTranslation`](https://github.com/counteraccro/natheo/blob/master/src/Entity/Admin/Content/Tag/TagTranslation.php) | Entité Doctrine (`tag_translation`) |
| [`TagRepository`](https://github.com/counteraccro/natheo/blob/master/src/Repository/Admin/Content/Tag/TagRepository.php) | Requêtes : pagination/tri (`getAllPaginate`), recherche pour l'auto-complete (`searchByName`) |
| [`TagService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/Content/Tag/TagService.php) | Logique métier : formatage pour le Grid, recherche, création à la volée (`newTagByNameAndLocale`) |
| [`TagController`](https://github.com/counteraccro/natheo/blob/master/src/Controller/Admin/Content/TagController.php) | Routes admin (`/admin/{_locale}/tag/...`) |
| [`TagTranslate`](https://github.com/counteraccro/natheo/blob/master/src/Utils/Translate/Content/TagTranslate.php) | Construit le tableau de traductions passé au composant Vue `TagForm` |
| [`TagRender`](https://github.com/counteraccro/natheo/blob/master/src/Utils/Content/Tag/TagRender.php) | Génère le HTML d'un tag (pastille colorée) affiché dans le Grid |

Le controller est protégé par `#[IsGranted('ROLE_CONTRIBUTEUR')]` (voir la
hiérarchie des rôles dans `config/packages/security.yaml`).

### Routes principales

| Route | Méthode | Description |
|---|---|---|
| `admin_tag_index` | GET | Page de listing |
| `admin_tag_load_grid_data` | GET | Données paginées du Grid (ajax) |
| `admin_tag_update_disabled` | PUT | Active/désactive un tag (ajax) |
| `admin_tag_delete` | DELETE | Supprime un tag (ajax) |
| `admin_tag_add` / `admin_tag_update` | GET | Formulaire de création / édition |
| `admin_tag_save` | POST | Sauvegarde le formulaire (ajax) |
| `admin_tag_stats` | GET | Bloc statistiques (ajax) |
| `admin_tag_search` | GET | Auto-complete par label + locale (ajax) |
| `admin_tag_tag_by_name` | GET | Récupère (ou crée) un tag par son nom (ajax) |

## Frontend

| Composant | Rôle |
|---|---|
| `Admin/Content/TagForm` (`assets/vue/controllers/Admin/Content/TagForm.vue`) | Formulaire de création/édition, monté via `vue_component()` |
| `Admin/GenericGrid` | Composant Grid générique réutilisé pour le listing — voir [Tableau GRID](../../../Architecture/composants/grid.md) |

### Dépendance croisée : l'éditeur de pages

Les routes `admin_tag_search` et `admin_tag_tag_by_name` ne servent pas
seulement à l'écran Tags : elles sont aussi consommées par l'onglet **Tags**
du formulaire d'édition d'une page (`assets/vue/Components/Page/PageTag.vue`),
qui permet de rattacher des tags à une page depuis un champ auto-complete —
avec création à la volée d'un tag si celui-ci n'existe pas encore
(`TagService::newTagByNameAndLocale`).

## Traductions

Toutes les chaînes (labels, messages de confirmation, aide) sont dans le
domaine de traduction `tag` : `translations/tag+intl-icu.{fr,en,es}.yaml`.
