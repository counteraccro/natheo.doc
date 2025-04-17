## Les pré-requis pour le bon fonctionnement de Natheo CMS

[Index](../../index.md) > Les pré-requis

Voici les pré-requis pour que Natheo CMS fonctionne correctement dans les meilleures conditions

* PHP 8.3 ou +
* Yarn 1.22.19 ou +
* Composer 2.7.7 ou +

### Base de données :
Natheo CMS prend en charge plusieurs bases de données qui sont :
    
* PostgresSql 15.2 ou +
* Mysql 8.2, 9.X ou +
  * Le storage engine de Mysql doit être **InnoDB**
  * Le CMS fonctionne avec MYISAM mais certaines fonctionnalités peuvent ne plus fonctionner (test unitaire etc..)

Procédure pour [changer de base de données](bdd.md)

### Serveur web
 * Testé sur Apache version 2.4.58 ou +