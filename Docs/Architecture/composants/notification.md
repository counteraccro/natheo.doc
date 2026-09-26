---
title: "Notification"
parent: "Composants"
grand_parent: "Architecture"
nav_order: 3
---

*Cette page vous explique comment déclencher rapidement une notification
depuis le code. Pour l'usage côté back-office (centre de notifications), voir
le [Guide d'administration](../../GuideAdmin/MonCompte/Notifications/notifications.md)
et sa [référence technique](../../GuideAdmin/MonCompte/Notifications/technique.md).*

Pour ajouter une nouvelle notification :

```php
$notificationService->add(
    $user,
    Notification::NEW_COMMENT->value,
    ['author' => $author, 'status' => $status, 'page' => $pageTitle, 'id' => $commentId],
);
```

La méthode [`NotificationService::add()`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/NotificationService.php)
prend en paramètres :
* l'objet `User` qui recevra la notification
* la clé de la notification, une valeur de l'enum `Notification`
* les paramètres associés, à injecter dans les traductions du titre/contenu

Elle ne fait rien si l'option système `OS_NOTIFICATION` est désactivée (pas
besoin de tester `OptionSystemService::canNotification()` avant l'appel).

Chaque type de notification est défini par un cas de l'enum
[`Notification`](https://github.com/counteraccro/natheo/blob/master/src/Enum/Admin/Global/Notification/Notification.php)
(namespace `App\Enum\Admin\Global\Notification`), avec sa configuration
associée dans un tableau `CONFIG` privé :

```php
// Dans l'enum Notification
case NEW_COMMENT = 'new_comment';

private const CONFIG = [
    self::NEW_COMMENT->value => [
        NotificationKeyConfig::CATEGORY->value => NotificationCategory::COMMENT->value,
        NotificationKeyConfig::LEVEL->value => NotificationLevel::INFO->value,
        NotificationKeyConfig::PARAMETERS->value => [
            'author' => '',
            'status' => '',
            'page' => '',
            'id' => '',
        ],
        NotificationKeyConfig::TITLE->value => 'notification.msg.new_comment.title',
        NotificationKeyConfig::CONTENT->value => 'notification.msg.new_comment.content',
    ],
];
```

Une notification possède :
* une **catégorie** (enum `NotificationCategory` : `comment`, `admin`, `SQL`) — utilisée pour les onglets du centre de notifications
* un **niveau** (enum `NotificationLevel` : `INFO`, `WARNING`, `ALERTE`) qui définit la couleur d'affichage
* un tableau de **paramètres** par défaut, fusionné avec ceux passés à `add()`, puis injectés dans les traductions
* une clé de traduction pour le **titre**
* une clé de traduction pour le **contenu**

Pour ajouter un nouveau type de notification, il suffit d'ajouter un cas à
l'enum `Notification` et son entrée correspondante dans `CONFIG`.

L'ensemble des clés de traduction (titres/contenus des notifications, et
libellés de l'interface) sont dans le domaine
`translations/notification+intl-icu.[locale].yaml`.
