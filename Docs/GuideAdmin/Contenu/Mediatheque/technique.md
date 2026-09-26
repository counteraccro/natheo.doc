---
title: "Référence technique"
parent: "La médiathèque"
grand_parent: "Guide d'administration"
nav_order: 4
---

Vue d'ensemble technique du module Médiathèque : tables, classes
backend/frontend et routes. Pour l'usage côté back-office, voir
[la médiathèque](mediatheque.md), [médias](medias.md),
[dossiers](dossiers.md) et [déplacer/corbeille](deplacer_et_corbeille.md).

## Tables

### `media_folder`

| Colonne | Type | Description |
|---|---|---|
| `id` | int, PK | Identifiant |
| `parent_id` | int, FK → `media_folder.id`, nullable | Dossier parent (`NULL` = dossier racine) |
| `name` | varchar(150) | Nom du dossier |
| `path` | text | Chemin des ancêtres (ex. `/parent/grand-parent`), sans le nom du dossier lui-même |
| `disabled` | bool | Non exploité par l'écran actuel (toujours `false`) |
| `trash` | bool | Présence dans la corbeille (voir [corbeille](deplacer_et_corbeille.md#la-corbeille)) |
| `created_at` / `update_at` | datetime | Dates de création / dernière modification |

### `media`

| Colonne | Type | Description |
|---|---|---|
| `id` | int, PK | Identifiant |
| `media_folder_id` | int, FK → `media_folder.id`, nullable | Dossier contenant le média (`NULL` = racine) |
| `user_id` | int, FK → `user.id` | Auteur de l'ajout |
| `name` | text | Nom physique du fichier sur le disque (généré à l'upload, avec suffixe aléatoire) |
| `title` | varchar(255) | Titre affiché (voir [modifier un média](medias.md#modifier-un-média)) |
| `description` | text, nullable | Description libre |
| `type` | varchar(50) | `img` ou `file` (`MediaConst`) |
| `extension` | varchar(50) | Extension du fichier |
| `size` | int | Poids en octets |
| `thumbnail` | varchar(255), nullable | Nom du fichier miniature (images uniquement) |
| `path` | text | Chemin du fichier sur le disque, relatif à la racine médiathèque |
| `web_path` | text | Chemin web public |
| `disabled` | bool | Non exploité par l'écran actuel |
| `trash` | bool | Présence dans la corbeille |
| `created_at` / `update_at` | datetime | Dates de création / dernière modification |

`media_folder` est une arborescence auto-référencée (`parent`/`children`,
`onDelete: CASCADE` en base), et `media` référence son dossier parent en
`ON DELETE` cascade côté Doctrine (`cascade: ['remove']`) : supprimer un
dossier supprime tous ses sous-dossiers et médias.

## Backend

| Classe | Rôle |
|---|---|
| [`Media`](https://github.com/counteraccro/natheo/blob/master/src/Entity/Admin/Content/Media/Media.php) | Entité Doctrine (`media`) |
| [`MediaFolder`](https://github.com/counteraccro/natheo/blob/master/src/Entity/Admin/Content/Media/MediaFolder.php) | Entité Doctrine (`media_folder`), arborescence auto-référencée |
| [`MediaRepository`](https://github.com/counteraccro/natheo/blob/master/src/Repository/Admin/Content/Media/MediaRepository.php) | Requêtes : médias d'un dossier, recherche par chemin (via `PathPrefixQueryTrait`), comptage corbeille |
| [`MediaFolderRepository`](https://github.com/counteraccro/natheo/blob/master/src/Repository/Admin/Content/Media/MediaFolderRepository.php) | Requêtes : sous-dossiers, arborescence pour le déplacement, comptage corbeille, recherche d'un dossier par nom au sein d'un même parent (unicité) |
| [`MediaFolderService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/Content/Media/MediaFolderService.php) | Gestion des dossiers : création/renommage/déplacement (BDD + disque), calcul de taille, arborescence |
| [`MediaService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/Content/Media/MediaService.php) (étend `MediaFolderService`) | Gestion des médias : upload, déplacement, corbeille, miniatures |
| [`MediaController`](https://github.com/counteraccro/natheo/blob/master/src/Controller/Admin/Content/MediaController.php) | Routes admin (`/admin/{_locale}/media/...`) |
| [`MediaTranslate`](https://github.com/counteraccro/natheo/blob/master/src/Utils/Translate/Content/MediaTranslate.php) | Construit le tableau de traductions passé au composant Vue `Mediatheque` |
| [`Thumbnail`](https://github.com/counteraccro/natheo/blob/master/src/Utils/Content/Media/Thumbnail.php) | Génère une miniature JPEG (200px de large) pour les images (`jpg`, `jpeg`, `png`, `gif`) |
| [`MediaConst`](https://github.com/counteraccro/natheo/blob/master/src/Utils/Content/Media/MediaConst.php) / [`MediaFolderConst`](https://github.com/counteraccro/natheo/blob/master/src/Utils/Content/Media/MediaFolderConst.php) | Constantes (types de média, extensions/MIME autorisées à l'upload, taille max 20 Mo, chemins racine, dossier par défaut `natheotheque`) |

Le controller est protégé par `#[IsGranted('ROLE_CONTRIBUTEUR')]` (voir la
hiérarchie des rôles dans `config/packages/security.yaml`), au niveau de la
classe entière — toutes les routes exigent donc le même rôle minimum.

### Stockage et options système

Le dossier racine de la médiathèque et son URL publique sont configurables
via les [options système](../../Systeme/options.md) `OS_MEDIA_PATH` et
`OS_MEDIA_URL` (à défaut : dossier `natheotheque` sous `public/assets/`).
L'option `OS_MEDIA_CREATE_PHYSICAL_FOLDER` (activée par défaut) détermine si
un dossier physique est réellement créé sur le disque pour chaque
`MediaFolder`, ou si l'arborescence reste purement logique en base — dans ce
second cas, tous les fichiers sont physiquement stockés à plat à la racine.
En environnement de test, un dossier dédié `natheotheque-test` est utilisé à
la place.

### Routes principales

| Route | Méthode | Description |
|---|---|---|
| `admin_media_index` | GET | Page d'accueil de la médiathèque |
| `admin_media_load_medias` | GET | Contenu d'un dossier + données d'init (ajax) |
| `admin_media_save_folder` | POST | Crée ou renomme un dossier (ajax) |
| `admin_media_upload` | POST | Ajoute un média (ajax, fichier en base64) |
| `admin_media_save_media_edit` | POST | Sauvegarde nom/description d'un média (ajax) |
| `admin_media_liste_move` | GET | Arborescence des dossiers pour le déplacement (ajax) |
| `admin_media_move` | POST | Déplace un média ou un dossier (ajax) |
| `admin_media_update_trash` | POST | Met à la corbeille / restaure un élément, en cascade sur ses descendants (ajax) |
| `admin_media_nb_trash` | GET | Nombre d'éléments dans la corbeille (ajax) |
| `admin_media_list_trash` | GET | Contenu de la corbeille (ajax) |
| `admin_media_remove` | POST | Supprime définitivement un élément de la corbeille (ajax) — refuse tout élément dont `trash` n'est pas déjà à `true` |

## Fixtures

| | |
|---|---|
| Fichiers de données | `src/DataFixtures/data/content/media/media_folder_fixtures_data.yaml`, `media_fixtures_data.yaml` |
| Classes de fixture | [`MediaFolderFixtures`](https://github.com/counteraccro/natheo/blob/master/src/DataFixtures/Admin/Content/Media/MediaFolderFixtures.php), [`MediaFixtures`](https://github.com/counteraccro/natheo/blob/master/src/DataFixtures/Admin/Content/Media/MediaFixtures.php) |
| Groupes | `media`, `content` |

```bash
php bin/console doctrine:fixtures:load --group=media
```

Les fichiers sources des fixtures sont copiés depuis `public/assets/fixtures/`
vers le dossier réel de la médiathèque
(`MediaService::moveMediaFixture()`), puis le média est mis à jour à partir
du fichier copié (extension, taille, miniature…) via
`MediaService::UpdateMediaFile()`.

## Frontend

| Composant | Rôle |
|---|---|
| `Admin/Content/Media/Mediatheque` (`assets/vue/controllers/Admin/Content/Media/Mediatheque.vue`) | Composant racine, monté via `vue_component()` ; gère le chargement, le fil d'Ariane, le panneau latéral et la corbeille |
| `MediasGrid` | Grille/tableau des dossiers et médias d'un dossier (vues grille et liste) |
| `MediasBreadcrumb` | Fil d'Ariane |
| `MediaInfo` | Panneau [Information](medias.md#informations-dun-média) |
| `MediaEdit` | Panneau [édition d'un média](medias.md#modifier-un-média) ou [création/renommage d'un dossier](dossiers.md) |
| `MediaMove` | Panneau [déplacement](deplacer_et_corbeille.md#déplacer-un-média-ou-un-dossier) |
| `MediaNew` | Panneau [ajout d'un média](medias.md#ajouter-un-média), englobe `FileUpload` |
| `FileUpload` (`Components/Global/FileUpload.vue`) | Zone de dépôt de fichier générique (drag & drop + validation taille/type côté client) ; à ce jour utilisée uniquement par `MediaNew` |
| `MediasTrash` | Grille des éléments de la [corbeille](deplacer_et_corbeille.md#la-corbeille) |

Ce module n'utilise **pas** le composant `GenericGrid` partagé (voir
[Tableau GRID](../../../Architecture/composants/grid.md)) : `MediasGrid` est
une implémentation dédiée, propre à la médiathèque.

### Dépendance croisée : l'éditeur Markdown

Un second composant, distinct de tout ce qui précède,
[`Mediatheque.vue`](https://github.com/counteraccro/natheo/blob/master/assets/vue/Components/Global/MarkdownEditor/Mediatheque.vue)
(module `MediaModule` de l'
[éditeur Markdown](../../../Architecture/composants/editeur_markdown.md)),
réutilise la route `admin_media_load_medias` en lecture seule pour permettre
de parcourir la médiathèque et insérer le chemin web d'un média existant dans
le texte édité. Il ne permet ni d'uploader, ni de créer un dossier, ni
d'éditer quoi que ce soit — uniquement de naviguer et sélectionner.

## Traductions

Toutes les chaînes (labels, messages de confirmation, aide) sont dans le
domaine de traduction `media` : `translations/media+intl-icu.{fr,en,es}.yaml`.
