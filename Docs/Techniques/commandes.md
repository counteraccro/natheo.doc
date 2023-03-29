# Commande Symfony

[Index](../../index.md) > [Documentation technique](index.md) > Commandes Symfony

*Liste des commandes utilisée pour le projet*

### Mise à jour des langues 
`php bin/console translation:extract --force --format=yaml fr`  
*Le projet gère actuellement 3 langues français (fr), Anglais (en) et Espagnol (es), les fichiers de langues sont en yaml*

### Fixtures

#### Génération de toutes les fixtures
`php bin/console doctrine:fixture:load `

#### Génération de fixtures en fonction d'un groupe
`php bin/console doctrine:fixtures:load --group=group1 --group=group2 `  
*Liste des groupes [disponible](composants.md#groupe-de-fixtures-existant)*


### Cache
`php bin/console cache:clear`