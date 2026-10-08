---
title: "Ajouter un nouveau commentaire"
parent: "API"
nav_order: 7
---

Ajoute un commentaire sur une page désignée par son id ou par son slug. Si tout est correct, renvoie l'id du
commentaire créé.

Cet endpoint exige un jeton API de rôle **Lecture + Écriture** (`ROLE_WRITE_API`) ou **Admin** : un jeton
*Lecture* reçoit un `403` (voir [Rôles des jetons](../index.md#rôles-des-jetons)).

Le commentaire est accepté si :
- la page existe, est **publiée** et **active** ;
- les commentaires sont ouverts **sur le site** (option système **Ouvrir les commentaires ?**, voir
  [options système](../../GuideAdmin/Systeme/options.md#commentaire)) **et sur la page** (onglet *Commentaires*
  de la page).

Il est enregistré avec le statut :
- **En attente de validation** si l'option système **Forcer la validation des nouveaux commentaires** est
  activée, ou si le statut par défaut des commentaires de la page est « En attente de validation » ou « Modéré » ;
- **Validé** sinon.

Les envois sont **limités à 5 toutes les 10 minutes par IP de visiteur** (fenêtre glissante), qu'ils aboutissent ou
non : au-delà, l'API répond `429`. L'IP prise en compte est celle du paramètre `ip` ; s'il est absent, c'est l'IP
de l'appelant, c'est-à-dire **celle de votre serveur front**.

> ⚠️ Transmettez toujours le paramètre `ip` du visiteur : sans lui, tous les visiteurs de votre site partagent la
> même limite de 5 commentaires en 10 minutes. À l'inverse, comme cette IP est fournie par le front, la limite ne
> protège que si votre front transmet la vraie IP et ne laisse pas le visiteur la choisir.

L'auteur de la page reçoit une [notification](../../GuideAdmin/MonCompte/Notifications/notifications.md) pour chaque
nouveau commentaire.

Paramètres attendus (corps JSON) :

| nom        | type    | obligatoire | valeur par défaut | commentaire                                               |
|------------|---------|-------------|-------------------|-----------------------------------------------------------|
| page_id    | Integer | OUI*        |                   | Id de la page. *Obligatoire si `page_slug` est absent      |
| page_slug  | String  | OUI*        |                   | Slug de la page. *Obligatoire si `page_id` est absent      |
| locale     | String  | NON         | fr                | Langue utilisée pour le titre de la page dans la notification |
| author     | String  | OUI         |                   | Nom affiché de l'auteur, 255 caractères maximum           |
| email      | String  | NON         |                   | Email de l'auteur, 255 caractères maximum. S'il est fourni, il doit être valide |
| comment    | String  | OUI         |                   | Texte du commentaire (Markdown), 10 000 caractères maximum |
| ip         | String  | NON         |                   | IP du visiteur. Si elle est fournie, elle doit être valide. Sert aussi à la limite d'envoi |
| user_agent | String  | NON         |                   | User-agent du navigateur du visiteur, 1 000 caractères maximum |

`page_id` et `page_slug` sont exclusifs : il en faut exactement un. Le slug est cherché dans toutes les langues.

> 📝 `ip` et `user_agent` doivent être ceux du **visiteur**, transmis par votre front : l'API ne les déduit pas de
> la requête (qui vient de votre serveur). Ils sont affichés aux modérateurs dans la
> [fiche du commentaire](../../GuideAdmin/Contenu/Commentaires/moderation.md).

> 📝 Les balises HTML sont retirées du texte du commentaire et du nom de l'auteur avant l'enregistrement
> (`<b>Bonjour</b>` devient `Bonjour`) ; le Markdown, lui, est conservé.

**Requête CURL**
`````shell
curl --request POST \
--url '[url-de-mon-site]/api/v1/comment' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer [mon-token-ecriture]' \
--data-raw '{
    "page_slug" : "a-propos",
    "author" : "Jean Dupont",
    "email" : "jean.dupont@example.com",
    "comment" : "Très bon article !",
    "ip" : "203.0.113.42",
    "user_agent" : "Mozilla/5.0"
}'
`````

**Réponse 201**
````json
{
  "code_http": 201,
  "message": "success",
  "data": {
    "id": 7
  }
}
````

**Réponse 400**

*Si `page_id` et `page_slug` sont présents ensemble*
````json
{
  "code_http": 400,
  "message": "Requête invalide",
  "errors": [
    "Le paramètre id_page et page_slug ne peuvent pas être mis ensemble"
  ]
}
````

*Si ni `page_id` ni `page_slug` ne sont présents*
````json
{
  "code_http": 400,
  "message": "Requête invalide",
  "errors": [
    "Le paramètre id_page ou page_slug doit être présent"
  ]
}
````

> 📝 Le message d'erreur parle de `id_page`, mais le paramètre attendu s'appelle bien **`page_id`**. Envoyer `id`
> ou `id_page` revient à ne rien envoyer.

*Si un paramètre ne respecte pas son format ou sa longueur maximale*
````json
{
  "code_http": 400,
  "message": "Requête invalide",
  "errors": [
    "Le paramètre author ne peut pas être vide",
    "Le paramètre email doit être un email valide",
    "Le paramètre comment ne peut pas être vide",
    "Le paramètre ip doit être une adresse ip valide"
  ]
}
````

*Si un paramètre n'est pas une valeur simple (tableau, objet)*
````json
{
  "code_http": 400,
  "message": "Requête invalide",
  "errors": [
    "Le paramètre author doit être de type string"
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

**Réponse 404**

*Si la page n'existe pas, n'est pas publiée ou est désactivée*
````json
{
  "code_http": 404,
  "message": "Ressource non disponible",
  "errors": [
    "Page non disponible"
  ]
}
````

**Réponse 403**

*Si le jeton API n'a pas le rôle `ROLE_WRITE_API`*
````json
{
  "code_http": 403,
  "message": "Ressource non accessible",
  "errors": [
    "Les droits du token API ne permettent pas d'accéder à cette ressource"
  ]
}
````

*Si les commentaires sont fermés sur le site ou sur la page*
````json
{
  "code_http": 403,
  "message": "Ressource non accessible",
  "errors": [
    "Les commentaires ne sont pas ouverts"
  ]
}
````

**Réponse 429**

*Si la limite d'envoi est atteinte pour cette IP*
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

Les erreurs communes (jeton invalide, API fermée, locale invalide) sont décrites dans la
[présentation de l'API](../index.md#format-des-réponses).

## Voir aussi
- [Liste des commentaires d'une page](comment_by_page.md)
- [Modérer un commentaire](moderate_comment.md)
- [Gestion des commentaires](../../GuideAdmin/Contenu/Commentaires/listing.md)
