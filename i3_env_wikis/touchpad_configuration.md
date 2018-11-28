## configuration du touchpad
### tags : touchpad, clic, libinput, synaptics, tapping

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
