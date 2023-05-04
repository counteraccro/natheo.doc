# Mon profil

[Index](index.md) > [Documentation fonctionnelle](../index.md) > [Administration](index.md) > Mon profil

*Cette page permet de pouvoir gérer vos données personnelles sur le CMS dans la partie administration*

![mon profil](files/mon_profil/mon_profil.png)

## Informations générales
Droit d'accès : **ROLE_USER**  
*Voir [Gestion des utilisateurs](System/user.md#informations-générales)*

## Règles de gestions globales
*Voir [Gestion des utilisateurs](System/user.md#règles-de-gestions-globales)*

## Règles de validation des champs
Les champs modifiables depuis cette page sont :

* Mon profil
  * login
    * Non obligatoire
  * firstname (Prénom)
    * Non obligatoire
  * lastname (Nom)
    * Non obligatoire

* Changement de mot de passe
  * password (Nouveau mot de passe)
    * Obligatoire
    * Règles de gestion
      * 8 caractères minimum
      * Au moins 1 lettre minuscule
      * Au moins 1 lettre majuscule
      * Au moins 1 chiffre
      * Au moins un caractère spécial - #?!@$%^&*-
  * password_confirm (Confirmer nouveau mot de passe)
    * Obligatoire
    * Règles de gestion
        * 8 caractères minimum
        * Au moins 1 lettre minuscule
        * Au moins 1 lettre majuscule
        * Au moins 1 chiffre
        * Au moins un caractère spécial - #?!@$%^&*-

## Regles de gestion des actions

### Mon profil
* Bouton "Modifier mon profil"
    
