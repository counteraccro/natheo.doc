## Authentification

[Index](../../../index.md) > [API](../index.md) > Authentification

Permet de savoir si vous pouvez accéder à l'API de NatheoCMS

Si le token est bon, renvoi les roles d'accès

**Requête CURL**
`````shell
curl --request GET \
--url '[url-de-mon-site]/api/v1/authentication' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer [mon-token]'
`````

**Réponse 200**
````json
{
    "code_http": 200,
    "message": "success",
    "data": {
        "roles": [
            "ROLE_READ_API",
            "ROLE_USER"
        ]
    }
}
````

**Réponse 401**

*Si le token est invalide*
````json
{
  "code_http": 401,
  "message": "Clé API Invalide"
}
````