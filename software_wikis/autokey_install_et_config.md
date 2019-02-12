## install et configuration de autokey
### tags : raccourcis clavier, emulation, touches, autokey, deplacement, keybind

#### installation :
Comme il est dit sur le github du projet (https://github.com/autokey/autokey), il y a plusieurs façon de faire : 
- via pip
- via aptitude
- via aptitude pointant sur un ppa

Nous avons opté pour la méthode aptitude, mais au bout de compte, ça ne change pas grand chose : il faut quand même installer d'autres dépendances à la main.

Etape de la méthode aptitude : 
- aptitude install autokey-gtk
- aptitude install libdbus-1-dev ## pour que autokey puisse utiliser le protocle bus pour communiquer avec le système. Cette dépendance ne s'installe pas toute seule
- aptitude install dbus-x11 ## pour avoir la commande dbus-launch, nécessaire au lancement de autokey
- création du fichier ~/.Xauthority (va savoir pourquoi sachant que Autokey ne le remplira jamais ?
- et là normalement, on peut appeler autokey-gtk et voir apparaître l'icône dans la barre de status

#### lancement en automatique avec i3
Pour lancer autokey au démarrage d'une session, on va ajouter la ligne suivante quelque part dans le `.config/i3/config` : 

```
exec /bin/bash -c "sleep 3; autokey > /dev/null 2>&1 &"
```

Pourquoi le lancer avec bash ?
Parce que bash a cette capacité de pouvoir détacher les programmes du terminal à partir duquel ils ont été lancés, ce que zsh ne sait pas faire (ou alors à configurer).

Pour ce faire, il faut avoir donné au programme une autre sortie que la console (ici /dev/null) et l'avoir lancé en arrière plan (&)

Pourquoi attendre avant de le lancer ?
Parce qu'il arrive parfois que le lancement plante si on le lance en même temps que i3.

#### gestion des logs
à chaque raccourci activé, autokey génère une ligne de log dans le fichier ~/.config/autokey/autokey.log
Une fois ce fichier à 5M, il est historisé (ie devient autokey.log.1, autokey.log.2, etc.) et un nouveau autokey.log est créé et appendé.
Pour éviter une écriture disque (inutile ici) à chaque utilisation d'un keybinding, on fait pointer autokey.log vers /dev/null avec un lien symbolique : 
```
ln -s /dev/null ~/.config/autokey/autokey.log
```
