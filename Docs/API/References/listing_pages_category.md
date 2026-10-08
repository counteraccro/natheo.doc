---
title: "Listing pages par catégorie"
parent: "API"
nav_order: 9
---

Renvoie la liste paginée des pages **publiées** et **actives** d'une catégorie, de la plus récemment modifiée à la
plus ancienne.

Paramètres attendus (query string) :

| nom      | type    | obligatoire | valeur par défaut | commentaire                                  |
|----------|---------|-------------|-------------------|----------------------------------------------|
| category | String  | OUI         |                   | **Libellé** de la catégorie (voir ci-dessous) |
| locale   | String  | NON         | fr                | Langue des titres et slugs renvoyés          |
| page     | Integer | NON         | 1                 | Page de résultats                            |
| limit    | Integer | NON         | 25                | Nombre de pages par page de résultats (1 à 100) |

Le paramètre `category` n'est pas l'identifiant numérique de la catégorie mais son **nom**, comparé sans tenir
compte de la casse ni des accents. Deux formes sont acceptées :

| Catégorie | Slug (recommandé) | Libellé français également accepté |
|---|---|---|
| Page | `page` | `Page` |
| Article | `article` | `Article` |
| Projet | `projet` | `Projet` |
| Blog | `blog` | `Blog` |
| Évènement | `evenement` | `Évènement`, `évènement` |
| News | `news` | `Nouveauté`, `nouveaute` |
| Évolution | `evolution` | `Evolution` |
| Documentation | `documentation` | `Documentation` |
| FAQ | `faq` | `FAQ` |

Le slug est celui utilisé dans les URL du [sitemap](sitemap.md). Les libellés anglais ou espagnols ne sont pas
reconnus (`project` est refusé), même quand `locale` vaut `en` ou `es`. Voir aussi les
[références globales](../../Architecture/references_globales.md#catégorie-de-page).

> 📝 Le `User-Token` est vérifié s'il est envoyé, mais il est **sans effet** : les brouillons ne sont jamais
> listés.

**Requêtes CURL**
`````shell
curl --request GET \
--url '[url-de-mon-site]/api/v1/page/category?category=blog' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer [mon-token]'
`````

`````shell
curl --request GET \
--url '[url-de-mon-site]/api/v1/page/category?category=evenement&locale=en&page=2&limit=1' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer [mon-token]'
`````

**Réponse 200**

url : `[url-de-mon-site]/api/v1/page/category?category=blog&limit=2`
````json
{
  "code_http": 200,
  "message": "success",
  "data": {
    "pages": [
      {
        "title": "Mon blog",
        "slug": "blog",
        "author": "user.demo@mail.fr",
        "created": 1791061090,
        "update": 1791061090
      },
      {
        "title": "Blog - 1 bloc",
        "slug": "blog-1-bloc",
        "author": "user.demo@mail.fr",
        "created": 1791061090,
        "update": 1791061090
      }
    ],
    "limit": 2,
    "current_page": 1,
    "rows": 4
  }
}
````

| Champ | Description |
|---|---|
| `pages` | Pages de la page de résultats : titre et slug dans la `locale`, auteur, dates de création et de modification (timestamp Unix) |
| `limit` / `current_page` | Pagination appliquée |
| `rows` | Nombre **total** de pages de la catégorie |

Une catégorie existante mais sans page publiée renvoie un `200` avec `pages` vide et `rows` à `0`.

**Réponse 404**

*Si la catégorie n'existe pas*
````json
{
  "code_http": 404,
  "message": "Ressource non disponible",
  "errors": [
    "La catégorie recherchée n'existe pas"
  ]
}
````

**Réponse 400**

*Si `category` est absent ou vide*
````json
{
  "code_http": 400,
  "message": "Requête invalide",
  "errors": [
    "Cette valeur ne doit pas être vide."
  ]
}
````

**Réponse 403**

*Si le `User-Token` est présent mais invalide ou expiré*
````json
{
  "code_http": 403,
  "message": "Ressource non accessible",
  "errors": [
    "Utilisateur non trouvé"
  ]
}
````

Les erreurs communes (jeton invalide, API fermée, locale invalide, `limit` hors limites) sont décrites dans la
[présentation de l'API](../index.md#format-des-réponses).

## Voir aussi
- [Listing pages par tag](listing_pages_tags.md)
- [Find page content](find_page_content.md) (bloc Listing)
- [Find page](find_page.md)
