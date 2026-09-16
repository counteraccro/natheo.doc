---
title: "Options de configuration"
parent: "Démarrage"
nav_order: 4
---

Voici les options qui peuvent influencer l'installation et le comportement de Nathéo CMS.

## Fichier `config/services.yaml`

| Paramètre | Valeur par défaut | Rôle |
|---|---|---|
| `app.supported_locales` | `fr\|en\|es` | Langues supportées par le CMS |
| `app.default_locale` | `fr` | Langue utilisée quand aucune n'est précisée dans l'URL |
| `app.api_version` | `v1` | Version de l'API exposée |
| `app.ip_api_active_filter` | `true` | Si `true`, filtre l'accès à l'API par IP (voir `app.ip_api_authorize`) |
| `app.ip_api_authorize` | `[127.0.0.1, ::1]` | Liste des IP autorisées à appeler l'API quand le filtre est actif |
| `app.default_database_prefix` | *(vide)* | Préfixe ajouté devant chaque table (`_` est ajouté automatiquement si non vide) |
| `app.default_database_name` | `%env(NATHEO_DBNAME)%` | Nom de la base de données, lu depuis la variable d'env `NATHEO_DBNAME` |
| `app.debug_mode` | `%env(bool:NATHEO_DEBUG)%` | Lu depuis `NATHEO_DEBUG` — voir l'encadré ci-dessous |
| `app.version` | `2.0.0-beta.1` | Version affichée du CMS |
| `app.natheo_horizon.version` | `Natheo-Horizon v1.0.0` | Version du thème front par défaut |
| `app.current_branche` | `master` | Branche Git courante (affichage/diagnostic) |

> ⚠️ **`app.debug_mode` (`NATHEO_DEBUG`)** — en mode debug, les fixtures créent un compte de démonstration par rôle
> (`user`, `contributeur`, `admin`, `super-admin`, `user-demo`) **à la place** du compte que vous créez. Ne jamais
> activer ce mode en production. Voir son impact concret sur [l'installeur graphique](installation_prod.md).

## Fichier `.env`

```dotenv
###> symfony/framework-bundle ###
APP_ENV=prod                                    # dev (+ de logs) ou prod
APP_DEBUG=0                                     # lié à APP_ENV
APP_SECRET=93df0a91243d55a4d72a801726b38645     # généré automatiquement à l'installation
###< symfony/framework-bundle ###

###> NatheoCMS ###
NATHEO_DBNAME='natheo'                          # nom de la base de données (app.default_database_name)
NATHEO_DEBUG=false                              # active le mode debug du CMS (app.debug_mode)
###< NatheoCMS ###

###> doctrine/doctrine-bundle ###
DATABASE_URL="mysql://app:!ChangeMe!@127.0.0.1:3306/app?serverVersion=10.11.2-MariaDB&charset=utf8mb4"
###< doctrine/doctrine-bundle ###

###> symfony/messenger ###
MESSENGER_TRANSPORT_DSN=doctrine://default?auto_setup=0
###< symfony/messenger ###

###> symfony/mailer ###
MAILER_DSN=null://null
###< symfony/mailer ###
```

> 💡 Pour utiliser PostgreSQL à la place de MySQL, voir la
> [procédure de changement de base de données](base_de_donnees.md).

## Voir aussi
- [Pré-requis](pre_requis.md)
- [Installation en mode développeur](installation_dev.md)
- [Installation via l'installeur](installation_prod.md)
- [Bases de données prises en charge](base_de_donnees.md)
