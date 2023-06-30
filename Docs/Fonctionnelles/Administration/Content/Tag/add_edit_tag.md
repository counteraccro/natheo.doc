# Créer / Editer un tag

[Index](../../../../../index.md) > [Documentation fonctionnelle](../../../index.md) > [Administration](../../index.md) > [Gestion des tags](tag.md) > Créer / editer un tag

*Permet de pouvoir créer/ éditer un tag*

## Nouveau tag
![Nouveau tag](../../files/tag/new_tag.png)

Cette page permet de créer un nouveau tag

### Règles de gestions globales

* Le champ couleur
  * Le champ est obligatoire
  * La valeur doit être une valeur hexadecimal sous la forme #rrggbb
* Le champ label
  * Le champ est obligatoire
  * Il existe autant de champ label que de langue

Le bouton "Exemple de couleur" permet d'afficher un exemple de 5 couleurs défini de façon aléatoire
Le bouton "reload" permet de générer 5 nouvelles couleurs aléatoires
Le choix d'une couleur met à jour la valeur du champ couleur.

L'option "dupliquer le label pour les autres langues" est par défaut activé.
Cette option permet de dupliquer le label de la langue courante vers les autres langues sans intervention de l'utilisateur 

Un aperçu en temps réel est disponible et ce met à jour de façon dynamique au changement de couleur ou de label

Au clic sur le bouton créer 
* Création du Tag
  * Le champ create_at est mis à jour à la date du jour au format [aaaa-mm-jj hh:mm:ss]
  * le champ disabled est mis à false
  * Pour chaque langue, création d'un tagTranslate avec la locale et le label


## Editer un tag
![Editer un tag](../../files/tag/edit_tag.png)

Meme règle de validation que [Nouveau tag](#nouveau-tag)

L'option "dupliquer le label pour les autres langues" est par défaut désactivé.

Au clic sur le bouton modifier
* Edition du Tag identifié par son id
  * Le champ update_at est mis à jour à la date du jour au format [aaaa-mm-jj hh:mm:ss]
  * Pour chaque langue, modification du tagTranslate avec le label modifié