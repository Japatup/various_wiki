## réglages des écrans : gestion des projecteurs, du multiscreen, de la luminosité, de la définition et autres
### tags : ecran, luminosite, projecteur, screen, multiscreen, definition

#### contrôle de la luminosité
se fait très simplement avec la commande xbacklight : 
- xbacklight -get ## récupère la valeur de la luminosité actuelle (entre 0 et 100)
- xbacklight -inc 10 # y ajoute 10
- xbacklight -dec 10 # y soustrait 10

**Le seul souci** : xbacklight n'hésite pas à atteindre le 0, même s'il gère seul la borne supérieure du 100. 

De fait, on ne peut pas rester le doigt appuyé sur le bouton en se disant qu'on va buter sur la valeur min : l'écran finit par s'éteindre.

On a donc écrit un script pour borner par le bas la valeur de la luminosité.

Ce script est le ~/Documents/01_scripts_admin/screen_backlight.sh, et il est représenté par un lien symbolique dans /usr/local/bin.

Dans i3 config, le raccourci clavier est donc : (en attendant qu'on parvienne à mapper les touches Fn de luminosité)
- `bindsym $mod+F5 exec --no-startup-id screen_backlight dec` *pour diminuer la luminosité*
- `bindsym $mod+F6 exec --no-startup-id screen_backlight inc` *pour augmenter la luminosité*


#### contrôle du gamma (finalement non utilisé)
(on pensait au départ faire le taf avec xrandr, qui n'agit pas sur la luminosité matérielle mais sur le gamma...)

*NB : la commande xrandr vient avec le paquet x11-xserver-utils*

Cette commande permet de définir le gamma, mais pas directement de la régler en relatif (en mode +-5%).

Le gamma varie ici dans l'intervalle [0;+oo], sachant qu'à 3, tout est déjà trop blanc --> la valeur par défaut est 1.

On peut de fait procéder en récupérant la valeur de la luminosité actuelle avec 
```
xrandr --verbose |grep -i brightness| cut -d: -f2
```
puis en ajoutant ou soustrayant une valeur fixe (0.1 par ex) : 

```
xrandr --output eDP-1 --brightness $(($(xrandr --verbose |grep -i brightness| cut -d: -f2)+0.1))
```

On ajoute tout de même un test pour ne pas pouvoir descendre sous les 0.3 (car à 0, l'écran est noir...)
--> cf. le script monté dans `~/Documents/01_scripts_admin/old_scripts/screen_gamma_old.sh`

### Dans les faits : 
On utilisera le script ~/Documents/01_Scripts_admin/screen_gamma.sh.

De la même façon que kbd_backlight, ce script sera accessible par lien symbolique pointant sur `/usr/local/bin/screen_gamma`

Ce script attend en argument soit : 
- up
- down
