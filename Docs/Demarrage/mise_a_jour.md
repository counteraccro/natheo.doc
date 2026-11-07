---
title: "Mise à jour"
parent: "Démarrage"
nav_order: 6
---

Cette page explique comment passer d'une version de la V2 à une version plus récente, et ce qu'il faut savoir si vous
venez de la V1.

## Depuis la V1

La V2 repart d'un nouveau schéma de base de données : **il n'existe pas de migration automatique depuis la V1**. Il faut
faire une nouvelle installation de la V2, puis reprendre le contenu :

- [en mode développeur](installation_dev.md) ;
- [via l'installeur graphique](installation_prod.md).

Vérifiez aussi les [pré-requis](pre_requis.md), qui ont évolué : la V2 demande notamment PHP 8.4 ou supérieur.

## Entre deux versions de la V2

À partir de la V2, le schéma de la base de données évolue par migrations Doctrine.

### Avant de commencer

- **Sauvegardez la base de données**, par exemple depuis le
  [gestionnaire de base de données](../GuideAdmin/Outils/gestionnaire_bdd.md).
- **Relevez les traductions modifiées depuis le CMS.** Elles sont écrites dans les fichiers YAML du dossier
  `translations/`, qui font partie du code source : une mise à jour peut les écraser (voir
  [Gestion des traductions](../GuideAdmin/Systeme/traductions.md)). `git diff translations/` les liste si le CMS a été
  installé avec Git. Elles sont à réappliquer après la mise à jour.
- **Consultez les [notes de version](../Projet/versions.md)** de la version visée, et vérifiez qu'elle ne demande pas de
  nouveaux [pré-requis](pre_requis.md).

### Procédure

Ces commandes s'exécutent à la racine du projet, en production (`APP_ENV=prod`).

```bash
# 1. Récupérer le nouveau code
git pull

# 2. Installer les dépendances PHP
composer install --no-dev --optimize-autoloader

# 3. Reconstruire les assets (public/build/ n'est pas versionné)
yarn install
yarn build

# 4. Mettre à jour le schéma de la base de données
php bin/console doctrine:migrations:migrate

# 5. Vider le cache
php bin/console cache:clear
```

`doctrine:migrations:migrate` demande une confirmation avant de s'exécuter. Seules les migrations de votre SGBD sont
jouées : les dossiers `migrations/V2/Mysql` et `migrations/V2/Postgresql` sont distincts, et le bon est choisi
automatiquement.

### Après la mise à jour

- Réappliquez les traductions relevées avant la mise à jour.
- Si un worker Messenger tourne en tâche de fond (voir [Commandes Symfony](../Architecture/commandes.md)), relancez-le :
  il garde l'ancien code en mémoire tant qu'il n'est pas redémarré.
- Vérifiez la version installée sur la page [Information](../GuideAdmin/Systeme/information.md).

## Voir aussi

- [Versions](../Projet/versions.md)
- [Changelog](../Projet/changelog.md)
- [Commandes Symfony](../Architecture/commandes.md)
