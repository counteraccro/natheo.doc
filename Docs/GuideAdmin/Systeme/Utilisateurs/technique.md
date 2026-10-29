---
title: "Référence technique"
parent: "Gestion des utilisateurs"
grand_parent: "Guide d'administration"
nav_order: 2
---

Vue d'ensemble technique du module de gestion des utilisateurs (back-office). Pour l'usage côté back-office, voir
le [listing](listing.md) et le [formulaire d'ajout/édition](ajouter_editer.md).

## Backend

| Classe | Rôle |
|---|---|
| [`UserController`](https://github.com/counteraccro/natheo/blob/master/src/Controller/Admin/System/UserController.php) | Contrôleur unique, gère à la fois la gestion des utilisateurs par un super-administrateur (`index`/`add`/`update`/`delete`/`updateDisabled`/`switch`/`sendResetPassword`) **et** la gestion de son propre compte (`updateMyAccount`, `updatePassword`, `selfDisabled`, `selfDelete`…, voir [Profil](../../MonCompte/profil.md)) |
| [`UserService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/System/User/UserService.php) | Formatage Grid, création (`addUser`), anonymisation (`anonymizer`), recherche par rôle (`getByRole`) |
| [`User`](https://github.com/counteraccro/natheo/blob/master/src/Entity/Admin/System/User.php) | Entité (table `user`) |
| [`UserRepository`](https://github.com/counteraccro/natheo/blob/master/src/Repository/Admin/System/UserRepository.php) | Requêtes (pagination/recherche Grid, recherche globale, résolution par rôle) |
| [`Role`](https://github.com/counteraccro/natheo/blob/master/src/Utils/System/User/Role.php) | Constantes des 4 rôles + vérification de rôle sur une instance `User` |
| [`Anonymous`](https://github.com/counteraccro/natheo/blob/master/src/Utils/System/User/Anonymous.php) | Logique d'anonymisation d'un compte |
| [`UserAddType`](https://github.com/counteraccro/natheo/blob/master/src/Form/Admin/User/UserAddType.php) / [`UserType`](https://github.com/counteraccro/natheo/blob/master/src/Form/Admin/User/UserType.php) | Formulaires Symfony de création / édition |

## Table `user` (extrait des champs pertinents)

| Champ | Détail |
|---|---|
| `email` | Identifiant de connexion |
| `login`, `firstname`, `lastname` | Données personnelles, toutes optionnelles |
| `roles` | Stocké en JSON (tableau), un seul rôle réellement utilisé en pratique via le formulaire |
| `disabled` | Empêche la connexion si `true` |
| `anonymous` | Positionné à `true` par l'anonymisation, masque toute action dans le listing |
| `founder` | Positionné une seule fois à l'installation, jamais réattribuable depuis l'administration |
| `avatar` | Nom du fichier stocké sur disque, supprimé physiquement lors d'une suppression définitive |

## Rôles

`Role::getListRole()` retourne les 4 rôles proposés dans les formulaires, **y compris `ROLE_SUPER_ADMIN`** : le
formulaire de création/édition ne fait aucune distinction particulière pour ce rôle — n'importe quel
super-administrateur peut donc en créer un autre, ou promouvoir n'importe quel compte existant (hors fondateur)
au rôle Super Administrateur, sans confirmation supplémentaire.

> 🐛 **Bug réel, vérifié en direct sur `dev.natheo:8888`** (inspection du DOM du `<select id="user_roles">`) :
> `templates/admin/system/user/update.html.twig` (et `add.html.twig`) construit la liste déroulante des rôles à
> la main, en itérant `field_choices(form.roles)`, **sans jamais marquer l'option courante comme `selected`**.
> Résultat : à l'ouverture du formulaire d'édition, le navigateur sélectionne par défaut la **première** option de
> la liste (`ROLE_USER`), indépendamment du rôle réellement stocké en base — confirmé sur le compte `contributeur`
> (rôle réel `ROLE_CONTRIBUTEUR`), dont le `<select>` s'ouvre pourtant sur « Utilisateur » sélectionné. Enregistrer
> le formulaire sans reselectionner manuellement le bon rôle rétrograde donc silencieusement le compte au rôle
> Utilisateur. Documenté côté utilisateur dans le [formulaire d'édition](ajouter_editer.md).

Seule la combinaison **`founder = true` et rôle Super Administrateur** (donc uniquement le compte fondateur créé
à l'installation) déclenche les protections particulières détaillées dans le [listing](listing.md) et le
[formulaire d'édition](ajouter_editer.md) : pas de désactivation/suppression/prise de contrôle possible par un
tiers, formulaire d'édition réservé au fondateur lui-même, champs rôle/désactivation masqués.

## Suppression et anonymisation

`UserController::delete()` s'appuie sur deux options système : `OS_ALLOW_DELETE_DATA` (autorise ou non l'action)
et `OS_REPLACE_DELETE_USER` (remplace une suppression définitive par une anonymisation). `Anonymous::anonymizer()`
réinitialise le compte ciblé : prénom `John`, nom `Doe`, login `John Doe`, rôle ramené à `ROLE_USER`, email et mot
de passe remplacés par des valeurs aléatoires, `disabled` et `anonymous` mis à `true`, `founder` mis à `false`, et
toutes les options personnalisées de l'utilisateur (`optionsUser`) supprimées. Une suppression définitive (sans
anonymisation) retire aussi le fichier avatar du disque avant de supprimer la ligne en base.

## Recherche et tri du listing

`UserRepository::getAllPaginate()` filtre sur **email, login, prénom et nom** (pas sur le rôle). Les colonnes
triables sont id/login/email/prénom/dates de création et mise à jour (`KEY_LIST_ORDER_FIELD`).

## Prise de contrôle ("Se connecter en tant que")

`UserController::switch()` redirige vers le tableau de bord avec le paramètre `_switch_user` de Symfony (aucune
vérification supplémentaire au-delà de `ROLE_SUPER_ADMIN` et des conditions d'affichage du bouton). L'action est
journalisée via `LoggerService::logSwitchUser()` sur le canal d'authentification, niveau *avertissement*.

## Réinitialisation de mot de passe et création de compte

Un compte créé par un administrateur reçoit un mot de passe aléatoire de 20 caractères, jamais transmis : un
email (`MAIL_CREATE_ACCOUNT_ADM`) est envoyé avec un lien contenant un jeton à usage unique stocké en tant que
`UserData` (clé `KEY_RESET_PASSWORD`), permettant à l'utilisateur de définir lui-même son mot de passe. Le bouton
**Réinitialiser le mot de passe** du formulaire d'édition régénère ce même jeton et renvoie l'email correspondant
(`MAIL_RESET_PASSWORD`).

## Notifications et emails liés

| Déclencheur | Notification | Destinataire réel |
|---|---|---|
| Création d'un compte par un administrateur | `NOTIFICATION_WELCOME` | Le nouvel utilisateur |
| Un utilisateur désactive/supprime/anonymise **son propre** compte (hors Super Administrateur), voir [Profil](../../MonCompte/profil.md) | `NOTIFICATION_SELF_DISABLED` / `NOTIFICATION_SELF_DELETE` / `NOTIFICATION_SELF_ANONYMOUS` | Le compte **fondateur uniquement** (voir note ci-dessous) |

> ⚠️ **Point relevé dans le code, à connaître avant de s'y fier :** `UserService::getByRole()` délègue à
> `UserRepository::findByRole()`, une méthode explicitement marquée `@deprecated` dans son commentaire
> (« retourne uniquement le fondateur pour le moment, à refaire car `roles` est un champ JSON ») : quel que soit
> le rôle demandé en paramètre, elle renvoie systématiquement `WHERE u.founder = 1`. En pratique, les emails et
> notifications censés partir « à tous les super-administrateurs » (auto-désactivation, auto-suppression,
> auto-anonymisation d'un utilisateur) ne partent en réalité **qu'au compte fondateur**. S'il existe des comptes
> Super Administrateur qui ne sont pas le fondateur, ils ne reçoivent aucune de ces alertes. Voir aussi la
> [référence technique des notifications](../../MonCompte/Notifications/technique.md).

## Voir aussi
- [Listing des utilisateurs](listing.md)
- [Ajouter / modifier un utilisateur](ajouter_editer.md)
- [Notifications — référence technique](../../MonCompte/Notifications/technique.md)
- [Profil (mon compte)](../../MonCompte/profil.md)
