# Les rôles de l'administration

[Index](index.md) > [Documentation fonctionnelle](../index.md) > [Administration](index.md) > Les rôles

*L'accès à la partie admin du site ainsi qu'aux modules administratifs de gestion et contributions sont soumis à des restrictions d'accès par rôle*

L'administration de Nathéo CMS possède 4 niveaux de rôles
* **ROLE_USER**
  * Rôle qui permet de ce connecter à l'administration et de pouvoir modifier ses options
* **ROLE_CONTRIBTEUR**
  * Rôle qui permet de voir ses statistiques et d'ajouter du contenu au site
* **ROLE_ADMIN**
  * Rôle qui permet de gérer les membres du CMS, voir les statistiques globales et ajouter du contenu au site
* **ROLE_SUPER_ADMIN**
  * Rôle le plus élevé, permet d'avoir accès à toute la partie administration du CMS 
  * Il peut gérer les utilisateurs et changer leurs droits
* **ROLE_FONDATEUR**
  * Rôle caché et non modifiable
  * Le role fondateur est attribué uniquement au premier compte crée à l'initialisation du CMS
  * Ce rôle permet de protéger le compte de l'utilisateur de toute modification possible par les utilisateurs ayant le role **ROLE_SUPER_ADMIN**