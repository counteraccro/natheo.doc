## Les bases de données prises en charge

[Index](../../index.md) > Base de données prises en charge

Natheo CMS prend en charge actuellement les bases de données suivantes :
* PostgresSql 15.2 ou +
* Mysql 8.2 ou +

Par défaut, Natheo CMS est configuré pour être installé avec Mysql.

Si vous souhaitez utiliser PostgresSql voici les manipulations à faire :

Dans le fichier .env commenter la ligne suivante :
```dotenv
DATABASE_URL="mysql://app:!ChangeMe!@127.0.0.1:3306/app?serverVersion=10.11.2-MariaDB&charset=utf8mb4"
```

et décommenter la ligne suivante :
````dotenv
# DATABASE_URL="postgresql://postgres:root@127.0.0.1:5432/natheo?serverVersion=15&charset=utf8"
````

Aller dans le fichier ```src/Utils/Installation/InstallationConst.php```
chercher la ligne suivante :
````php
    /**
     * Stratégie pour la création de la base de données
     * SEQUENCE pour ORACLE et PostgreSQL
     * IDENTITY pour MySQL, SQLite, MsSQL et SQL
     */
    const STRATEGY = self::STRATEGY_MYSQL;
````

Remplacer ```self::STRATEGY_MYSQL``` par ```self::STRATEGY_POSTGRESQL```

### Pour les développeurs
Le changement est tout à fait possible à ***chaud*** en environnement de dev mais nécessite un clear cache et une
perte de la session en cours.