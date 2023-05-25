### Ajout d'une nouvelle notification

[Index](../../../index.md) > [Documentation technique](../index.md) > Notification

*Cette page vous explique comment mettre en place rapidement une nouvelle notification*

Pour ajouter une nouvelle notification

```php
$notificationService->add(
       $user,
       NotificationKey::NOTIFICATION_SELF_DISABLED,
       ['login' => $user->getLogin()]
 );
```
La fonction prend en paramètres les valeurs suivantes
* l'objet User qui recevra la notification
* la clé de la notification
* les paramètres associés à ajouter dans les traductions

Chaque notification est définie par une clé dans la classe NotificationKey

```php
// Dans la class NotificationKey
/**
     * Notification désactivation par l'utilisateur lui-même
     * @const
     */
    const NOTIFICATION_SELF_DISABLED = 'NOTIFICATION_SELF_DISABLED';
    
    const TAB_NOTIFICATIONS = [
        self::NOTIFICATION_SELF_DISABLED => [
            self::KEY_PARAMETER => [
                'login' => '',
            ],
            self::KEY_TITLE => 'notification.msg.self_disabled.title',
            self::KEY_CONTENT => 'notification.msg.self_disabled.content',
            self::KEY_LEVEL => self::LEVEL_WARNING
        ],
    ];
```
Une notification possède :
* Un tableau de paramètres contenant la liste des paramètres défini dans les traductions
* Une clé de traduction pour le titre
* Une clé de traduction pour le contenu
* Un niveau qui va définir la couleur d'affichage

L'ensemble des clés de traductions sont présentes dans le fichier ``translations/notification+intl-icu.[locale].yaml``

