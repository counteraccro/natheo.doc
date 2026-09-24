---
title: "Référence technique"
parent: "Recherche globale"
grand_parent: "Guide d'administration"
nav_order: 1
---

Vue d'ensemble technique de la recherche globale : routes, classes
backend/frontend et détail des requêtes par entité. Pour l'usage côté
back-office, voir la [page fonctionnelle](recherche_globale.md).

## Backend

| Classe | Rôle |
|---|---|
| [`GlobalSearchController`](https://github.com/counteraccro/natheo/blob/master/src/Controller/Admin/Global/GlobalSearchController.php) | Routes admin (`/admin/{_locale}/search/...`) |
| [`GlobalSearchService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/GlobalSearchService.php) | Aiguille la recherche vers le bon repository selon l'entité, formate les résultats et surligne les correspondances |
| [`GlobalSearchTranslate`](https://github.com/counteraccro/natheo/blob/master/src/Utils/Translate/GlobalSearchTranslate.php) | Construit le tableau de traductions passé au composant Vue `GlobalSearch` |

Le controller est protégé par `#[IsGranted('ROLE_USER')]` (le rôle minimal de
la hiérarchie) — c'est la présence conditionnelle du champ de recherche dans
`header.html.twig` (`is_granted('ROLE_CONTRIBUTEUR')`) qui restreint l'accès
en pratique, pas le controller lui-même.

### Routes

| Route | Méthode | Description |
|---|---|---|
| `admin_search_index` | POST | Page de résultats ; reçoit le terme recherché depuis le champ `global-search-input` du formulaire de l'en-tête |
| `admin_search_global` | GET | `/search/{entity}/{page}/{limit}/{search}` — recherche ajax pour une seule entité (`page`, `menu`, `faq`, `tag` ou `user`), appelée une fois par onglet. Le paramètre `{search}` a la contrainte `requirements: ['search' => '.+']`, censée accepter un `/` dans le critère (voir la limite côté serveur web ci-dessous) |

`GlobalSearchService::globalSearch()` résout d'abord `entity` (`getEntityByString()`)
vers le nom de classe complet de l'entité correspondante (`Page::class`,
`Menu::class`…), puis appelle
`getRepository(ucfirst($entity))->search($search, $locale, $page, $limit)` —
le `ucfirst()` est ici sans effet réel (le nom de classe complet commence déjà
par une majuscule), `getRepository()` n'étant qu'un raccourci vers
`EntityManager::getRepository()`. La méthode `search()` est donc **générique
par convention de nom** : chaque repository concerné doit l'implémenter avec
la même signature.

### Ce que recherche `search()` par repository

| Repository | Champs, condition de langue |
|---|---|
| [`PageRepository::search()`](https://github.com/counteraccro/natheo/blob/master/src/Repository/Admin/Content/Page/PageRepository.php) | Titre de page (langue courante) **ou** texte d'un bloc de type `TEXT` (langue courante) **ou** login de l'auteur (toutes langues) |
| [`MenuRepository::search()`](https://github.com/counteraccro/natheo/blob/master/src/Repository/Admin/Content/Menu/MenuRepository.php) | Nom du menu (non traduit) **ou** login de l'auteur (toutes langues) **ou** texte d'un lien du menu (langue courante) |
| [`FaqRepository::search()`](https://github.com/counteraccro/natheo/blob/master/src/Repository/Admin/Content/Faq/FaqRepository.php) | Titre de la FAQ, titre d'une catégorie, titre **ou** réponse d'une question (langue courante pour chacun) **ou** login de l'auteur (toutes langues) |
| [`TagRepository::search()`](https://github.com/counteraccro/natheo/blob/master/src/Repository/Admin/Content/Tag/TagRepository.php) | Libellé du tag (langue courante) |
| [`UserRepository::search()`](https://github.com/counteraccro/natheo/blob/master/src/Repository/Admin/System/UserRepository.php) | Login, e-mail, prénom, nom (paramètre `locale` reçu mais inutilisé, un utilisateur n'étant pas traduit) |

> ⚠️ **Correction (2026-09-23)** : ces deux méthodes avaient été notées à
> tort comme du code mort lors de la rédaction initiale des domaines
> [Menus](../Contenu/Menus/technique.md) et [Faq](../Contenu/Faq/technique.md)
> (« jamais appelée par aucun controller/service »). Elles sont en réalité
> **le seul point d'entrée de la recherche globale** pour ces deux entités —
> la recherche transverse n'avait simplement pas été identifiée comme
> appelante lors de ces relectures. Corrigé dans les deux pages concernées.

### Formatage et mise en avant

`GlobalSearchService::formatResult()` transforme chaque entité trouvée en un
tableau `{id, label, contents[], date, author?, urls}`, spécifique par type
(`formatResulPage`, `formatResulMenu`, `formatResultFaq`, `formatResultTag`,
`formatResultUser`). Le terme recherché y est mis en évidence par
`highlightText()`, qui l'entoure d'un `<mark>` (insensible à la casse,
`str_ireplace`) — aussi bien dans le titre que dans les extraits de contenu et
le nom de l'auteur.

Pour les pages et la FAQ, les extraits de contenu ne sont pas le texte entier
mais une fenêtre d'environ 10 mots avant/après chaque occurrence (regex
`(?-i:\w+[^\w\n]+){0,10}` de part et d'autre du terme) ; pour les menus, cette
fenêtre est réduite à 1 mot avant/après, le texte d'un lien étant
généralement court. Le terme recherché est échappé via `preg_quote()` avant
d'être inséré dans ces motifs, pour qu'un caractère spécial d'expression
régulière (`.`, `(`, `+`…) dans le critère ne casse pas l'extraction plutôt
que d'être traité comme un opérateur regex.

Les tags et les utilisateurs n'ont pas d'extrait de contenu (`contents`
toujours vide) : seul le titre/libellé porte la mise en évidence.

## Frontend

| Composant | Rôle |
|---|---|
| `Admin/Global/GlobalSearch` (`assets/vue/controllers/Admin/Global/GlobalSearch.vue`) | Composant racine, monté via `vue_component()` sur `templates/admin/global_search/index.html.twig` ; lance les 5 recherches en parallèle au montage et gère les onglets |
| `Global/Search/TabSearchResult` (`assets/vue/Components/Global/Search/TabSearchResult.vue`) | Affiche la liste des fiches résultat d'un onglet |
| `Global/Search/SearchPaginate` (`assets/vue/Components/Global/Search/SearchPaginate.vue`) | Pagination propre à chaque onglet |
| `Skeleton/SearchResult` (`assets/vue/Components/Skeleton/SearchResult.vue`) | Squelette de chargement affiché pendant l'appel ajax d'un onglet |

Chaque onglet appelle indépendamment `GET admin_search_global` au montage du
composant (`mounted()` déclenche les 5 recherches) puis à chaque changement de
page (`changePage`). Un onglet dont le total de résultats est à 0 ne stocke
jamais de résultat (`results[entity]` reste `null`) : c'est ce qui détermine
l'absence de badge sur cet onglet, pas un badge à `0`.

## Traductions

Domaine `global_search` : `translations/global_search+intl-icu.{fr,en,es}.yaml`
(textes propres à la page et aux onglets). Le texte du champ de recherche
dans l'en-tête (`global.header.search.input`) appartient lui au domaine
`global` (`translations/global+intl-icu.{fr,en,es}.yaml`).

## Voir aussi
- [Page fonctionnelle](recherche_globale.md)
- [Tableau GRID](../../Architecture/composants/grid.md) (option `OU_NB_ELEMENT`)
