## Ajouter un nouveau commentaire

[Index](../../../index.md) > [API](../index.md) > Ajouter un nouveau commentaire

Permet d'ajouter un nouveau commentaire à la page défini par id ou page_slug et la locale
Si tout est ok, retourne l'id du commentaire

Paramètres attendus :

| nom        | type   | obligatoire | valeur par défaut | commentaire     |
|------------|--------|-------------|-------------------|-----------------|
| id         | String | OUI         |                   | id ou page_slug |
| page_slug  | String | OUI         |                   | id ou page_slug |
| locale     | String | NON         | fr                |                 |
| author     | String | OUI         |                   |                 |
| email      | String | OUI         |                   |                 |
| comment    | String | OUI         |                   |                 |
| ip         | String | OUI         |                   |                 |
| user_agent | String | OUI         |                   |                 |


**Requête CURL**
`````shell
curl  --request POST \
--url 'http://dev.natheo/api/v1/comment/' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer [mon-token]' \
--data-raw '{
                "page_slug" : "bienvenue",
                "author" : "azerty2",
                "email" : "azerty@gmail.com",
                "comment" : "test comment",
                "ip" : "127.0.0.1",
                "user_agent" : "user_agent"
}'

`````

**Réponse 201**
````json
{
  "code_http": 201,
  "message": "success",
  "data": {
    "id": 4
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

*La page n'existe pas*
````json
{
  "code_http": 403,
  "message": "Ressource non accessible",
  "errors": [
    "Page non disponible"
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