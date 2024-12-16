## Find menu

[Index](../../../index.md) > [API](../index.md) > Find page content

Permet de renvoyer une le block d'une page formatée en fonction de différents paramètres


Pour plus d'information sur les références globales, [cliquez ici](../../Techniques/Références_globales.md)

Paramètres attendus :

| nom    | type   | obligatoire | valeur par défaut | commentaire |
|--------|--------|-------------|-------------------|-------------|
| id     | String | OUI         |                   | Obligatoire |
| locale | String | NON         | fr                |             |
| page   | int    | NON         | 1                 |             |
| limit  | int    | NON         | 20                |             |


### Informations


**Requêtes CURL**
`````shell
curl --request GET \
--url --location '[url-de-mon-site]/api/v1/page/content?id=83 \
--header 'Accept: application/json' \
--header 'User-Token: [user-token' \
--header 'Authorization: Bearer [mon-token]'
`````

`````shell
curl --request GET \
--url --location '[url-de-mon-site/api/v1/page/content?id=83&locale=es&page=5&limit=1 \
--header 'Accept: application/json' \
--header 'Authorization: Bearer [mon-token]'
`````

**Réponse 200**

Pour un listing d'article
url : [url-de-mon-site]/api/v1//page/content?id=83
````json
{
  "code_http": 200,
  "message": "success",
  "data": {
    "content": {
      "pages": [
        {
          "title": "Dernières articles du blog",
          "slug": "blogs"
        },
        {
          "title": "Article de blog",
          "slug": "article-blog"
        }
      ],
      "limit": 25,
      "current_page": 1,
      "rows": 2
    }
  }
}
````
**Réponse 200**

Contenu de page de type texte
url : [url-de-mon-site]/api/v1/page/content?id=100&locale=es
````json
{
  "code_http": 200,
  "message": "success",
  "data": {
    "content": "<p>ES - Lorem ipsum dolor sit amet, consectetur adipiscing elit. Curabitur viverra condimentum sapien, quis pellentesque lectus viverra quis. Phasellus ut massa hendrerit orci scelerisque dignissim a et purus. Vestibulum ullamcorper venenatis turpis nec pharetra. Mauris dui nunc, faucibus sit amet ullamcorper ac, mattis non neque. Fusce auctor nisi dolor, a sollicitudin eros lacinia in. Pellentesque sollicitudin, ipsum eget vehicula porta, nisi nisl varius lacus, eget rhoncus massa sapien sed dui. Vestibulum tincidunt ex a hendrerit congue.</p>\n"
  }
}
````

**Réponse 200**

Contenu de page de type FAQ
url : [url-de-mon-site]/api/v1/page/content?id=85&locale=es
````json
{
  "code_http": 200,
  "message": "success",
  "data": {
    "content": {
      "title": "[ES] La FAQ de NatheoCMS",
      "categories": [
        {
          "title": "[ES] Installation",
          "questions": [
            {
              "title": "[ES] Comment installer NatheoCMS",
              "answer": "[ES] In sed quam ac arcu fringilla vulputate vitae id est. Donec eleifend purus vel tincidunt ultricies. Integer dapibus erat eu ultrices aliquam. Pellentesque ut porta purus. Vestibulum porttitor vestibulum ante, a aliquam dui pretium sit amet. Aenean vitae mattis arcu. Curabitur semper lorem sed lacinia sodales. Integer et finibus erat. Aenean lectus augue, ullamcorper at felis non, egestas elementum felis. Proin a tortor feugiat, sagittis mi ut, pellentesque libero. Ut in justo eget libero vehicula elementum a ut magna. In quis tristique leo."
            },
            {
              "title": "[ES] Comment mettre à jour NateoCMS",
              "answer": "[ES] In sed quam ac arcu fringilla vulputate vitae id est. Donec eleifend purus vel tincidunt ultricies. Integer dapibus erat eu ultrices aliquam. Pellentesque ut porta purus. Vestibulum porttitor vestibulum ante, a aliquam dui pretium sit amet. Aenean vitae mattis arcu. Curabitur semper lorem sed lacinia sodales. Integer et finibus erat. Aenean lectus augue, ullamcorper at felis non, egestas elementum felis. Proin a tortor feugiat, sagittis mi ut, pellentesque libero. Ut in justo eget libero vehicula elementum a ut magna. In quis tristique leo."
            }
          ]
        },
        {
          "title": "[ES] Mise à jour",
          "questions": [
            {
              "title": "[ES] Comment mettre à jour NatheoCMS",
              "answer": "[ES] In sed quam ac arcu fringilla vulputate vitae id est. Donec eleifend purus vel tincidunt ultricies. Integer dapibus erat eu ultrices aliquam. Pellentesque ut porta purus. Vestibulum porttitor vestibulum ante, a aliquam dui pretium sit amet. Aenean vitae mattis arcu. Curabitur semper lorem sed lacinia sodales. Integer et finibus erat. Aenean lectus augue, ullamcorper at felis non, egestas elementum felis. Proin a tortor feugiat, sagittis mi ut, pellentesque libero. Ut in justo eget libero vehicula elementum a ut magna. In quis tristique leo."
            }
          ]
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


*Si le paramètre locale n'est pas valide*
````json
{
  "code_http": 403,
  "message": "Ressource non accessible",
  "errors": [
    "Choisir une locale entre fr (français) ou es (espagnol) ou en (anglais) "
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

*Si le contenu n'existe pas*
````json
{
    "code_http": 403,
    "message": "Ressource non accessible",
    "errors": [
        "Contenu non disponible"
    ]
}
````