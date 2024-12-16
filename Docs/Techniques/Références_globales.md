[Index](../../index.md) > [Documentation technique](index.md) > Références globales

*Liste des références sur le CMS*

### Pour les Pages

#### Rendu de la page
Permet d'afficher le rendu général de la page. 
Défini par la clé 'render' dans l'API [find page](../API/References/find_page.md)

* 1 => Affichage du rendu mode 1 block
* 2 => Affichage du rendu mode 2 block côte à côte
* 3 => Affichage du rendu mode 3 block côte à côte
* 4 => Affichage du rendu mode 2 bloc l'un en dessous de l'autre
* 5 => Affichage du rendu mode 3 bloc l'un en dessous de l'autre
* 6 => Affichage du rendu mode 1 block au dessus + 2 block côte à côte dessous
* 7 => Affichage du rendu mode 2 block côte à côte + 1 block en dessous
* 8 => Affichage du rendu mode 2 block côte à côte + 2 block côte à côte en dessous

#### Les types de content
Permet de définir le type de content à afficher. 
Définie par la clé 'type' dans l'API [find page](../API/References/find_page.md), block 'contents'

* 1 => Type texte pour les pages contents
* 2 => Type FAQ pour les pages contents
* 3 => Type LISTING pour les pages contents

#### Catégorie de page
Permet de définir le type de page.

* 1 => Catégorie page
* 2 => Catégorie article
* 3 => Catégorie projet
* 4 => Catégorie blog
* 5 => Catégorie évènement
* 6 => Catégorie news
* 7 => Catégorie évolution
* 8 => Catégorie documentation
* 9 => Catégorie faq

### Pour les menus

#### Position globale du menu
Défini la position globale du ménu

* 1 => Position header
* 2 => Position droite
* 3 => Position footer
* 4 => Position gauche

#### Type menu header
Défini le type de menu header.
Défini par la clé 'type' dans l'API  [find page](../API/References/find_menu.md), block 'type'

* 1 => Menu header de type side bar
* 2 => Menu header de type menu déroulant
* 3 => Menu header de type menu déroulant, big menu
* 4 => Menu header de type menu déroulant, big menu 2 colonnes
* 5 => Menu header de type menu déroulant, big menu 3 colonnes
* 6 => Menu header de type menu déroulant, big menu 4 colonnes

#### Type menu gauche / droite
Défini le type de menu gauche ou droite.
Défini par la clé 'type' dans l'API [find page](../API/References/find_menu.md), block 'type'

* 11 => Menu gauche - droite side-bar
* 12 => Menu gauche - droite side-bar accordéon

#### Type menu footer
Défini le type de menu footer.
Défini par la clé 'type' dans l'API [find page](../API/References/find_menu.md), block 'type'

* 16 => Menu footer 4 colonnes
* 17 => Menu footer 1 ligne à droite
* 18 => Menu footer 1 ligne centrée