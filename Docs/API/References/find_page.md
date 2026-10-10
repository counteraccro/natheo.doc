---
title: "Find page"
parent: "API"
nav_order: 4
---

Renvoie une page à partir de son slug : titre, auteur, rendu, tags, statistiques, SEO, liste de ses blocs de contenu
et, par défaut, tous ses menus. Le contenu de chaque bloc se récupère ensuite avec
[Find page content](find_page_content.md).

- Sans `slug` (ou avec un slug vide), renvoie la **page d'accueil** (*landing page*) si elle existe.
- Seules les pages **publiées** et **actives** sont renvoyées. Avec un `User-Token` valide d'un compte
  **Contributeur** ou plus, les pages en **brouillon** le sont aussi.
- Le slug est cherché dans **toutes les langues** ; la `locale` ne sert qu'à choisir la langue des textes
  renvoyés.

> ⚠️ Chaque appel réussi sur une page **publiée** **incrémente son compteur de lectures** (`PAGE_NB_READ`) ; la
> consultation d'un brouillon avec un `User-Token` ne compte pas. Évitez d'appeler cet endpoint pour autre chose
> qu'un affichage réel de la page (pré-chargement, tests…), sous peine de fausser la statistique.

Pour plus d'information sur les valeurs numériques renvoyées, voir les
[références globales](../../Architecture/references_globales.md).

Paramètres attendus (query string) :

| nom               | type    | obligatoire | valeur par défaut | commentaire                                                         |
|-------------------|---------|-------------|-------------------|---------------------------------------------------------------------|
| slug              | String  | NON         |                   | Slug de la page. Absent ou vide : page d'accueil                    |
| locale            | String  | NON         | fr                | Langue des textes renvoyés                                          |
| show_menus        | Boolean | NON         | true              | `false` pour ne pas renvoyer les menus                              |
| show_tags         | Boolean | NON         | true              | `false` pour ne pas renvoyer les tags                               |
| show_statistiques | Boolean | NON         | true              | `false` pour ne pas renvoyer les statistiques                       |
| menu_positions    | String  | NON         | 0                 | Positions des menus à renvoyer, séparées par des virgules (ex. `1,3`) : `0` tous, `1` en-tête, `2` droite, `3` pied de page, `4` gauche |

Les paramètres `show_*` acceptent `true`/`false`, `1`/`0`, `yes`/`no` et `on`/`off` ; toute autre valeur est refusée
(`400`).

**Requêtes CURL**
`````shell
curl --request GET \
--url '[url-de-mon-site]/api/v1/page/find?slug=a-propos' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer [mon-token]'
`````

`````shell
curl --request GET \
--url '[url-de-mon-site]/api/v1/page/find?slug=a-propos&locale=en&show_statistiques=false&menu_positions=1,3' \
--header 'Accept: application/json' \
--header 'User-Token: [user-token]' \
--header 'Authorization: Bearer [mon-token]'
`````

**Réponse 200**

url : `[url-de-mon-site]/api/v1/page/find?slug=a-propos` (menus raccourcis)
````json
{
  "code_http": 200,
  "message": "success",
  "data": {
    "page": {
      "title": "A propos",
      "slug": "a-propos",
      "status": 1,
      "render": 5,
      "author": {
        "author": "user.demo@mail.fr",
        "description": "Je suis un compte de démonstration généré à l'installation du CMS",
        "avatar": "avatar-2.png"
      },
      "created": 1791061090,
      "update": 1791061090,
      "headerImg": "http://dev.natheo:8888/assets/natheotheque//medias/paysage/road.jpg",
      "tags": [
        {
          "label": "FAQ",
          "color": "#262cdf"
        },
        {
          "label": "Démo",
          "color": "#F0C847"
        }
      ],
      "statistiques": {
        "PAGE_NB_READ": "1"
      },
      "contents": [
        {
          "id": 32,
          "type": 1,
          "position": 1
        },
        {
          "id": 33,
          "type": 1,
          "position": 2
        },
        {
          "id": 34,
          "type": 1,
          "position": 3
        }
      ],
      "seo": [
        {
          "name": "description",
          "value": "La Faq de Natheo CMS",
          "balise": "<meta name=\"description\" content=\"La Faq de Natheo CMS\">"
        },
        {
          "name": "keywords",
          "value": "Natheo-CMS, demo, FAQ",
          "balise": "<meta name=\"keywords\" content=\"Natheo-CMS, demo, FAQ\">"
        }
      ],
      "openComment": true,
      "menus": {
        "HEADER": {
          "id": 1,
          "position": "HEADER",
          "type": 2,
          "elements": [
            {
              "id": 1,
              "target": "_self",
              "label": "Index",
              "url": "",
              "slug": "home",
              "category": 1,
              "columnPosition": 1,
              "rowPosition": 1
            }
          ]
        },
        "LEFT": {
          "id": 2,
          "position": "LEFT",
          "type": 12,
          "elements": []
        },
        "FOOTER": {
          "id": 3,
          "position": "FOOTER",
          "type": 16,
          "elements": []
        }
      }
    }
  }
}
````

