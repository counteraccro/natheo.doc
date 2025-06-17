## Modération d'un commentaire

[Index](../../../index.md) > [API](../index.md) > Modération d'un commentaire

Permet de modérer un commentaire via son id

Paramètres attendus :

| nom                | type   | obligatoire | valeur par défaut | commentaire |
|--------------------|--------|-------------|-------------------|-------------|
| status             | String | OUI         |                   |             |
| moderation_comment | String | OUI         |                   |             |
| user_token         | String | OUI         |                   |             |


**Requête CURL**
`````shell
curl  --request PUT \
--url 'http://dev.natheo/api/v1/comment/moderate/2' \
--header 'Accept: application/json' \
--header 'User-Token: [user-token]' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer [mon-token]' \
--data '{
    "status":3,
    "moderation_comment":"API modération"
}'

`````

**Réponse 202**
````json
{
  "code_http": 202,
  "message": "success",
  "data": [
    "Commentaire modéré"
  ]
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

*Si login et/ou mot de passe invalide ou user n'a pas les droits*
````json
{
  "code_http": 403,
  "message": "Accès non autorisé",
  "errors": [
    "Utilisateur non trouvé"
  ]
}
````

**Réponse 404**

*Le commentaire n'existe pas*
````json
{
  "code_http": 404,
  "message": "Ressource non disponible",
  "errors": [
    "Commentaire non disponible"
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