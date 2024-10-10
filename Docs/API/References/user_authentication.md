## Authentification

[Index](../../../index.md) > [API](../index.md) > User authentication

Permet d'authentifier un utilisateur, si tout est ok renvoi un user token pour identifier l'utilisateur
Ce token à une date de validité défini par l'option ``OS_API_TIME_VALIDATE_USER_TOKEN``

Paramètres attendus :

| nom      | type   | obligatoire | valeur par défaut | commentaire |
|----------|--------|-------------|-------------------|-------------|
| username | String | OUI         |                   |             |
| passowrd | String | OUI         |                   |             |

**Requête CURL**
`````shell
curl  --request POST \
--url 'http://dev.natheo/api/v1/authentication/user' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer [mon-token]' \
--data-raw '{
    "username" : "user@natheo.com",
    "password": "user@natheo.com"
}'
`````

**Réponse 200**
````json
{
  "code_http": 200,
  "message": "success",
  "data": {
    "token": "c2bDuBobeZmZdwvk1HCMnfhmpF18zciw"
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
  "message": "error",
  "data": [],
  "errors": [
    "Utilisateur non trouvé"
  ]
}
````

**Réponse 403**

*Si un paramètre n'a pas le bon type*
````json
{
    "code_http": 403,
    "message": "error",
    "data": [],
    "errors": [
        "Le paramètre password doit être de type string"
    ]
}
````

**Réponse 403**

*Si un paramètre est mal écrit ou non présent*
````json
{
  "code_http": 403,
  "message": "Ressource non accessible",
  "errors": [
    "Le paramètre attendu username n'est pas présent"
  ]
}
````