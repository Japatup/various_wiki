## configuration airplane mode : comment activer et désactiver le réseau avec fn F2 et activer la del qui va bien sur la touche f2
### tags : del, led, network, reseau, mode avion, raccourci clavier, nmcli, network manager

#### Comment associer fn+F2 à l'activation / désactivation du réseau
via un script : ~/Documents/01_scripts_admin/plane_mode.sh (voir repo i3_various_scripts)

Ce script permettra plusieurs choses :

- que tous les users puissent écrire dans le fichier `/sys/class/leds/asus-wireless::airplane/brightness`
- de modifier le statut nmcli networking, et de synchroniser la led du mode avion

Il sera placé (via un lien) dans `/usr/local/bin` et prendra le nom `plane_mode`

On autorisera (pour chaque boot) l'ensemble des users à utiliser le script avec la manip suivante : 
```bash
echo "/bin/bash /usr/local/bin/plane_mode allowusers" >> /etc/rc.local
```

et enfin, on ajoutera un alias à notre `.zshrc` :
```
alias "plane_mode"="/bin/bash /usr/local/bin/plane_mode"
```
Le script pourra notamment prendre les valeurs suivantes : 

```bash
plane_mode toggle 
plane_mode on
plane_mode off
plane_mode allowusers
```

#### gestion des permissions : 

Comme dit plus haut, le script touche à une led, et comme pour le rétroéclairage clavier, seul root a le droit d'écriture dans les fichiers `/sys` qui vont bien --> un service doit commencer par donner les permissions aux différents users, et ce à chaque ouverture de session.

Ce service, ce sera le (déjà configuré) `rc.local` (cf. le wiki sur kbd_backlight).

On y placera la commande suivante : 

```
/bin/bash -c "/usr/local/bin/plane_mode allowusers"
```
#### synchronisation de la led au démarrage
Au démarrage du pc, la led n'est pas forcément correctement allumée selon l'état des connexions gérées par network-manager.

Pour y remédier, on peut lancer la commande 
```
/bin/bash -c "/usr/local/bin/plane_mode sync_led"
```

Dans i3, on peut ainsi ajouter à ~/.config/i3/config : 

```
exec /bin/bash -c "plane_mode sync_led"
```

#### raccourci clavier

Enfin, on ajoutera le raccourci suivant dans ~/.config/i3/config : 
```
bindcode 255 exec plane_mode toggle 
```

(il se trouve que le la touche XF86 relative au plane mode n'est pas reconnue avec un joli nom de symbole. On utilise donc directement son code (255) trouvé grâce à la commande xev pour l'assigner à un raccourci)

