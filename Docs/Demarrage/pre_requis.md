---
title: "Pré-requis"
parent: "Démarrage"
nav_order: 1
---

Voici les pré-requis pour installer et faire fonctionner Nathéo CMS dans les meilleures conditions.

## Serveur

| Composant | Version requise | Notes |
|---|---|---|
| **PHP** | 8.4 ou supérieur | Extensions recommandées ci-dessous |
| **Composer** | 2.8.9 ou supérieur | Gestionnaire de dépendances PHP |
| **Yarn** | 1.22.22 ou supérieur | Gestionnaire de paquets JavaScript |
| **Apache** | 2.4.58 ou supérieur | Version testée ; un autre serveur web compatible PHP-FPM devrait fonctionner |

### Extensions PHP recommandées

| Extension | Rôle |
|---|---|
| `ext-ctype` | Validation de chaînes |
| `ext-iconv` | Conversion d'encodages |
| `ext-pdo` / `ext-pdo_mysql` | Accès base de données |
| `ext-json` | (Dé)sérialisation JSON |
| `ext-mbstring` | Chaînes multi-octets (UTF-8) |

## Base de données

Nathéo CMS prend en charge deux SGBD :

| SGBD | Version requise | Notes |
|---|---|---|
| **MySQL** | 8.2 ou supérieur | *Par défaut.* Storage engine **InnoDB** requis — MyISAM fonctionne mais certaines fonctionnalités (tests unitaires, etc.) peuvent être impactées |
| **PostgreSQL** | 15.2 ou supérieur | Nécessite un changement de configuration |

> 💡 **Astuce** — Pour utiliser PostgreSQL à la place de MySQL, suivez la [procédure de changement de base de données](base_de_donnees.md).

## Voir aussi
- [Installation en mode développeur](installation_dev.md)
- [Installation via l'installeur](installation_prod.md)
- [Options de configuration](configuration_installation.md)
- [Bases de données prises en charge](base_de_donnees.md)
