---
title: "Bases de données prises en charge"
parent: "Démarrage"
nav_order: 5
---

Nathéo CMS prend en charge actuellement les bases de données suivantes :
* **MySQL** 8.2 ou supérieur *(par défaut)*
* **PostgreSQL** 15.2 ou supérieur

Si vous souhaitez utiliser PostgreSQL à la place de MySQL, deux manipulations sont nécessaires.

### 1. Changer le DSN dans le fichier `.env`

Commentez la ligne MySQL et décommentez (ou ajoutez) la ligne PostgreSQL :

```dotenv
# DATABASE_URL="mysql://app:!ChangeMe!@127.0.0.1:3306/app?serverVersion=10.11.2-MariaDB&charset=utf8mb4"
DATABASE_URL="postgresql://app:!ChangeMe!@127.0.0.1:5432/app?serverVersion=15&charset=utf8"
```

### 2. Changer la stratégie de génération des identifiants

Doctrine ne génère pas les clés primaires de la même façon selon le SGBD (`IDENTITY` pour MySQL, `SEQUENCE` pour
PostgreSQL). Cette stratégie est centralisée dans `src/Enum/Installation/DoctrineStrategy.php` :

```php
enum DoctrineStrategy: string
{
    const string CURRENT = self::MYSQL->value; // <- à changer en self::POSTGRESQL->value

    case POSTGRESQL = 'SEQUENCE';
    case MYSQL = 'IDENTITY';
}
```

Remplacez `self::MYSQL->value` par `self::POSTGRESQL->value` sur la constante `CURRENT`. Toutes les entités du CMS
référencent cette constante (`#[ORM\GeneratedValue(strategy: DoctrineStrategy::CURRENT)]`), il n'y a donc qu'un seul
endroit à modifier.

### Pour les développeurs

Le changement est possible à ***chaud*** en environnement de dev, mais nécessite un `cache:clear` et entraîne la
perte de la session en cours.

## Voir aussi
- [Pré-requis](pre_requis.md)
- [Options de configuration](configuration_installation.md)
- [Installation en mode développeur](installation_dev.md)
- [Installation via l'installeur](installation_prod.md)
