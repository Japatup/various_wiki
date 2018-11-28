## configuration de la i3bar avec i3blocks
### tags : bar, i3bar, i3blocks, icon, fonts-font-awesome

#### configuration basique de la i3bar

la config de la i3bar se fait dans le fichier de config de i3, dans la section "bar"

Dans cette section on trouvera notamment le choix de la police utilisée, l'icône à utiliser comme séparateur, et ici le renvoi vers le fichier de conf de i3blocks, où on paramètrera tout le reste :

```
status_command i3blocks -c ~/.config/i3/i3blocks.conf
```

#### Installer i3blocks

i3blocks est un wrapper pour i3bar, permettant de scheduler simplement la mise à jour des différentes composantes de la barre de statut apparaissant au bas de l'écran (par défaut). 

Sur debian, on peut l'installer directement avec aptitude.

#### configurer les blocks

##### les scripts

Par défaut, on configure i3blocks dans le fichier de config `~/.i3blocks.conf`.

On a préféré déplacer ce fichier de config dans le même dossier que la config de i3, pour que tout soit centralisé. On a donc le fichier `~/.config/i3/i3blocks.conf`, qui est utilisé car déclaré dans le fichier config de i3.

Niveau configuration, il est possible de créer des "blocks", soit des sections de la barre de statut qui doivent renvoyer telle ou telle information après avoir exécuté tel ou tel programme. Par défaut dans notre conf i3blocks, la section "bidule" cherchera à exécuter le script "bidule" présent dans le dossier `~/Documents/01_scripts_admin/scripts_i3blocks`.

Dans ce dossier se trouve également le dossier default_scripts, où sont présents une copie des scripts livrés par défaut avec i3blocks, et initialement placés dans `/usr/share/i3blocks`.

Ces scripts sont pointés par des liens symboliques du dossier parent lorsqu'on souhaite les conserver (ie conserver les scripts par défaut de i3blocks), et sont remplacés dans le dossier parent par d'autres scripts homonymes lorsqu'on a souhaité écrire nous-même les scripts à utiliser.

##### les icônes
Pour chaque section, il est possible de définir un label, soit un texte qui sera affiché en début de block pour permettre l'identification du dit block. Par exemple, la section batterie peut commencer par le mot clef "BAT".

En guise de label, on a préféré utiliser des icônes, pour faire plus joli et pour facilier la lecture de la barre de statut.

Ces icônes proviennent de la police fonts-font-awesome qu'il a fallu installer au préalable (`aptitude install fonts-font-awesome`). (NB : c'est la version 4.7 et non la version 5 qui est stockée sur les dépôts debian 9.

En conséquence, la liste des icones est à aller chercher sur cette page : https://fontawesome.com/v4.7.0/icons/)
Une fois la police installée, on choisit de l'utiliser pour la barre de statut : dans le fichier `~/.config/i3/config`, on l'ajoute aux polices utilisables : 

```
bar {
  (...)
  font pango:DejaVu Sans Mono, Awesome 8
  (...)
}
```

Ensuite, on insère les icônes dans le fichier de configuration de i3, en guise de label. Pour ce faire, on peut soit copier coller les icônes depuis la page https://fontawesome.com/v4.7.0/icons/, soit les insérer à partir de leur code unicode. Ceci est possible avec vim notamment : 

  - on se place à l'endroit voulu
  - on entre en mode insertion
  - ctrl+v
  - u
  - on entre le code unicode correspondant à l'icône choisie (par exemple f012 pour l'icône du signal réseau)