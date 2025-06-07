## Idée pour l'avenir

[Index](../index.md) > Evolution et Historique

Liste non exhaustive des futures évolutions du CMS ainsi que son historique d'évolution

### Bugs
* [*Recherche en cours*...]

### Changement mineurs
* Exportation / importation sous forme de CSV des traductions du contenu du site (en base de données)
* ~~Editeur Markdown~~ ([⤵️ #45](https://github.com/counteraccro/natheo/pull/45))
  * Ajouter des liens de page dans l'éditeur de texte
  * Ajouter un bouton qui ouvre un nouvel onglet pour avoir un aperçu du rendu
* ~~Dans les listings, pouvoir filtrer uniquement avec ses contenus~~ ([⤵️ #46](https://github.com/counteraccro/natheo/pull/46))
* Dans le dashboard créer les blocks suivants :
  * ~~Derniers commentaires~~ ([⤵️ #49](https://github.com/counteraccro/natheo/pull/49))
  * Mes dernières pages
  * Mes notifications
* Revoir le code dans DatabaseTablePrefixListener.php pour pouvoir proprement séparer le schéma SQL du nom de la base de données
* Mise en place API pour les commentaires
* Mise en place API pour les options systems

### Changement majeur
* Mise en place d'un système de versioning pour les pages
  * Pouvoir éditer une page publier sans modifier la page actuellement publier
* Gérer les mises à jour des traductions du CMS (fichier yaml)
  * Créer une copie des traductions actuelles pour les sauvegarder
  * Proposer un merge automatique des traductions actuelles vers les nouvelles pour ne pas perdre les éventuelles modifications faites par l'utilisateur.

### Nouveau module
* Module calendrier
  * Pouvoir gérer une prise de rendez-vous depuis le CMS
  * Gérer les rdv
* Module de formulaire
  * Pouvoir créer un formulaire
  * Gérer le contenu de celui-ci + les réponses possibles

### Divers
 * APIsation du back-office
 * Refonte design back-office
 * Utilisation des DTO au lieu des objets Doctrine pour les API
 * ~~Ajouter nouveau droit~~
   * ~~Ne vois que ses propres contenus créés.~~( [⤵️ #46](https://github.com/counteraccro/natheo/pull/46))