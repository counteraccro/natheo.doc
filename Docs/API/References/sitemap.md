---
title: "Sitemap"
parent: "API"
nav_order: 12
---

Renvoie une entrée par page **publiée** et **active**, et par langue, pour générer le sitemap du site. Les pages sont triées de
la plus récemment modifiée à la plus ancienne.

**Requête CURL**
`````shell
curl --request GET \
--url '[url-de-mon-site]/api/v1/sitemap' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer [mon-token]'
`````

**Réponse 200**
````json
{
  "code_http": 200,
  "message": "success",
  "data": [
    {
      "loc": "/fr/page/home",
      "priority": "1.00",
      "lastmod": "2026-10-03T20:58:10+00:00"
    },
    {
      "loc": "/en/page/home",
      "priority": "1.00",
      "lastmod": "2026-10-03T20:58:10+00:00"
    },
    {
      "loc": "/es/page/home",
      "priority": "1.00",
      "lastmod": "2026-10-03T20:58:10+00:00"
    },
    {
      "loc": "/fr/blog/blog",
      "priority": "1.00",
      "lastmod": "2026-10-03T20:58:10+00:00"
    }
  ]
}
````

| Champ | Description |
|---|---|
| `loc` | Chemin relatif de la page : `/{locale}/{catégorie}/{slug}` (à préfixer par l'URL de votre front) |
| `priority` | Toujours `"1.00"` |
| `lastmod` | Date de dernière modification de la page, au format ISO 8601 |

Le segment `{catégorie}` est le **slug** de la catégorie de la page, sans accent ni majuscule, identique dans toutes
les langues : `page`, `article`, `projet`, `blog`, `evenement`, `news`, `evolution`, `documentation`, `faq` (par
exemple `/en/blog/...`, `/es/blog/...`). C'est aussi une valeur acceptée par le paramètre `category` du
[listing par catégorie](listing_pages_category.md).

Les erreurs communes (jeton invalide, API fermée) sont décrites dans la [présentation de l'API](../index.md#format-des-réponses).

## Voir aussi
- [Find page](find_page.md)
- [Références globales](../../Architecture/references_globales.md#catégorie-de-page)
