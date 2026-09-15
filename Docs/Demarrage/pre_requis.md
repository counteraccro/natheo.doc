---
title: "Pré-requis"
parent: "Démarrage"
nav_order: 1
---

Voici les pré-requis pour installer et faire fonctionner Nathéo CMS dans les meilleures conditions.

## Serveur

- **PHP** 8.4 ou supérieur
- **Composer** 2.8.9 ou supérieur
- **Yarn** 1.22.22 ou supérieur

### Extensions PHP recommandées

`ext-ctype`, `ext-iconv`, `ext-pdo`, `ext-pdo_mysql`, `ext-json`, `ext-mbstring`

### Serveur web

- Testé sur Apache 2.4.58 ou supérieur

## Base de données

Nathéo CMS prend en charge plusieurs bases de données :

- **MySQL** 8.2 ou supérieur *(par défaut)*
  - Le storage engine doit être **InnoDB**
  - Le CMS fonctionne avec MyISAM, mais certaines fonctionnalités peuvent ne plus fonctionner correctement (tests unitaires, etc.)
- **PostgreSQL** 15.2 ou supérieur

> 💡 Pour utiliser PostgreSQL à la place de MySQL, suivez la [procédure de changement de base de données](base_de_donnees.md).

## Voir aussi
- [Installation en mode développeur](installation_dev.md)
- [Installation via l'installeur](installation_prod.md)
- [Options de configuration](configuration_installation.md)
- [Bases de données prises en charge](base_de_donnees.md)
