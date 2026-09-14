---
title: "Tableau GRID"
parent: "Composants"
grand_parent: "Architecture"
nav_order: 1
---

Composant générique de tableau paginé, trié et filtrable, utilisé pour tous
les listings de l'administration (Tags, Pages, FAQ, Menus, Commentaires,
Utilisateurs...). Côté Vue, il est composé de trois éléments :
[`GenericGrid.vue`](https://github.com/counteraccro/natheo/blob/master/assets/vue/controllers/Admin/GenericGrid.vue)
(orchestrateur : recherche, filtre, tri, pagination, appels ajax),
[`Grid.vue`](https://github.com/counteraccro/natheo/blob/master/assets/vue/Components/Grid/Grid.vue)
(rendu du `<table>` et des boutons d'action) et
[`GridPaginate.vue`](https://github.com/counteraccro/natheo/blob/master/assets/vue/Components/Grid/GridPaginate.vue)
(pagination + choix du nombre d'éléments par page). Côté serveur, il
s'appuie sur [`GridService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/GridService.php),
que chaque service métier vient compléter.

![Exemple de grid](../files/exemple_grid.png)
*Listing des tags, un exemple parmi d'autres domaines utilisant le grid générique*

## Utilisation de base

Dans le template Twig du listing, on instancie le composant Vue :

{% raw %}
```twig
<div {{ vue_component('Admin/GenericGrid', {
    'url' : path('admin_tag_load_grid_data', {'page' : page, 'limit' : limit}),
    'page' : page,
    'limit' : limit,
    'activeSearchData' : true
}) }}></div>
```
{% endraw %}

| Prop | Type | Défaut | Rôle |
|---|---|---|---|
| `url` | `String` | — | Route ajax qui renvoie les données du grid (voir [Controller](#controller)) |
| `page` | `Number` | — | Page initiale affichée |
| `limit` | `String\|Number` | — | Nombre d'éléments par page initial |
| `activeSearchData` | `Boolean` | `false` | Active le bouton de recherche en base (mode "bdd", voir [Recherche](#recherche)) |
| `showFilter` | `Boolean` | `false` | Affiche le bouton de filtre "Mes données / Toutes les données" (voir [Filtre](#filtre)) |

`page` et `limit` sont fournis par le controller `index()` du domaine
(généralement `page = 1` et `limit` la valeur de l'option utilisateur
`OU_NB_ELEMENT`).

## Mise en place côté serveur

### Controller

Une route ajax dédiée renvoie les données au format attendu par le composant :

```php
/**
 * Charge le tableau grid de tag en ajax
 * @param TagService $tagService
 * @param Request $request
 * @param int $page
 * @param int $limit
 * @return JsonResponse
 */
#[Route('/ajax/load-grid-data/{page}/{limit}', name: 'load_grid_data', methods: ['GET'])]
public function loadGridData(TagService $tagService, Request $request, int $page = 1, int $limit = 20): JsonResponse
{
    $queryParams = [
        'search' => $request->query->get('search'),
        'orderField' => $request->query->get('orderField'),
        'order' => $request->query->get('order'),
        'locale' => $request->getLocale(),
    ];

    $grid = $tagService->getAllFormatToGrid($page, $limit, $queryParams);
    return $this->json($grid);
}
```

`page` et `limit` sont portés par l'URL elle-même (`GenericGrid.vue` les
ajoute en suffixe à chaque appel), tandis que `search`, `orderField`,
`order` et `filter` arrivent en query string.

### Service

Le service métier construit le tableau attendu par `GridService::addAllDataRequiredGrid()` :

```php
/**
 * Construit le tableau de donnée à envoyer au tableau GRID
 * @param int $page
 * @param int $limit
 * @param array $queryParams
 * @return array
 */
public function getAllFormatToGrid(int $page, int $limit, array $queryParams): array
{
    $translator = $this->getTranslator();
    $gridService = $this->getGridService();

    // Entêtes du tableau, dans l'ordre d'affichage des colonnes
    $column = [
        $translator->trans('tag.grid.id', domain: 'tag'),
        $translator->trans('tag.grid.label', domain: 'tag'),
        $translator->trans('tag.grid.color', domain: 'tag'),
        $translator->trans('tag.grid.created_at', domain: 'tag'),
        $translator->trans('tag.grid.update_at', domain: 'tag'),
        GridService::KEY_ACTION,
    ];

    $dataPaginate = $this->getAllPaginate($page, $limit, $queryParams);

    $nb = $dataPaginate->count();
    $data = [];
    foreach ($dataPaginate as $element) {
        /* @var Tag $element */

        // Chaque ligne doit avoir exactement les mêmes clés que $column
        $data[] = [
            $translator->trans('tag.grid.id', domain: 'tag') => $element->getId(),
            $translator->trans('tag.grid.label', domain: 'tag') => $element->getLabel(),
            $translator->trans('tag.grid.color', domain: 'tag') => $element->getColor(),
            $translator->trans('tag.grid.created_at', domain: 'tag') => $element->getCreatedAt()->format('d/m/y H:i'),
            $translator->trans('tag.grid.update_at', domain: 'tag') => $element->getUpdateAt()->format('d/m/y H:i'),
            GridService::KEY_ACTION => $this->generateTabAction($element),
            'isDisabled' => $element->isDisabled(),
        ];
    }

    $tabReturn = [
        GridService::KEY_NB => $nb,
        GridService::KEY_DATA => $data,
        GridService::KEY_COLUMN => $column,
        GridService::KEY_RAW_SQL => $gridService->getFormatedSQLQuery($dataPaginate),
        GridService::KEY_LIST_ORDER_FIELD => [
            'id' => $translator->trans('tag.grid.id', domain: 'tag'),
            'label' => $translator->trans('tag.grid.label', domain: 'tag'),
            'color' => $translator->trans('tag.grid.color', domain: 'tag'),
            'createdAt' => $translator->trans('tag.grid.created_at', domain: 'tag'),
            'updateAt' => $translator->trans('tag.grid.update_at', domain: 'tag'),
        ],
    ];
    return $gridService->addAllDataRequiredGrid($tabReturn);
}
```

`$dataPaginate` provient d'un `Paginator` Doctrine classique (repository
`getAllPaginate($page, $limit, $queryParams)`), qui applique lui-même le
tri (`orderField`/`order`) et la recherche (`search`) sur la requête SQL —
c'est ce `Paginator` que `getFormatedSQLQuery()` transforme ensuite en SQL
lisible pour l'aperçu de requête.

`ligne['isDisabled']` est optionnelle : si présente et à `true`, `Grid.vue`
affiche la ligne grisée (hors colonne action).

### GridService

[`GridService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/GridService.php)
centralise ce qui est commun à tous les grids :

| Méthode / constante | Rôle |
|---|---|
| `addAllDataRequiredGrid(array $tab)` | À appeler en dernier : ajoute l'URL de sauvegarde SQL, les choix de pagination et les traductions natives du composant |
| `addOptionsSelectLimit(array $tab)` | Ajoute la clé `listLimit` (choix `5 / 10 / 20 / 50 / 100`) |
| `getFormatedSQLQuery(Paginator $paginator)` | Reconstruit la requête SQL exécutée, paramètres inclus, pour l'aperçu "Afficher le SQL" |
| `renderRole(string $role)` | Traduit un rôle Symfony (`ROLE_ADMIN`...) en libellé affichable, pour les grids qui listent des utilisateurs |
| `KEY_NB`, `KEY_DATA`, `KEY_COLUMN`, `KEY_ACTION` | Clés obligatoires du tableau de retour |
| `KEY_RAW_SQL`, `KEY_LIST_ORDER_FIELD` | Clés optionnelles : requête SQL et colonnes triables |

## Colonnes triables

Cliquer sur l'en-tête d'une colonne déclenche un tri, à condition que son
champ Doctrine soit déclaré dans `GridService::KEY_LIST_ORDER_FIELD` — un
tableau `champ Doctrine => libellé affiché` :

```php
GridService::KEY_LIST_ORDER_FIELD => [
    'id' => $translator->trans('tag.grid.id', domain: 'tag'),
    'label' => $translator->trans('tag.grid.label', domain: 'tag'),
],
```

Le champ cliqué est renvoyé au serveur via `orderField` et `order`
(`ASC`/`DESC`), que le repository applique à la requête. Sans cette clé,
aucune colonne n'est triable — c'est aussi cette liste qui alimente le menu
déroulant "Plus d'options" du grid.

## Actions de ligne

`generateTabAction()` construit, pour chaque ligne, un tableau de boutons
placés dans la colonne `action` :

```php
private function generateTabAction(Tag $tag): array
{
    $actionDisabled = [
        'label' => ['M3.933 13.909A4.357...'], // 1 ou 2 chemins SVG (path d="...")
        'color' => 'primary',                   // classe bootstrap : primary, danger...
        'type' => 'put',                        // verbe HTTP de l'appel ajax
        'url' => $this->getRouter()->generate('admin_tag_update_disabled', ['id' => $tag->getId()]),
        'ajax' => true,
        'confirm' => true,                      // false ou absent = action directe, sans confirmation
        'msgConfirm' => $this->getTranslator()->trans('tag.confirm.disabled.msg', ['label' => $tag->getLabel()], 'tag'),
    ];

    // Bouton non-ajax : redirige simplement le navigateur vers `url`
    $actionEdit = [
        'label' => ['M10.779 17.779...'],
        'color' => 'primary',
        'type' => 'post',
        'url' => $this->getRouter()->generate('admin_tag_update', ['id' => $tag->getId()]),
        'ajax' => false,
    ];

    return [$actionDisabled, $actionEdit];
}
```

| Clé | Rôle |
|---|---|
| `label` | Tableau de 1 ou 2 chemins SVG (`d="..."`) affichés dans le bouton — pas d'icône de police, du path brut |
| `color` | Couleur du bouton (`primary`, `danger`...), appliquée en `btn-ghost-{color}` |
| `type` | Verbe HTTP de l'appel ajax (`put`, `post`, `delete`...), ignoré si `ajax` est à `false` |
| `url` | Route de l'action |
| `ajax` | `true` : appel en arrière-plan puis rechargement du grid. `false` : redirection classique du navigateur |
| `confirm` | Si `true`, ouvre une modale de confirmation avant d'exécuter l'action |
| `msgConfirm` | Message affiché dans la modale de confirmation (pris en compte seulement si `confirm` est à `true`) |

Chaque action ajax attend en retour un JSON `{'success': true|false, 'msg': '...'}`
(`Controller` classique) — `GenericGrid.vue` affiche `msg` dans un toast puis
recharge automatiquement les données de la page courante.

## Recherche

Deux modes, accessibles depuis le menu "..." à droite de la barre de
recherche :
- **Dans le tableau** (mode par défaut) : filtrage instantané, côté client,
  sur les données déjà chargées à l'écran (`Grid.vue`), sans appel serveur.
- **Dans la base de données** : envoie la saisie au serveur (`search` en
  query string) pour une recherche sur l'ensemble des données, pas
  seulement la page affichée. N'apparaît que si la prop `activeSearchData`
  est à `true`.

## Filtre

Quand `showFilter` est à `true`, un bouton bascule le filtre entre
`all` (toutes les données) et `me` (uniquement celles créées par
l'utilisateur connecté), envoyé en query string (`filter=me`). Le service
métier doit le lire lui-même côté controller — il n'y a pas de traitement
automatique :

```php
$filter = $request->query->get('filter');
$userId = null;
if ($filter === self::FILTER_ME) { // AppAdminController::FILTER_ME = 'me'
    $userId = $this->getUser()->getId();
}
$grid = $pageService->getAllFormatToGrid($page, $limit, $queryParams, $userId);
```

## Affichage de la requête SQL

Si `GridService::KEY_RAW_SQL` est renseigné dans le retour (voir
[Service](#service)), le menu "..." propose "Afficher le SQL" : la requête
Doctrine réellement exécutée (paramètres inclus) s'affiche avec coloration
syntaxique, copiable en un clic, et peut être enregistrée en tant que
requête personnalisée par un `ROLE_SUPER_ADMIN` (route
`admin_sql_manager_save_generic_query`, ajoutée automatiquement par
`GridService::addAllDataRequiredGrid()`) — voir le
[Gestionnaire SQL](../../GuideAdmin/Outils/gestionnaire_sql.md) *(page 🚧, à rédiger)*.

## Traductions

Tous les libellés natifs du composant (placeholder de recherche, textes de
confirmation, pagination...) sont déjà fournis par
[`GridTranslate`](https://github.com/counteraccro/natheo/blob/master/src/Utils/Translate/GridTranslate.php)
(domaine `grid`, fichiers `translations/grid+intl-icu.{fr,en,es}.yaml`) et
injectés automatiquement par `GridService::addAllDataRequiredGrid()`. Il n'y
a rien à faire côté domaine métier pour ces textes-là : seuls les libellés
de colonnes et les messages de confirmation d'actions restent à traduire
dans le domaine du composant concerné (`tag`, `page`...).
