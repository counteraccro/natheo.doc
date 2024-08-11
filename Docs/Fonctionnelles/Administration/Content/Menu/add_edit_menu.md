# Créer / Editer un menu

[Index](../../../../../index.md) > [Documentation fonctionnelle](../../../index.md) > [Administration](../../index.md) > [Gestion des menus](menu.md) > Créer / éditer un menu

*Permet de pouvoir créer / éditer un menu*

![Edition d'un menu](../../files/menu/form_menu.png)

### Règles de gestions globales
* Un menu doit posséder au moins 1 élement


### Module de la page
La page est découpés en différent module qui sont :

#### Aperçu en temps réel
Ce petit module permet de voir le rendu de votre menu en temps réel en fonction des données saisies

*Un rendu pour un header*
[![rendu header](../../files/menu/rendu_header.png)]

*Le même menu mais pour un menu à gauche*
![rendu menu gauche](../../files/menu/rendu_menu_gauche.png)

#### Données globales
![module données globales](../../files/menu/form_module_donnee_globale.png)

Configurer ici les données globales du menu
* Le nom du menu (ne peut pas être vide)
* Notion de menu par défaut
  * Si "menu par défaut" est sélectionné alors ce menu apparaitra si une page n'a pas de menu rattaché.
  * Si c'est "menu classique" qui est sélectionné, ce menu n'apparaitra que sur la page auquel il est rattaché
* La position du menu
  * Défini si le menu est un header / footer / menu à gauche ou à droite
* Type de menu
  * Le type de menu dépend de la position du menu. Par exemple pour le header, on peut définir un header classique ou avec menu déroulant ou bien encore en mode big menu

#### Architecture du menu
![Module architecture](../../files/menu/form_module_architecture.png)

Ce module permet de gérer l'architecture du menu comme bon vous semble.

Un menu est défini avec un ensemble d'éléments :
* Chaque élement peut être parent et enfant d'un autre parent.
* Chaque élement s'affiche via un ordre d'affichage défini par row_position
  * Pour les élement de profondeur 1 (Le premier niveau de sous menu), il est possible aussi de les trier par position de colonne
    * Dans ce cas c'est le champ column_position qui est prioritaire dans le trie

**Actions possibles**

![bouton new](../../files/menu/form_tree_btn_new.png) : Permet de créer un nouvel élement dans le parent courant

![bouton new child](../../files/menu/form_tree_btn_new_child.png) : Permet de créer un nouvel élement enfant à l'élément sélectionné