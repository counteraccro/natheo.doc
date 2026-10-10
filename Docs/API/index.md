---
title: "API"
nav_icon: "m3.75 13.5 10.5-11.25L12 10.5h8.25L9.75 21.75 12 13.5H3.75Z"
has_children: true
nav_order: 7
---

Nathéo CMS est un CMS *headless* : le contenu saisi dans le back-office (pages, menus, FAQ, commentaires, options)
est mis à disposition d'un front externe au travers d'une API JSON. Cette page décrit les règles communes à tous
les endpoints ; le détail de chacun est listé dans le menu à gauche.

## URL de base

Toutes les routes sont préfixées par `/api/{version}/`. La seule version existante est **`v1`** (paramètre
`app.api_version` dans `config/services.yaml`) ; toute autre version renvoie une erreur `404`.

```
[url-de-mon-site]/api/v1/...
```

| Endpoint | Méthode | Rôle |
|---|---|---|
| [`/authentication`](References/authentication.md) | GET | Tester un jeton API |
| [`/authentication/user`](References/user_authentication.md) | POST | Connecter un utilisateur et obtenir un token utilisateur |
| [`/menu/find`](References/find_menu.md) | GET | Récupérer un menu |
| [`/page/find`](References/find_page.md) | GET | Récupérer une page (et ses menus) |
| [`/page/content`](References/find_page_content.md) | GET | Récupérer le contenu d'un bloc de page |
| [`/page/category`](References/listing_pages_category.md) | GET | Lister les pages d'une catégorie |
| [`/page/tag`](References/listing_pages_tags.md) | GET | Lister les pages d'un tag |
| [`/comment/page`](References/comment_by_page.md) | GET | Lister les commentaires d'une page |
| [`/comment`](References/add_comment.md) | POST | Ajouter un commentaire |
| [`/comment/moderate/{id}`](References/moderate_comment.md) | PUT | Modérer un commentaire |
| [`/options-systems`](References/option_system.md) | GET | Lire les options système publiques |
| [`/sitemap`](References/sitemap.md) | GET | Liste des pages publiées pour le sitemap |

Les routes `/authentication`, `/comment`, `/options-systems` et `/sitemap` acceptent indifféremment un slash final
(`/sitemap` ou `/sitemap/`).

## Authentification par jeton API

Chaque appel doit porter un **jeton API** dans l'en-tête `Authorization`, préfixé exactement par `Bearer ` :

```shell
curl --request GET \
--url '[url-de-mon-site]/api/v1/authentication/' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer [mon-token]'
```

Les jetons se créent dans le back-office, voir [Jetons API](../GuideAdmin/Systeme/ApiToken/jetons_api.md). Un
appel est accepté si **toutes** les conditions suivantes sont remplies :

