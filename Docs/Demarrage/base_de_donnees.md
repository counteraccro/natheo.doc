---
title: "Bases de données prises en charge"
parent: "Démarrage"
nav_order: 5
---

Nathéo CMS prend en charge actuellement les bases de données suivantes :
* **MySQL** 8.2 ou supérieur *(par défaut)*
* **PostgreSQL** 15.2 ou supérieur

Pour utiliser PostgreSQL à la place de MySQL, il suffit de changer le DSN dans le fichier `.env` : commentez la ligne
MySQL et décommentez (ou ajoutez) la ligne PostgreSQL. Aucun fichier PHP n'est à modifier.

```dotenv
# DATABASE_URL="mysql://app:!ChangeMe!@127.0.0.1:3306/app?serverVersion=10.11.2-MariaDB&charset=utf8mb4"
DATABASE_URL="postgresql://app:!ChangeMe!@127.0.0.1:5432/app?serverVersion=15&charset=utf8"
```

### Pour les développeurs

Le CMS déduit le SGBD du schéma de `DATABASE_URL` (`DoctrineStrategy::current()`). Les schémas `postgres`,
`postgresql`, `pgsql` et `pdo-pgsql` correspondent à PostgreSQL, tous les autres à MySQL / MariaDB.

Les migrations existent en une version par SGBD, dans `migrations/V2/Mysql/` et `migrations/V2/Postgresql/`. Seul le
dossier du SGBD courant est chargé (`App\Doctrine\Migrations\PlatformMigrationFinder`).

> ⚠️ Le schéma et les données ne sont pas convertis : changer de SGBD sur un site existant revient à repartir d'une
> base neuve, à recréer avec l'[installeur](installation_prod.md) ou la commande `natheo:install`.

## Voir aussi
- [Pré-requis](pre_requis.md)
- [Options de configuration](configuration_installation.md)
- [Installation en mode développeur](installation_dev.md)
- [Installation via l'installeur](installation_prod.md)
