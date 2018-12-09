## configuration du touchpad
### tags : touchpad, clic, libinput, synaptics, tapping, toggle, touchpad_toggle.sh

La configuration du touchpad se fait dans le fichier `/usr/share/X11/xorg.conf.d/40-libinput.conf`
(on aurait également pu mettre nos réglages dans un autre fichier du dossier xorg.conf.d,
mais les préréglages de libinput (qui gère notamment le touchpad) étaient déjà renseignés dans 40-libinput.conf)

#### ajout de l'option multitouch qui n'est pas activée par défaut
Dans le fichier de conf, le paragraphe Touchpad doit ressembler à ca : 

```
Section "InputClass"
        Identifier "libinput touchpad catchall"
        MatchIsTouchpad "on"
        MatchDevicePath "/dev/input/event*"
        Driver "libinput"
        Option "Tapping" "on"
EndSection
```

On peut si on le souhaite rajouter les options suivantes, à tester : 

```
#        Option          "MinSpeed"              "0.5"
#        Option          "MaxSpeed"              "1.0"
#        Option          "AccelFactor"           "0.075"
#        Option          "TapButton1"            "1"
#        Option          "TapButton2"            "2"     # multitouch
#        Option          "TapButton3"            "3"     # multitouch
#        Option          "VertTwoFingerScroll"   "1"     # multitouch
#        Option          "HorizTwoFingerScroll"  "1"     # multitouch
#        Option          "VertEdgeScroll"        "1"
#        Option          "CoastingSpeed"         "8"
#        Option          "CornerCoasting"        "1"
#        Option          "CircularScrolling"     "1"
#        Option          "CircScrollTrigger"     "7"
#        Option          "EdgeMotionUseAlways"   "1"
#        Option          "LBCornerButton"        "8"     # browser "back" btn
#        Option          "RBCornerButton"        "9"     # browser "forward" btn
```

#### toggle du touchpad

1) Les commandes : On utilise la librairie xinput (à installer) : 
	- `xinput list` : renvoie la liste des inputs gérés, dont le touchpad (chez nous, c'est "FocalTechPS/2 FocalTech Touchpad")
	- `xinput enable <touchpad_device_name>` : connecte le touchpad
	- `xinput disable <touchpad_device_name>` : déconnecte le touchpad

2) En pratique : on a monté le script `~/Documents/01_scripts_admin/touchpad_toggle.sh`, qui récupère l'état actuel du touchpad (avec `xinput list-props <touchpad_device_name>`) et qui toggle en fonction. Ce script a été copié en symlink dans `/usr/local/bin` pour pouvoir être appelé par tous les users: 
	```
	chmod a+x ~/Documents/01_scripts_admin/touchpad_toggle.sh
	ln -s ~/Documents/01_scripts_admin/touchpad_toggle.sh /usr/local/bin/touchpad_toggle
	```
3) Ajout du raccourci i3 dans `~/.config/i3/config` : 
	```
	bindsym XF86TouchpadToggle exec --no-startup-id touchpad_toggle
	```
	