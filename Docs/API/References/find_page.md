## Find menu

[Index](../../../index.md) > [API](../index.md) > Find page

Permet de renvoyer une page formatée en fonction de différents paramètres
Si le User-Token est présent dans le header et valide, permet de voir une page en brouillon

Si slug n'est pas précisé ou vide renvoi la landingPage si elle existe

Paramètres attendus :

| nom               | type    | obligatoire | valeur par défaut | commentaire                                                                         |
|-------------------|---------|-------------|-------------------|-------------------------------------------------------------------------------------|
| slug              | String  | NON         |                   | Obligatoire                                                                         |
| locale            | String  | NON         | fr                |                                                                                     |
| show_menus        | boolean | NON         | true              | remonte ou non les menus associés à la page                                         |
| show_tags         | boolean | NON         | true              | remonte ou non les tags associés à la page                                          |
| show_statistiques | boolean | NON         | true              | remonte ou non les statistiques associés à la page                                  |
| menu_position     | array   | NON         | 0                 | remonte uniquement les menus dans les positions demandés                            |


### Informations


**Requêtes CURL**
`````shell
curl --request GET \
--url --location '[url-de-mon-site]/api/v1/page/find?slug=bienvenue' \
--header 'Accept: application/json' \
--header 'User-Token: [user-token' \
--header 'Authorization: Bearer [mon-token]'
`````

`````shell
curl --request GET \
--url --location '[url-de-mon-site/api/v1/page/find?slug=bienvenue&locale=es&page=10&limit=250&show_menu=false&menu_positions=2%2C3%2C4' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer [mon-token]'
`````

**Réponse 200**

url : [url-de-mon-site]/api/v1/page/find?slug=bienvenue
````json
{
  "code_http": 200,
  "message": "success",
  "data": {
    "page": {
      "title": "Bienvenue sur NatheoCMS",
      "render": 6,
      "author": "user.demo@mail.fr",
      "created": 1730096624,
      "update": 1730096626,
      "tags": [
        {
          "label": "Natheo",
          "color": "#6F42C1"
        },
        {
          "label": "Article",
          "color": "#1188b4"
        },
        {
          "label": "Evolution",
          "color": "#b61114"
        }
      ],
      "statistiques": {
        "PAGE_NB_READ": "49"
      },
      "contents": [
        {
          "id": 92
        },
        {
          "id": 93
        },
        {
          "id": 94
        }
      ],
      "menus": {
        "LEFT": {
          "position": "LEFT",
          "type": 12,
          "elements": [
            {
              "target": "_blank",
              "label": "Pages",
              "url": "#",
              "slug": "",
              "elements": [
                {
                  "target": "_blank",
                  "label": "Listing des pages",
                  "url": "",
                  "slug": "pages"
                },
                {
                  "target": "_blank",
                  "label": "Les nouveautées",
                  "url": "",
                  "slug": "new-in-natheo-cms"
                },
                {
                  "target": "_blank",
                  "label": "Démonstration",
                  "url": "",
                  "slug": "demo-page"
                }
              ]
            }
          ]
        },
        "HEADER": {
          "position": "HEADER",
          "type": 3,
          "elements": [
            {
              "target": "_blank",
              "label": "Contenu",
              "url": "",
              "slug": "",
              "elements": [
                {
                  "target": "_blank",
                  "label": "Listing des pages",
                  "url": "",
                  "slug": "pages",
                  "elements": [
                    {
                      "target": "_blank",
                      "label": "Les nouveautées",
                      "url": "",
                      "slug": "new-in-natheo-cms"
                    },
                    {
                      "target": "_blank",
                      "label": "Démonstration",
                      "url": "",
                      "slug": "demo-page"
                    }
                  ]
                },
                {
                  "target": "_blank",
                  "label": "Liste des articles de blogs",
                  "url": "",
                  "slug": "blogs",
                  "elements": [
                    {
                      "target": "_blank",
                      "label": "Page de blog",
                      "url": "",
                      "slug": "article-blog"
                    }
                  ]
                }
              ]
            },
            {
              "target": "_blank",
              "label": "Documentation",
              "url": "https://counteraccro.github.io/natheo.doc/",
              "slug": ""
            },
            {
              "target": "_blank",
              "label": "Autre",
              "url": "#",
              "slug": "",
              "elements": [
                {
                  "target": "_blank",
                  "label": "Natheo",
                  "url": "#",
                  "slug": "",
                  "elements": [
                    {
                      "target": "_blank",
                      "label": "GitHub",
                      "url": "https://github.com/counteraccro/natheo",
                      "slug": ""
                    },
                    {
                      "target": "_blank",
                      "label": "Site officiel",
                      "url": "#",
                      "slug": ""
                    }
                  ]
                },
                {
                  "target": "_blank",
                  "label": "Partenaires",
                  "url": "https://www.google.fr/",
                  "slug": "",
                  "elements": [
                    {
                      "target": "_blank",
                      "label": "Natheo agency",
                      "url": "https://www.natheo-agency.fr/",
                      "slug": ""
                    },
                    {
                      "target": "_blank",
                      "label": "Natheo community",
                      "url": "https://www.natheo-community.fr/",
                      "slug": ""
                    },
                    {
                      "target": "_blank",
                      "label": "Natheo Book",
                      "url": "https://www.natheo-book.fr/",
                      "slug": ""
                    }
                  ]
                }
              ]
            }
          ]
        },
        "FOOTER": {
          "position": "FOOTER",
          "type": 17,
          "elements": [
            {
              "target": "_blank",
              "label": "Site officiel",
              "url": "https://www.google.fr",
              "slug": ""
            },
            {
              "target": "_blank",
              "label": "Documentation",
              "url": "https://github.com/counteraccro/natheo",
              "slug": ""
            },
            {
              "target": "_blank",
              "label": "GitHub",
              "url": "https://github.com/counteraccro/natheo",
              "slug": ""
            }
          ]
        }
      }
    }
  }
}
````
**Réponse 200**

url : [url-de-mon-site]/api/v1/page/find?slug=bienvenue&locale=es&show_menus=false&show_tags=false,&show_statistiques=false
````json
{
    "code_http": 200,
    "message": "success",
    "data": {
        "page": {
            "title": "[ES] Bienvenue sur NatheoCMS",
            "render": 6,
            "author": "user.demo@mail.fr",
            "created": 1730096624,
            "update": 1730096626,
            "contents": [
                {
                    "id": 92
                },
                {
                    "id": 93
                },
                {
                    "id": 94
                }
            ]
        }
    }
}
````


**Réponse 401**

*Si le token n'est pas valide*
````json
{
    "code_http": 401,
    "message": "Accès non autorisé",
    "errors": [
        "Token Invalide"
    ]
}
````

**Réponse 403**


*Si le paramètre menu_position n'est pas valide*
````json
{
  "code_http": 403,
  "message": "Ressource non accessible",
  "errors": [
    "Choisi une position entre 0 (tout) - 1 (haut) - 2 (droite) - 3 (bas) - 4 (gauche). Plusieurs choix possible "
  ]
}
````

*Si le User-Token est présent mais faux et/ou périmé*
````json
{
    "code_http": 403,
    "message": "Ressource non accessible",
    "errors": [
        "Utilisateur non trouvé"
    ]
}
````

*Si la page n'existe pas*
````json
{
    "code_http": 403,
    "message": "Ressource non accessible",
    "errors": [
        "Page non disponible"
    ]
}
````