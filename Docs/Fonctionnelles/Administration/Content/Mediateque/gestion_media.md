# Gestion des médias

[Index](../../../../../index.md) > [Documentation fonctionnelle](../../../index.md) > [Administration](../../index.md) > [Médiathèque](mediatheque.md) > Gestion des médias

*Gestion des médias dans la médiathèque*

## Nouveau média
![Nouveau média](../../files/mediatheque/modale_new_media.png)

Au click sur le bouton "Nouveau média", ouvre la modale nouveau média

Lorsque vous ajoutez une image depuis le bouton "Choisir un fichier", un aperçu de celle-ci est alors disponible.  
Depuis cet aperçu, vous pouvez aussi choisir le nom du média ainsi qu'une description.

![Nouveau média image](../../files/mediatheque/modale_new_media_img.png)

Dans le cas d'un fichier de type document, l'aperçu n'est pas disponible

![Nouveau média pdf](../../files/mediatheque/modale_new_media_pdf.png)

Actuellement seuls les fichiers avec les extensions suivantes sont téléchargeables dans la médiathèque :
- csv
- pdf
- jpg
- png
- xls
- xlsx
- doc
- docx
- gif

### Règle de gestion des champs
- Champ "choisir un fichier"`
  - Le champ ne peut pas être vide
- Champ "nom du média"
  - Si le champ est vide, met par défaut le nom du fichier
- Champ "description"
  - Pas de règle de gestion

### Actions possibles
**Bouton "choisir un fichier**  
`Permet de sélectionner un fichier depuis votre ordinateur

**Bouton "télécharger"**  
Si les règles de gestions sont valides, alors le fichier est téléchargé sur le serveur est un média est créé en base de donnée

Dans le cas ou l'option OS_MEDIA_CREATE_PHYSICAL_FOLDER est à true alors l'image sera copier dans le dossier courant.

**Bouton "Annuler"**  
Retourne à l'état précédent, supprime l'aperçu du document ainsi que les données saisies

**Bouton "Fermer"**  
Ferme la modale et supprime l'aperçu du document ainsi que les données saisies


## Edition d'un média
![Edition média](../../files/mediatheque/modale_edit_media.png)

Pour éditer un média, sur le média, cliquez sur "Editer"

![Edition média](../../files/mediatheque/edit_media_btn.png)

Ouverture de la modale "Editer un média"

### Règle de gestion des champs
- Champ "nom du média"
  - Si le champ est vide, met par défaut le nom du fichier
- Champ "description"
  - Pas de règle de gestion

### Actions possibles

**Bouton "valider"**  
Au click sur le bouton, met à jour le média concerné ainsi que le path et le webPath

**Bouton "Annuler"**  
Ferme la modale, aucune donnée n'est modifié