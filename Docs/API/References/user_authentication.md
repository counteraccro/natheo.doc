---
title: "Authentification utilisateur"
parent: "API"
nav_order: 2
---

Permet d'authentifier un utilisateur du back-office avec son email et son mot de passe. Si tout est correct,
renvoie un **token utilisateur** à transmettre ensuite dans l'en-tête `User-Token` des autres appels (voir
[Token utilisateur](../index.md#token-utilisateur-user-token)).

Seuls les comptes ayant au moins le rôle **Contributeur** peuvent obtenir un token : un compte qui n'a que le rôle
Utilisateur (`ROLE_USER`) est refusé comme si ses identifiants étaient faux. Les comptes désactivés ou anonymisés
sont refusés aussi.

Le nombre de tentatives est **limité** : au-delà de **5 tentatives en 15 minutes** pour un même couple adresse IP +
email (fenêtre glissante), l'appel est refusé avec un `429`, même si le mot de passe est correct. Une connexion
réussie remet le compteur à zéro.

Paramètres attendus (corps JSON) :

| nom      | type   | obligatoire | valeur par défaut | commentaire              |
|----------|--------|-------------|-------------------|--------------------------|
| username | String | OUI         |                   | Email du compte          |
| password | String | OUI         |                   | Mot de passe du compte   |

**Requête CURL**
`````shell
curl --request POST \
--url '[url-de-mon-site]/api/v1/authentication/user' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer [mon-token]' \
--data-raw '{
    "username" : "contributeur@natheo.com",
    "password": "contributeur@natheo.com"
}'
`````

**Réponse 200**
````json
{
  "code_http": 200,
  "message": "success",
  "data": {
    "token": "XqcMK5yWHkdYpiXvoa1Evv5jttGbHsvm"
  }
}
````

## Validité du token

- Le token est valable pendant la durée définie par l'option système **Temps de validation du token de connexion**
  (`OS_API_TIME_VALIDATE_USER_TOKEN`, 1 heure à l'installation, **24 heures maximum**, voir
  [options système](../../GuideAdmin/Systeme/options.md#api)).
- Un utilisateur n'a qu'**un seul** token à la fois : se reconnecter génère un nouveau token et invalide le
  précédent.
- Le token est refusé une fois sa durée dépassée, si le compte a été désactivé ou anonymisé entre-temps, ou après un
  changement de mot de passe (le token est alors supprimé, il faut se reconnecter).
- Comme les jetons API, le token utilisateur n'est jamais stocké en clair : seule son empreinte (SHA-256) est
  conservée en base.

**Réponse 401**

*Si l'email et/ou le mot de passe sont faux, ou si le compte n'a que le rôle Utilisateur*
````json
{
  "code_http": 401,
  "message": "Accès non autorisé",
  "errors": [
    "Utilisateur non trouvé"
  ]
}
````

**Réponse 429**

*Si la limite de tentatives est atteinte*
````json
{
  "code_http": 429,
  "message": "Trop de tentatives, veuillez réessayer plus tard",
  "errors": [
    "Trop de tentatives, veuillez réessayer plus tard"
  ]
}
````

L'en-tête HTTP `Retry-After` indique le nombre de secondes à attendre avant de réessayer.

**Réponse 400**

*Si un paramètre est absent*
````json
{
  "code_http": 400,
  "message": "Requête invalide",
  "errors": [
    "Le paramètre attendu password n'est pas présent"
  ]
}
````

*Si un paramètre est vide*
````json
{
  "code_http": 400,
  "message": "Requête invalide",
  "errors": [
    "Le champ username ne peut pas être vide",
    "Le champ password ne peut pas être vide"
  ]
}
````

*Si un paramètre n'est pas une valeur simple (tableau, objet)*
````json
{
  "code_http": 400,
  "message": "Requête invalide",
  "errors": [
    "Le paramètre username doit être de type string"
  ]
}
````

*Si le corps de la requête est vide ou n'est pas du JSON valide*
````json
{
  "code_http": 400,
  "message": "Requête invalide",
  "errors": [
    "Le corps de la requête doit être un JSON valide"
  ]
}
````

> 📝 Un nombre ou un booléen est accepté et converti en texte : `"username": 123` ne provoque pas d'erreur de
> type, seulement un « Utilisateur non trouvé ».

Les erreurs communes (jeton invalide, API fermée) sont décrites dans la [présentation de l'API](../index.md#format-des-réponses).

## Voir aussi
- [Authentification](authentication.md)
- [Modérer un commentaire](moderate_comment.md), qui exige un token utilisateur