| Condition | Sinon |
|---|---|
| L'option système **Ouvrir l'API ?** est activée ([options système](../GuideAdmin/Systeme/options.md#api)) | `403` « Ressource non accessible - API fermée » |
| L'IP de l'appelant fait partie de `app.ip_api_authorize` (si `app.ip_api_active_filter` vaut `true`) | `401` « Token Invalide » |
| Le jeton existe, est actif et n'est pas expiré | `401` « Token Invalide » |
| Un en-tête `Authorization: Bearer ...` est présent | `401` « Full authentication is required to access this resource. » |

Le filtrage par IP se règle dans `config/services.yaml` :

```yaml
parameters:
    app.ip_api_active_filter: true # Si true alors filtre l'accès à l'API par les IP défini dans app.ip_api_authorize
    app.ip_api_authorize: [127.0.0.1, ::1]
```

Par défaut, seules les IP locales sont autorisées : ajoutez l'IP du serveur qui héberge votre front avant la mise
en production. Si le CMS est derrière un **reverse proxy**, déclarez-le dans la variable d'environnement
`SYMFONY_TRUSTED_PROXIES` (exemple commenté dans `.env`) : sinon c'est l'IP du proxy, et non celle du client, qui est
comparée à la liste.

> 📝 L'ouverture de l'API (**Ouvrir l'API ?**) est indépendante de l'ouverture du site au public (**Ouvrir le site au
> public ?**) : un site fermé peut continuer à servir son API, et inversement.

### Rôles des jetons

Les routes en lecture n'exigent que le rôle *Lecture* ; les routes qui **écrivent** exigent le rôle *Lecture +
Écriture* (`ROLE_WRITE_API`) ou *Admin*, qui en hérite :

| Endpoint | Rôle minimum du jeton |
|---|---|
| Tous les `GET`, et [Authentification utilisateur](References/user_authentication.md) | `ROLE_READ_API` |
| [Ajouter un commentaire](References/add_comment.md), [Modérer un commentaire](References/moderate_comment.md) | `ROLE_WRITE_API` |

Un jeton qui n'a pas le rôle requis reçoit un `403` :

````json
{
  "code_http": 403,
  "message": "Ressource non accessible",
  "errors": [
    "Les droits du token API ne permettent pas d'accéder à cette ressource"
  ]
}
````

> 📝 Les paramètres sont contrôlés **avant** le rôle du jeton : une requête mal formée envoyée avec un jeton
> *Lecture* reçoit d'abord l'erreur `400` du paramètre, et seulement ensuite, une fois corrigée, le `403`.

`ROLE_ADMIN_API` n'apporte aujourd'hui rien de plus que `ROLE_WRITE_API`.

## Token utilisateur (`User-Token`)

Certains endpoints changent de comportement lorsqu'un **utilisateur du back-office** est identifié. Il faut
d'abord obtenir un token utilisateur via [Authentification utilisateur](References/user_authentication.md),
puis l'envoyer dans l'en-tête `User-Token`, **en plus** du jeton API :

```shell
--header 'Authorization: Bearer [mon-token]' \
--header 'User-Token: [user-token]'
```

| Endpoint | Effet du `User-Token` |
|---|---|
| [Find page](References/find_page.md) | Un Contributeur (ou plus) voit aussi les pages en **brouillon** |
| [Commentaires d'une page](References/comment_by_page.md) | Un Contributeur (ou plus) voit le texte des commentaires en attente / modérés et le motif de modération |
| [Modérer un commentaire](References/moderate_comment.md) | **Obligatoire** : identifie le modérateur |
| [Find page content](References/find_page_content.md) | Un Contributeur (ou plus) peut lire les blocs des pages en **brouillon** |
| [Find menu](References/find_menu.md), [Listings](References/listing_pages_category.md) | Vérifié s'il est présent, mais **sans effet** sur le résultat |

Si l'en-tête `User-Token` est présent mais invalide, expiré, ou que le compte a été désactivé entre-temps, l'appel
est refusé :

````json
{
  "code_http": 403,
  "message": "Ressource non accessible",
  "errors": [
    "Utilisateur non trouvé"
  ]
}
````

## Format des réponses

Toutes les réponses sont en JSON et suivent la même enveloppe :

````json
{
  "code_http": 200,
  "message": "success",
  "data": { }
}
````

En cas d'erreur, `data` disparaît et la clé `errors` liste le ou les messages détaillés ; `message` donne la
catégorie de l'erreur :

| `code_http` | `message` |
|---|---|
| `400` | « Requête invalide » : paramètre absent, vide, de mauvais type ou hors limites, corps JSON invalide |
| `401` | « Accès non autorisé » : jeton absent ou refusé, identifiants utilisateur faux |
| `403` | « Ressource non accessible » : API fermée, rôle insuffisant, token utilisateur refusé, commentaires fermés |
| `404` | « Ressource non disponible » : route, page, bloc, menu, catégorie, tag, option ou commentaire introuvable |
| `405` | « Méthode HTTP non autorisée » : la route existe mais pas avec cette méthode |
| `429` | « Trop de tentatives, veuillez réessayer plus tard » : limite de fréquence dépassée sur la [connexion utilisateur](References/user_authentication.md) ou l'[ajout de commentaire](References/add_comment.md) |
| `500` | « Erreur serveur interne » |

````json
{
  "code_http": 400,
  "message": "Requête invalide",
  "errors": [
    "Le paramètre page doit être supérieur à 0",
    "Le paramètre limit doit être compris entre 1 et 100"
  ]
}
````

Lorsque plusieurs paramètres sont invalides, `errors` contient **un message par erreur**.

Les erreurs ci-dessous peuvent être renvoyées par **tous** les endpoints ; elles ne sont pas répétées dans chaque
page :

*Jeton API invalide, expiré, désactivé, ou IP non autorisée*
````json
{
  "code_http": 401,
  "message": "Accès non autorisé",
  "errors": [
    "Token Invalide"
  ]
}
````

*API fermée (option « Ouvrir l'API ? » désactivée)*
````json
{
  "code_http": 403,
  "message": "Ressource non accessible",
  "errors": [
    "Ressource non accessible - API fermée"
  ]
}
````

*Route inexistante*
````json
{
  "code_http": 404,
  "message": "Ressource non disponible",
  "errors": [
    "No route found for \"GET http://mon-site.fr/api/v1/nimporte\""
  ]
}
````

Une erreur interne inattendue renvoie un `500` « Erreur serveur interne » ; le détail est écrit dans les
[logs](../GuideAdmin/Systeme/logs.md). Le message technique n'est ajouté dans `errors` que si le CMS tourne en
mode debug (`APP_DEBUG=1`), jamais en production.

> 📝 Le détail des erreurs produites par Symfony lui-même (route inexistante, méthode non autorisée, en-tête
> `Authorization` absent) reste en anglais.

## Langues

Les endpoints de contenu acceptent un paramètre `locale` : `fr` (par défaut), `en` ou `es`. Toute autre valeur
est refusée :

````json
{
  "code_http": 400,
  "message": "Requête invalide",
  "errors": [
    "Choisir une locale entre fr (français) ou es (espagnol) ou en (anglais)"
  ]
}
````

## Pagination

Les endpoints paginés (listings de pages, bloc Listing, commentaires) acceptent `page` (à partir de 1) et `limit`
(de **1 à 100**, 25 par défaut). Une valeur hors limites renvoie un `400`, par exemple :

````json
{
  "code_http": 400,
  "message": "Requête invalide",
  "errors": [
    "Le paramètre limit doit être compris entre 1 et 100"
  ]
}
````

`page=0` et `limit=0` sont considérés comme absents et remplacés par les valeurs par défaut.

## Langue des messages

Les messages d'erreur du CMS sont toujours en français (langue par défaut du CMS), quelle que soit la `locale`
demandée.

## Voir aussi
- [Jetons API](../GuideAdmin/Systeme/ApiToken/jetons_api.md) : création et gestion des jetons
- [Références globales](../Architecture/references_globales.md) : signification des valeurs numériques renvoyées
  (rendu, type de menu, position…)
- [Options système](../GuideAdmin/Systeme/options.md#api) : options qui influencent l'API
