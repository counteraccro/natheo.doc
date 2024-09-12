## Options de configuration

[Index](../../index.md > Options de configuration

Voici les options qui peuvent influencer l'installation et le comportement de NatheoCMS

## Ficher service.yaml

Dans le fichier ``config/services.yaml`` retrouver les configurations suivantes :

````yaml
app.default_database_prefix: '' # un "_" est ajouté par défaut, laissez vide si pas de prefix
app.default_database_schema: 'natheo'  # schéma de la base de donnée
app.version: 'dev-04-2024'
app.current_branche: 'master'
# Permet d'activer le mode débug du CMS
# En débug mode, créer un type d'utilisateur par droit dans les fixtures
app.debug_mode: true
````
## Fichier .env

````yaml
###> Config environnement ###
APP_ENV=prod # Défini si l'environnement du CMS est en dev (+de log) ou prod
APP_DEBUG=0 # Lier à APP_ENV
APP_SECRET=93df0a91243d55a4d72a801726b38645 # Généré automatiquement à l'installation
##< Config environnement ###

###> Config database ###
DATABASE_URL="postgresql://app:!ChangeMe!@127.0.0.1:5432/app?serverVersion=16&charset=utf8"
###< Config database ###

###> symfony/messenger ###
MESSENGER_TRANSPORT_DSN=doctrine://default?auto_setup=0
###< symfony/messenger ###

###> symfony/mailer ###
 MAILER_DSN=null://null
###< symfony/mailer ###
````