### Champs renvoyés

| Champ | Description |
|---|---|
| `title` / `slug` | Titre et slug de la page dans la `locale` demandée |
| `status` | `1` publiée, `2` brouillon (visible uniquement avec un `User-Token`) |
| `render` | Mise en page des blocs ([références](../../Architecture/references_globales.md#rendu-de-la-page)) |
| `author.author` | Nom de l'auteur, affiché selon sa préférence de rendu des données personnelles |
| `author.description` / `author.avatar` | Description et nom de fichier de l'avatar de l'auteur |
| `created` / `update` | Dates de création et de dernière modification (timestamp Unix, en secondes) |
| `headerImg` | URL de l'image d'en-tête de la page |
| `tags` | Tags de la page : libellé dans la `locale` et couleur (absent si `show_tags=false`) |
| `statistiques` | Compteur de lectures `PAGE_NB_READ`, en texte (absent si `show_statistiques=false`) |
| `contents` | Blocs de la page : `id` (à passer à [Find page content](find_page_content.md)), `type` ([références](../../Architecture/references_globales.md#type-de-contenu-de-page)) et `position` (emplacement dans le rendu) |
| `seo` | Balises meta : nom, valeur dans la `locale` et balise HTML prête à insérer |
| `openComment` | `true` si les commentaires sont ouverts sur cette page |
| `menus` | Menus de la page indexés par position (absent si `show_menus=false`), même format que [Find menu](find_menu.md) |

### Menus renvoyés

Le bloc `menus` contient au plus un menu par position :

1. les menus **actifs** rattachés à la page ;
2. complétés, pour chaque position restée vide, par le **menu par défaut** de cette position ;
3. sauf qu'un menu **gauche** par défaut n'est pas ajouté si la page a déjà un menu **droite**, et inversement ;
4. enfin, seules les positions demandées par `menu_positions` sont conservées.

Si aucun menu ne correspond, la clé `menus` est absente de la réponse.

> 📝 L'URL de `headerImg` contient un double slash (`natheotheque//medias`), dû à une concaténation de chemin
> dans le CMS. Le lien fonctionne malgré tout.

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

**Réponse 400**

*Si un paramètre `show_*` n'est pas un booléen*
````json
{
  "code_http": 400,
  "message": "Requête invalide",
  "errors": [
    "Le paramètre show_tags doit être un booléen (true/false, 1/0)"
  ]
}
````

*Si `menu_positions` contient une valeur hors de 0 à 4*
````json
{
  "code_http": 400,
  "message": "Requête invalide",
  "errors": [
    "Choisi une position entre 0 (tout) - 1 (haut) - 2 (droite) - 3 (bas) - 4 (gauche). Plusieurs choix possible"
  ]
}
````

**Réponse 403**

*Si le `User-Token` est présent mais invalide ou expiré*
````json
{
  "code_http": 403,
  "message": "Ressource non accessible",
  "errors": [
    "Utilisateur non trouvé"
  ]
}
````

Les erreurs communes (jeton invalide, API fermée, locale invalide) sont décrites dans la
[présentation de l'API](../index.md#format-des-réponses).

## Voir aussi
- [Find page content](find_page_content.md)
- [Find menu](find_menu.md)
- [Gestion des pages](../../GuideAdmin/Contenu/Pages/listing.md)
- [Références globales](../../Architecture/references_globales.md)
