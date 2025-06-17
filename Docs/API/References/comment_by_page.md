## Liste des commentaires en fonction d'une page

[Index](../../../index.md) > [API](../index.md) > Liste des commentaires en fonction d'une page

Retourne une liste de commentaires paginée en fonction de l'id ou du slug de la page et de la langue

Paramètres attendus :

| nom        | type   | obligatoire | valeur par défaut | commentaire     |
|------------|--------|-------------|-------------------|-----------------|
| id         | String | OUI         |                   | id ou page_slug |
| page_slug  | String | OUI         |                   | id ou page_slug |
| locale     | String | NON         | fr                |                 |
| page       | String | NON         | 1                 |                 |
| limit      | String | NON         | 25                |                 |
| order      | String | NON         | desc              |                 |
| order_by   | String | NON         | createdAt         |                 |
| user_token | String | NON         |                   |                 |


**Requête CURL**
`````shell
curl  --request GET \
--url 'http://dev.natheo/api/v1/comment/page?page_slug=bienvenue&limit=10' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer [mon-token]' \
`````

**Réponse 200**
````json
{
  "code_http": 200,
  "message": "success",
  "data": {
    "comments": [
      {
        "id": 4,
        "status": 1,
        "createdAt": "2025-06-14T14:23:39+00:00",
        "updateAt": "2025-06-14T14:23:39+00:00",
        "comment": "test comment"
      },
      {
        "id": 1,
        "status": 1,
        "createdAt": "2025-04-16T13:37:24+00:00",
        "updateAt": "2025-05-02T08:29:31+00:00",
        "comment": "Je suis un commentaire **en attente de validation**"
      },
      {
        "id": 2,
        "status": 3,
        "createdAt": "2025-04-16T13:37:24+00:00",
        "updateAt": "2025-06-14T07:03:42+00:00",
        "comment": "Je suis un commentaire **validé**",
        "moderate": "API modération"
      },
      {
        "id": 3,
        "status": 3,
        "createdAt": "2025-04-16T13:37:24+00:00",
        "updateAt": "2025-04-16T13:37:24+00:00",
        "comment": "Je suis un commentaire **modéré**",
        "moderate": "Raison de la modération"
      }
    ],
    "current_page": 1,
    "rows": 4,
    "limit": 10
  }
}
````

**Réponse 401**

*Si la clé API n'est pas valide*
````json
{
  "code_http": 401,
  "message": "Clé API Invalide"
}
````

**Réponse 401**

*Si login et/ou mot de passe invalide ou user n'a pas les droits*
````json
{
  "code_http": 401,
  "message": "Accès non autorisé",
  "errors": [
    "Utilisateur non trouvé"
  ]
}
````

**Réponse 403**

*Si id et slug en même temps*
````json
{
  "code_http": 403,
  "message": "Ressource non accessible",
  "errors": [
    "Le paramètre id et page_slug ne peuvent pas être mis ensemble"
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