---
title: "Find page content"
parent: "API"
nav_order: 5
---

Renvoie le contenu d'un bloc de page, à partir de l'`id` donné par la liste `contents` de [Find page](find_page.md).
Le format de `content` dépend du type du bloc :

| Type de bloc | `content` contient |
|---|---|
| `1` Texte | Le texte du bloc **converti en HTML** (Markdown, liens internes `P#id` résolus) |
| `2` FAQ | La FAQ liée : titre, catégories actives et leurs questions actives, réponses en **Markdown** |
| `3` Listing | La liste paginée des pages publiées de la catégorie liée au bloc |

Pour plus d'information sur les types de bloc, voir les
[références globales](../../Architecture/references_globales.md#type-de-contenu-de-page).

Paramètres attendus (query string) :

| nom    | type    | obligatoire | valeur par défaut | commentaire                                   |
|--------|---------|-------------|-------------------|-----------------------------------------------|
| id     | Integer | OUI         |                   | Id du bloc de contenu                         |
| locale | String  | NON         | fr                | Langue du contenu renvoyé                     |
| page   | Integer | NON         | 1                 | Page de résultats (bloc Listing uniquement)   |
| limit  | Integer | NON         | 25                | Nombre de pages par page de résultats, de 1 à 100 (bloc Listing uniquement) |

Le bloc n'est renvoyé que si la page qui le contient est **publiée** et **active**. Avec un `User-Token` valide
d'un compte **Contributeur** ou plus, les blocs des pages en **brouillon** sont aussi accessibles (même règle que
[Find page](find_page.md)).

**Requêtes CURL**
`````shell
curl --request GET \
--url '[url-de-mon-site]/api/v1/page/content?id=32' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer [mon-token]'
`````

`````shell
curl --request GET \
--url '[url-de-mon-site]/api/v1/page/content?id=5&locale=en&page=2&limit=2' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer [mon-token]'
`````

**Réponse 200**

*Bloc de type Texte* — url : `[url-de-mon-site]/api/v1/page/content?id=32`
````json
{
  "code_http": 200,
  "message": "success",
  "data": {
    "id": 32,
    "content": "<h1>Teneo ad saetigeri nocte</h1>\n<p>Lorem markdownum ipse supplex subito, versus eadem, hastam nocebant te\n<strong>piscem</strong>. Haec simulacra ut quae.</p>\n"
  }
}
````

**Réponse 200**

*Bloc de type FAQ* — url : `[url-de-mon-site]/api/v1/page/content?id=2`
````json
{
  "code_http": 200,
  "message": "success",
  "data": {
    "id": 2,
    "content": {
      "title": "FAQ de démonstration",
      "categories": [
        {
          "title": "Curvique fronte",
          "questions": [
            {
              "title": "Pati utque in petuntur",
              "answer": "# Elusaque facit tibi illa\n\n## Secum currunt cupido Tartessia\n\nLorem markdownum, *tamen tibi ostendit* tauri vincemus..."
            }
          ]
        }
      ]
    }
  }
}
````

> 📝 Contrairement au bloc Texte, les réponses de FAQ (`answer`) sont renvoyées en **Markdown** (seuls les liens
> internes `P#id` sont résolus) : c'est au front de les convertir en HTML.

**Réponse 200**

*Bloc de type Listing* — url : `[url-de-mon-site]/api/v1/page/content?id=5&limit=2`
````json
{
  "code_http": 200,
  "message": "success",
  "data": {
    "id": 5,
    "content": {
      "pages": [
        {
          "img": "http://dev.natheo:8888/assets/natheotheque//medias/paysage/railroad.jpg",
          "title": "Mon blog",
          "slug": "blog",
          "category": 4,
          "author": "user.demo@mail.fr",
          "created": 1791061090,
          "update": 1791061090
        },
        {
          "img": "http://dev.natheo:8888/assets/natheotheque//medias/paysage/the-desert.png",
          "title": "Blog - 1 bloc",
          "slug": "blog-1-bloc",
          "category": 4,
          "author": "user.demo@mail.fr",
          "created": 1791061090,
          "update": 1791061090
        }
      ],
      "limit": 2,
      "current_page": 1,
      "rows": 4
    },
    "title": "Liste des pages pour la catégorie \"Blog\""
  }
}
````

Pour un bloc Listing :

| Champ | Description |
|---|---|
| `content.pages` | Pages publiées et actives de la catégorie, de la plus récemment modifiée à la plus ancienne : image d'en-tête, titre et slug dans la `locale`, id de catégorie, auteur, dates (timestamp Unix) |
| `content.limit` / `content.current_page` | Pagination appliquée |
| `content.rows` | Nombre **total** de pages de la catégorie (toutes pages de résultats confondues) |
| `title` | Titre du listing, dans la `locale` demandée |

> 📝 La catégorie listée, et reprise dans le `title`, est celle **choisie dans le bloc** (et non celle de la page
> qui le contient). `page=0` ou `limit=0` sont remplacés par les valeurs par défaut (1 et 25).

> ⚠️ Le nom de la catégorie dans `title` reste en français quelle que soit la `locale`. De plus, les traductions
> anglaise et espagnole du titre ne sont pas encore renseignées dans le CMS : avec `locale=en` ou `es`, `title` vaut
> littéralement `__page.content.listing.title`.

**Réponse 400**

*Si `limit` est supérieur à 100 (ou `page` négatif)*
````json
{
  "code_http": 400,
  "message": "Requête invalide",
  "errors": [
    "Le paramètre limit doit être compris entre 1 et 100"
  ]
}
````

**Réponse 404**

*Si le bloc n'existe pas, si `id` est absent, si sa page n'est pas visible, ou si la FAQ liée est désactivée*
````json
{
  "code_http": 404,
  "message": "Ressource non disponible",
  "errors": [
    "Contenu de page non disponible"
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

Les erreurs communes (jeton invalide, API fermée, locale invalide) sont décrites dans la
[présentation de l'API](../index.md#format-des-réponses).

## Voir aussi
- [Find page](find_page.md)
- [Listing pages par catégorie](listing_pages_category.md)
- [Éditeur Markdown](../../GuideAdmin/Modules/editeur_markdown.md)
- [FAQ](../../GuideAdmin/Contenu/Faq/listing.md)
