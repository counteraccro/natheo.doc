## Authentification

[Index](../../../index.md) > [API](../index.md) > Sitemap

Permet de retourner l'ensemble des pages (toutes les langues) publiées pour générer le sitemap du site
Le retour est formaté pour la génération d'un sitemap

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
      "loc": "/fr/page/bienvenue",
      "priority": "1.00",
      "lastmod": "2025-07-25T06:09:18+00:00"
    },
    {
      "loc": "/en/page/welcome",
      "priority": "1.00",
      "lastmod": "2025-07-25T06:09:18+00:00"
    },
    {
      "loc": "/es/page/bienvenido",
      "priority": "1.00",
      "lastmod": "2025-07-25T06:09:18+00:00"
    },
    {
      "loc": "/fr/page/pages",
      "priority": "1.00",
      "lastmod": "2025-07-24T19:41:06+00:00"
    },
    {
      "loc": "/es/page/paginas",
      "priority": "1.00",
      "lastmod": "2025-07-24T19:41:06+00:00"
    },
    {
      "loc": "/en/page/page",
      "priority": "1.00",
      "lastmod": "2025-07-24T19:41:06+00:00"
    }
  ]
}
````

**Réponse 401**

*Si le token est invalide*
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

*Si l'API est fermée*
````json
{
  "code_http": 403,
  "message": "Ressource non accessible",
  "errors": [
    "Ressource non accessible - API fermée"
  ]
}
````