---
title: "Référence technique"
parent: "Menus"
grand_parent: "Guide d'administration"
nav_order: 2
---

Vue d'ensemble technique du module Menus : tables, classes backend/frontend
et routes. Pour l'usage côté back-office, voir [Menus](listing.md) et
[Ajouter / Éditer un menu](ajouter_editer.md).

## Tables

### `menu`

| Colonne | Type | Description |
|---|---|---|
| `id` | int, PK | Identifiant |
| `user_id` | int, FK → `user.id`, nullable | Auteur |
| `name` | varchar(255) | Nom interne du menu |
| `type` | int | Valeur de l'enum `MenuType`, dépendante de `position` |
| `position` | int | Valeur de l'enum `MenuPosition` (1 haut de page, 2 à droite, 3 pied de page, 4 à gauche) |
| `render_order` | int | Non exploité par l'écran actuel |
| `default_menu` | bool | Un seul menu par défaut actif à la fois par position |
| `disabled` | bool | Menu désactivé (invisible sur le site) |
| `created_at` / `update_at` | datetime | Dates de création / dernière modification |

### `menu_element`

| Colonne | Type | Description |
|---|---|---|
| `id` | int, PK | Identifiant |
| `menu_id` | int, FK → `menu.id`, non nullable | Menu propriétaire |
| `parent_id` | int, FK → `menu_element.id`, nullable, `ON DELETE CASCADE` | Élément parent (arborescence auto-référencée) |
| `page_id` | int, FK → `page.id`, nullable | Page ciblée si lien interne |
| `column_position` / `row_position` | int | Position dans l'arborescence (mise à jour par le glisser-déposer) |
| `link_target` | varchar(100) | `_self` ou `_blank` (`MenuLinkTarget`) |
| `disabled` | bool | Élément masqué (cascade visuelle sur ses enfants, voir [Architecture du menu](ajouter_editer.md#architecture-du-menu)) |

### `menu_element_translation`

| Colonne | Type | Description |
|---|---|---|
| `id` | int, PK | Identifiant |
| `menu_element_id` | int, FK → `menu_element.id`, non nullable | Élément traduit |
| `locale` | varchar(10) | Langue |
| `text_link` | varchar(255) | Label affiché |
| `external_link` | text, nullable | URL externe (lien externe uniquement) |

### `page_menu`

Table de jointure Many-to-Many entre `page` et `menu` (association d'un menu
à des pages précises, indépendante du statut « par défaut » — voir
[Le menu par défaut](ajouter_editer.md#le-menu-par-défaut)). Éditable
uniquement depuis l'onglet *Menu* de l'écran d'édition d'une page ; aucun
contrôle correspondant côté écran Menu.

Suppression en cascade : supprimer un `menu` supprime tous ses
`menu_element` (`orphanRemoval`), qui suppriment eux-mêmes leurs
`menu_element_translation` (`orphanRemoval`) et leurs enfants
(`ON DELETE CASCADE` en base sur `parent_id`).

## Backend

| Classe | Rôle |
|---|---|
| [`Menu`](https://github.com/counteraccro/natheo/blob/master/src/Entity/Admin/Content/Menu/Menu.php) | Entité Doctrine (`menu`) |
| [`MenuElement`](https://github.com/counteraccro/natheo/blob/master/src/Entity/Admin/Content/Menu/MenuElement.php) | Entité Doctrine (`menu_element`), arborescence auto-référencée |
| [`MenuElementTranslation`](https://github.com/counteraccro/natheo/blob/master/src/Entity/Admin/Content/Menu/MenuElementTranslation.php) | Entité Doctrine (`menu_element_translation`) |
| [`MenuRepository`](https://github.com/counteraccro/natheo/blob/master/src/Repository/Admin/Content/Menu/MenuRepository.php) | Requêtes : listing paginé, menus par défaut d'une position, requêtes dédiées à l'API front (voir [Dépendance : l'API front](#dépendance-croisée--lapi-front)) |
| [`MenuElementRepository`](https://github.com/counteraccro/natheo/blob/master/src/Repository/Admin/Content/Menu/MenuElementRepository.php) | Requêtes : éléments d'un menu par parent |
| [`MenuService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/Content/Menu/MenuService.php) | Listing/Grid, listes position/type, calcul des positions sans défaut, bascule du menu par défaut |
| [`MenuFactory`](https://github.com/counteraccro/natheo/blob/master/src/Utils/Content/Menu/MenuFactory.php) | Crée un `MenuElement` vide avec ses traductions (une par langue) |
| [`MenuPopulate`](https://github.com/counteraccro/natheo/blob/master/src/Utils/Content/Menu/MenuPopulate.php) | Fusionne le tableau reçu du front dans l'entité `Menu` (menu, arborescence d'éléments récursive, association pages) à la sauvegarde |
| [`MenuConvertToArray`](https://github.com/counteraccro/natheo/blob/master/src/Utils/Content/Menu/MenuConvertToArray.php) | Convertit un `Menu` en tableau pour le front (sens inverse de `MenuPopulate`) |
| [`MenuController`](https://github.com/counteraccro/natheo/blob/master/src/Controller/Admin/Content/MenuController.php) | Routes admin (`/admin/{_locale}/menu/...`) |
| [`MenuTranslate`](https://github.com/counteraccro/natheo/blob/master/src/Utils/Translate/Content/MenuTranslate.php) | Construit le tableau de traductions passé au composant Vue `Menu` |

Le controller est protégé par `#[IsGranted('ROLE_CONTRIBUTEUR')]` (voir la
hiérarchie des rôles dans `config/packages/security.yaml`), au niveau de la
classe entière. L'action de suppression n'est proposée dans le Grid que si
l'option système *Autoriser la suppression des données* est activée
(`OptionSystemService::canDelete()`).

### Routes principales

| Route | Méthode | Description |
|---|---|---|
| `admin_menu_index` | GET | Page d'accueil (listing) |
| `admin_menu_load_grid_data` | GET | Contenu du Grid (ajax) |
| `admin_menu_update_disabled` | PUT | Active/désactive un menu (ajax) |
| `admin_menu_delete` | DELETE | Supprime définitivement un menu (ajax) |
| `admin_menu_switch_default` | PUT | Définit un menu comme par défaut de sa position, retire ce statut aux autres menus de la même position (ajax) |
| `admin_menu_add` / `admin_menu_update` | GET | Écran de création / édition |
| `admin_menu_load_menu` | GET | Charge un menu (et son arborescence complète) en JSON (ajax) |
| `admin_menu_save_menu` | POST | Crée ou modifie un menu, toute son arborescence et son statut par défaut (ajax) |
| `admin_menu_list_parent_menu_element` | GET | Retourne les parents valides pour un élément donné — générée (`urls.list_parent_menu_element`) mais **jamais appelée** par le composant `Menu` actuel (aucun sélecteur de parent dans le formulaire, voir [Architecture du menu](ajouter_editer.md#architecture-du-menu)) |

## Fixtures

| | |
|---|---|
| Fichier de données | `src/DataFixtures/data/content/menu/menu_fixtures_data.yaml` |
| Classe de fixture | [`MenuFixtures`](https://github.com/counteraccro/natheo/blob/master/src/DataFixtures/Admin/Content/Menu/MenuFixtures.php) |
| Groupes | `menu`, `content` |

```bash
php bin/console doctrine:fixtures:load --group=menu
```

## Frontend

| Composant | Rôle |
|---|---|
| `Admin/Content/Menu/Menu` (`assets/vue/controllers/Admin/Content/Menu/Menu.vue`) | Composant racine de l'écran [Ajouter / Éditer un menu](ajouter_editer.md), monté via `vue_component()` |
| `MenuTree` | Nœud d'arborescence récursif (glisser-déposer, actions, badges d'état) |
| `MenuElementForm` | Formulaire d'édition d'un élément (voir [Éditer un élément de menu](ajouter_editer.md#éditer-un-élément-de-menu)) |
| `MenuHeader` / `MenuFooter` / `MenuLeftRight` | Aiguillent vers le bon sous-composant d'aperçu selon `type` (ex. `MenuHeaderMega` avec 0 à 4 colonnes pour les menus déroulants géants) |

`Admin/GenericGrid` est utilisé pour le [listing](listing.md) (voir
[Tableau GRID](../../../Architecture/composants/grid.md)) — contrairement à
la [Médiathèque](../Mediatheque/technique.md), ce domaine n'a pas de grille
dédiée.

`assets/utils/Admin/Content/Menu/MenuElementsTools.js` (classe
`MenuElementTools`) n'est importé par aucun composant du module : code mort
laissé en place lors de la réécriture Vue 3 de l'écran.

### Dépendance croisée : l'API front

`ApiMenuController` / `ApiMenuService` exposent les menus au front public
(recherche par position + URL de page, ou menu par défaut de la position),
via les mêmes entités que ce module. C'est ce qui détermine réellement quel
menu s'affiche pour une page donnée : le menu associé explicitement à la
page (table `page_menu`) prime, sinon le menu par défaut de la position est
utilisé.

## Traductions

Toutes les chaînes sont dans le domaine de traduction `menu` :
`translations/menu+intl-icu.{fr,en,es}.yaml`. Plusieurs clés n'ont plus de
correspondance dans l'écran actuel — reliquats de traductions non nettoyés
lors de la réécriture Vue 3 : `menu.form.parent.*` et
`menu.form.position.column/row.label` (pas de sélecteur de parent ni de
saisie manuelle de colonne/ligne, uniquement le glisser-déposer), et
`menu.checkbox.default.menu.false.label` (« Menu classique », jamais
affiché — le libellé du switch *Menu par défaut* reste fixe quel que soit son
état).
