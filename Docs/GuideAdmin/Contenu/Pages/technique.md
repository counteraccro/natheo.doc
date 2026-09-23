---
title: "Référence technique"
parent: "Pages"
grand_parent: "Guide d'administration"
nav_order: 5
---

Vue d'ensemble technique du module Pages : tables, classes backend/frontend
et routes. Pour l'usage côté back-office, voir le [listing](listing.md),
[créer et éditer une page](ajouter_editer.md), [le contenu de la
page](contenu.md), [SEO, tags et menus](seo_tags_menus.md) et [historique et
aperçu](historique_apercu.md).

## Tables

### `page`

| Colonne | Type | Description |
|---|---|---|
| `id` | int, PK | Identifiant |
| `user_id` | int, FK → `user.id`, NOT NULL | Auteur de la page |
| `render` | int | Mise en page du contenu (voir [`PageRender`](https://github.com/counteraccro/natheo/blob/master/src/Enum/Admin/Content/Page/PageRender.php), 1 à 8, voir [le contenu de la page](contenu.md)) |
| `status` | int | Statut (`PageStatus` : 1 Publiée, 2 Brouillon, 3 Archivée) |
| `disabled` | bool | Visibilité de la page (voir le [listing](listing.md)) |
| `category` | int | Catégorie (`PageCategory`, 1 à 9, entre dans l'URL publique) |
| `landing_page` | bool | Une seule page peut être à `true` à la fois |
| `is_open_comment` | bool | Commentaires ouverts sur cette page |
| `nb_comment` | int | Nombre de commentaires (compteur dénormalisé) |
| `rule_comment` | int | Statut par défaut d'un commentaire déposé sur cette page (`CommentStatus`) |
| `header_img` | text, nullable | URL de l'image d'entête |
| `created_at` | datetime | Date de création |
| `update_at` | datetime, nullable | Date de dernière modification |

### `page_translation`

| Colonne | Type | Description |
|---|---|---|
| `id` | int, PK | Identifiant |
| `page_id` | int, FK → `page.id`, NOT NULL | Page parente |
| `locale` | varchar(10) | Langue |
| `titre` | varchar(255) | Titre dans cette langue |
| `url` | varchar(255) | Url dans cette langue, unique tous locales confondues (voir plus bas) |
| `created_at` | datetime | Date de création |
| `update_at` | datetime, nullable | Date de dernière modification |

### `page_content`

| Colonne | Type | Description |
|---|---|---|
| `id` | int, PK | Identifiant |
| `page_id` | int, FK → `page.id`, NOT NULL | Page parente |
| `render_block` | int | Numéro du bloc (1 à 4 selon la mise en page choisie) |
| `render_order` | int | Non exploité par l'éditeur actuel (toujours `1`, voir plus bas) |
| `type` | int | `PageContentType` : 1 Texte, 2 Faq, 3 Listing |
| `type_id` | int, nullable | Id de la FAQ ou de la catégorie ciblée (`null` pour un bloc texte) |

### `page_content_translation`

| Colonne | Type | Description |
|---|---|---|
| `id` | int, PK | Identifiant |
| `page_content_id` | int, FK → `page_content.id`, NOT NULL | Bloc parent |
| `locale` | varchar(10) | Langue |
| `text` | text | Contenu Markdown (blocs de type Texte uniquement) |

### `page_meta`

| Colonne | Type | Description |
|---|---|---|
| `id` | int, PK | Identifiant |
| `page_id` | int, FK → `page.id`, NOT NULL | Page parente |
| `name` | varchar(100) | Clé de la meta (`PageMeta` : `description`, `keywords`, `author`, `copyright`) |

### `page_meta_translation`

| Colonne | Type | Description |
|---|---|---|
| `id` | int, PK | Identifiant |
| `page_meta_id` | int, FK → `page_meta.id`, NOT NULL | Meta parente |
| `locale` | varchar(10) | Langue |
| `value` | text | Valeur de la meta dans cette langue |

### `page_statistique`

| Colonne | Type | Description |
|---|---|---|
| `id` | int, PK | Identifiant |
| `page_id` | int, FK → `page.id`, NOT NULL | Page parente |
| `key` | varchar(255) | `PAGE_NB_VISITEUR` ou `PAGE_NB_READ` (`PageStatistics`) |
| `value` | varchar(255) | Valeur de la statistique |

`PAGE_NB_READ` alimente la colonne **Affichage** du [listing](listing.md).
Deux tables de liaison many-to-many complètent le modèle : `page_tag`
(tags, voir [Tags](../Tags/technique.md#tables)) et `page_menu` (menus, voir
[Menus](../Menus/technique.md#tables)). Voir le [modèle de données
complet](../../../Architecture/modele_donnees.md) pour la vue d'ensemble de
toutes les tables du CMS.

## Backend

| Classe | Rôle |
|---|---|
| [`Page`](https://github.com/counteraccro/natheo/blob/master/src/Entity/Admin/Content/Page/Page.php) | Entité Doctrine (`page`) |
| [`PageTranslation`](https://github.com/counteraccro/natheo/blob/master/src/Entity/Admin/Content/Page/PageTranslation.php) | Entité Doctrine (`page_translation`) |
| [`PageContent`](https://github.com/counteraccro/natheo/blob/master/src/Entity/Admin/Content/Page/PageContent.php) | Entité Doctrine (`page_content`) |
| [`PageContentTranslation`](https://github.com/counteraccro/natheo/blob/master/src/Entity/Admin/Content/Page/PageContentTranslation.php) | Entité Doctrine (`page_content_translation`) |
| [`PageMeta`](https://github.com/counteraccro/natheo/blob/master/src/Entity/Admin/Content/Page/PageMeta.php) | Entité Doctrine (`page_meta`) |
| [`PageMetaTranslation`](https://github.com/counteraccro/natheo/blob/master/src/Entity/Admin/Content/Page/PageMetaTranslation.php) | Entité Doctrine (`page_meta_translation`) |
| [`PageStatistique`](https://github.com/counteraccro/natheo/blob/master/src/Entity/Admin/Content/Page/PageStatistique.php) | Entité Doctrine (`page_statistique`) |
| [`PageRepository`](https://github.com/counteraccro/natheo/blob/master/src/Repository/Admin/Content/Page/PageRepository.php) | Requêtes : pagination/tri/recherche (`getAllPaginate`), liste par catégorie pour le front (`getPagesByCategoryPaginate`) |
| [`PageTranslationRepository`](https://github.com/counteraccro/natheo/blob/master/src/Repository/Admin/Content/Page/PageTranslationRepository.php) | `isUniqueUrl()` : vérifie l'unicité de l'URL tous locales confondues |
| [`PageService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/Content/Page/PageService.php) | Logique métier : formatage pour le Grid, listes de choix (statuts/rendus/catégories/contenus), bascule de la landing page |
| [`PageController`](https://github.com/counteraccro/natheo/blob/master/src/Controller/Admin/Content/PageController.php) | Routes admin (`/admin/{_locale}/page/...`) |
| [`PageFactory`](https://github.com/counteraccro/natheo/blob/master/src/Utils/Content/Page/PageFactory.php) | Construit une page vide (traductions, 1 statistique par clé, 1 bloc texte, 4 metas vides) dans toutes les locales |
| [`PagePopulate`](https://github.com/counteraccro/natheo/blob/master/src/Utils/Content/Page/PagePopulate.php) | Merge les données reçues du composant Vue dans l'entité `Page` à la sauvegarde |
| [`PageHistory`](https://github.com/counteraccro/natheo/blob/master/src/Utils/Content/Page/PageHistory.php) | Sauvegarde/restauration/listing des snapshots de sauvegarde automatique (voir plus bas) |
| [`PageConst`](https://github.com/counteraccro/natheo/blob/master/src/Utils/Content/Page/PageConst.php) | Constante `DEFAULT_LANDING_PAGE` |
| [`PageTranslate`](https://github.com/counteraccro/natheo/blob/master/src/Utils/Translate/Content/PageTranslate.php) | Construit le tableau de traductions passé au composant Vue `Page` |

Le controller est protégé par `#[IsGranted('ROLE_CONTRIBUTEUR')]` (voir la
hiérarchie des rôles dans `config/packages/security.yaml`).

### Sauvegarde automatique : `PageHistory`

Contrairement au reste du CMS (base de données), la sauvegarde automatique
écrit dans un fichier plat JSON-lines, un fichier par page, dans
`var/pageHistory/page-{id}.txt` (`var/pageHistory-test/...` en environnement
de test). Pour une page pas encore créée, le fichier est nommé
`page-user-{userId}.txt` ; dès la première sauvegarde réelle
(`PageController::save()`), `renamePageHistorySave()` renomme ce fichier
« brouillon » vers `page-{id}.txt` (sans écraser un fichier existant du même
nom). Chaque ligne du fichier est un enregistrement JSON
`{time, user, pageH}` (`pageH` = l'état complet de la page). Supprimer une
page (`PageController::delete()`) supprime aussi son fichier d'historique.
Ce mécanisme est entièrement indépendant de la table `page` : il n'y a
aucune purge automatique, le fichier grandit à chaque sauvegarde
automatique.

### `render_order` non exploité

`PageContent::$renderOrder` (colonne `render_order`) existe dans le modèle
et est toujours écrit à `1` (par `PageFactory` et
`PagePopulate::populatePageContent()`), mais rien ne le lit ni ne permet de
le changer depuis l'éditeur : le seul ordonnancement des blocs vient du
couple mise en page (`render`) + `renderBlock`, réordonnés uniquement par
échange de position (`PageController::handleSwapBlocks` côté Vue). C'est un
reliquat, probablement prévu pour un tri plus fin au sein d'un même bloc qui
n'a jamais été implémenté côté V2.

### Résumé des erreurs de la barre de statut

Chaque onglet fait remonter ses erreurs à `Page.vue` via l'évènement
`update:section-errors`, sous une clé de section : `PageInformation.vue`
utilise la clé **`information`**, `PageContent.vue` (blocs) la clé
**`content`**, `PageSeo.vue` la clé **`seo`**. Ce triplet pilote à la fois le
point rouge affiché sur chaque onglet concerné (Informations/Contenu/SEO,
voir [Créer et éditer une page](ajouter_editer.md)) et, de façon cohérente,
le résumé cliquable de la barre de statut (bas de l'écran) :
`PageStatusBar.sectionLabels` associe chaque clé au bon libellé traduit
(`onglet_information`/`onglet_content`/`onglet_seo`), et
`Page.vue::handleGoToError()` route chaque clé vers le bon onglet
(`information` → `nav-0-tab`, `content` → `nav-1-tab`, `seo` → `nav-2-tab`).

### Route `admin_page_preview` : code mort

Le bouton **Voir le rendu** (voir [Historique et aperçu](historique_apercu.md#aperçu))
n'appelle plus cette route : `Page.vue::openPreview()` construit directement
l'URL publique de la page (`{OS_ADRESSE_SITE}/{locale}/{categorie}/{url}`) et
l'ouvre dans un nouvel onglet (`window.open`). La route `admin_page_preview`
(`PageController::preview()`, template `templates/admin/content/page/preview.html.twig`,
qui appelle `vue_component('Admin/Content/Page/PagePreview', ...)`) reste
définie côté backend mais n'est plus appelée par aucun bouton de
l'interface — probable reliquat d'un ancien écran d'aperçu dédié (le
composant Vue `PagePreview` attendu par le template n'existe d'ailleurs plus
sous `assets/vue/controllers/Admin/Content/Page/`, seul `Page.vue` y est présent).

### Routes principales

| Route | Méthode | Description |
|---|---|---|
| `admin_page_index` | GET | Page de listing |
| `admin_page_load_grid_data` | GET | Données paginées du Grid (ajax) |
| `admin_page_add` / `admin_page_update` | GET | Écran d'édition (création / édition) |
| `admin_page_load_page` | GET | Charge les données complètes d'une page, dont l'historique et les menus disponibles (ajax) |
| `admin_page_save` | POST | Sauvegarde une page (ajax) |
| `admin_page_auto_save` | PUT | Sauvegarde automatique dans `var/pageHistory/` (ajax) |
| `admin_page_load_tab_history` | GET | Liste l'historique de sauvegarde automatique (ajax) |
| `admin_page_reload_page_history` | POST | Restaure une entrée de l'historique (ajax) |
| `admin_page_new_content` | POST | Crée un bloc de contenu vierge d'un type donné (ajax) |
| `admin_page_liste_content_by_id` | GET | Liste des FAQ ou catégories disponibles pour un type de bloc (ajax) |
| `admin_page_is_unique_url_page` | POST | Vérifie l'unicité d'une URL (ajax) |
| `admin_page_info_render_block` | GET | Résumé d'un bloc Faq/Listing (ajax, peu utilisée côté UI actuelle) |
| `admin_page_liste_pages_internal_link` | GET | Liste des pages publiées pour le lien interne de l'éditeur Markdown |
| `admin_page_update_disabled` | PUT | Active/désactive une page (ajax) |
| `admin_page_delete` | DELETE | Supprime une page et son historique (ajax) |
| `admin_page_switch_Landing_page` | PUT | Définit la page comme landing page (ajax) |
| `admin_page_preview` | GET | Ancien écran d'aperçu, plus appelé par le bouton **Voir le rendu** (voir ci-dessus, code mort) |

## Frontend

| Composant | Rôle |
|---|---|
| `Admin/Content/Page/Page` (`assets/vue/controllers/Admin/Content/Page/Page.vue`) | Point d'entrée, monté via `vue_component()` ; gère le chargement, les 7 onglets, la barre de statut et la sauvegarde |
| `PageInformation` | Onglet **Informations** (voir [créer et éditer une page](ajouter_editer.md)) |
| `PageContent` + `PageContentBlock` | Onglet **Contenu** : mise en page et blocs (voir [le contenu de la page](contenu.md)) |
| `PageSeo` | Onglet **SEO** |
| `PageTag` | Onglet **Les tags** |
| `PageMenu` | Onglet **Menus** |
| `PageComment` | Onglet **Commentaires** : ouverture des commentaires (`openComment`) et statut par défaut à la soumission (`ruleComment`) pour cette page, avec rappel du paramétrage global des commentaires |
| `PageHistory` | Onglet **Historique** |
| `PageStatusBar` | Barre de statut fixe : sauvegarde automatique, résumé d'erreurs, boutons Retour/Aperçu/Sauvegarder |
| `Admin/GenericGrid` | Composant Grid générique réutilisé pour le listing — voir [Tableau GRID](../../../Architecture/composants/grid.md) |
| `MarkdownEditor` | Éditeur utilisé pour un bloc de contenu de type Texte — voir [Éditeur Markdown](../../Modules/editeur_markdown.md) |
| `MediathequeModale` | Sélecteur d'image pour l'image d'entête — voir [La médiathèque](../Mediatheque/mediatheque.md) |

Chaque onglet (sauf Tags/Menus/Commentaires) fait remonter ses erreurs de
validation au composant `Page` via un évènement `update:section-errors`,
agrégées ensuite par `PageStatusBar` pour construire le résumé d'erreurs et
désactiver le bouton **Sauvegarder**.

## Traductions

Toutes les chaînes (labels, messages de confirmation, aide) sont dans le
domaine de traduction `page` : `translations/page+intl-icu.{fr,en,es}.yaml`.

Dans `PageService::getAllFormatToGrid()`, la variable locale `$isDisabled`
(pensée pour afficher un badge « désactivée » à côté du numéro dans le Grid,
sur le même principe que le badge landing page 📌) est déclarée puis jamais
réassignée : elle reste toujours une chaîne vide, le badge ne s'affiche donc
jamais. La donnée `isDisabled` transmise séparément au Grid (utilisée pour
l'affichage grisé de la ligne) n'est pas affectée par ce reliquat.

## Fixtures

| | |
|---|---|
| Fichier de données | `src/DataFixtures/data/content/page/page_fixtures_data.yaml` |
| Classe de fixture | [`PageFixtures`](https://github.com/counteraccro/natheo/blob/master/src/DataFixtures/Admin/Content/Page/PageFixtures.php) |
| Groupes | `page`, `content` |

```bash
php bin/console doctrine:fixtures:load --group=page
```

Les menus associés à une page sont dans une fixture séparée
(`src/DataFixtures/data/content/page/page_menu_fixtures_data.yaml`,
[`PageMenuFixtures`](https://github.com/counteraccro/natheo/blob/master/src/DataFixtures/Admin/Content/Page/PageMenuFixtures.php)).

## Tests

| Fichier | Contenu |
|---|---|
| [`PageServiceTest`](https://github.com/counteraccro/natheo/blob/master/tests/Service/Admin/Content/Page/PageServiceTest.php) | Tests du service (Grid, listes de choix, bascule landing page) |
| [`PageFactoryTest`](https://github.com/counteraccro/natheo/blob/master/tests/Utils/Content/Page/PageFactoryTest.php) | Tests de la création d'une page vide |
| [`PagePopulateTest`](https://github.com/counteraccro/natheo/blob/master/tests/Utils/Content/Page/PagePopulateTest.php) | Tests du merge des données reçues du front |
| [`PageHistoryTest`](https://github.com/counteraccro/natheo/blob/master/tests/Utils/Content/Page/PageHistoryTest.php) | Tests de la sauvegarde automatique (fichiers plats) |
| [`PageControllerTest`](https://github.com/counteraccro/natheo/blob/master/tests/Controller/Admin/Content/PageControllerTest.php) | Tests des routes du contrôleur admin |
| `PageFixturesTrait` (`tests/Helper/Fixtures/Content/PageFixturesTrait.php`) | Aide de test pour créer des pages factices |

Le domaine **API** (`Api/v1/Content/PageController`, `ApiPageService`,
`ApiPageContentService`) restitue les pages sur le site public — voir
[Find page](../../../API/References/find_page.md),
[Find page content](../../../API/References/find_page_content.md),
[Listing pages by category](../../../API/References/listing_pages_category.md)
et [Listing pages by tags](../../../API/References/listing_pages_tags.md).
