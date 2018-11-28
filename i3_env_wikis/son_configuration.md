## configuration du son
### tags : pulse audio, pactl, volume, hdmi, pavucontrol, sink, default sink, sink par defaut

#### 1) gérer le volume

##### avec volume_main.py : 

On a monté le script ~/Documents/01_scripts_admin/volume_main.py, qui s'occupe automatiquement de réaliser la procédure ci dessous, à savoir détecter le sink en cours d'utilisation (ou sink par défaut si aucun sink running) et d'agir sur le volume de ce dernier. Ce script a été copié en symlink dans /usr/local/bin/volume_main

On notera que pour que le sink par défaut doit potentiellement avoir été défini manuellement au moins une fois, avec la commande :
```bash
pactl set-default-sink <nom du sink>
## (le nom du sink se retrouve facilement par autocomplétion)
```

##### à la main : 
- soit par pavucontrol
- soit en ligne de commande :
    - trouver le sink actif (son nom ou son numéro) : par analyse de la sortie de 
    ```bash
    pactl list sinks
    ```
    - ce qui nous donne notamment le RUNNING SINK : 
    ```
    Sink #44
        State: RUNNING
        Name: alsa_output.pci-0000_00_1b.0.analog-stereo
        Description: Audio interne Stéréo analogique
    (...)
    ```

    - puis controler le volume : 
    ```bash
    pactl set-sink-volume 44 120%

    ## ou avec le nom du sink : 
    pactl set-sink-volume alsa_output.pci-0000_00_1b.0.analog-stereo 100%

    ## ou en relatif : 
    pactl set-sink-volume 44 -5%
    pactl set-sink-volume 44 +5%

    ## ou enclencher le mute :
    pactl set-sink-mute 44 toggle
    ```

#### 2) Envoyer le son vers la sortie hdmi

- via pavucontrol : 
    - dans le menu configuration, choisir "éteint" pour le bloc sortie locale (ça doit être le deuxième bloc), et hdmi output sur l'autre bloc. Cela va avoir pour effet de switcher le son vers l'hdmi
- en ligne de commande :
    - avec la commande 
    ```bash
    pactl move-sink-input 34 alsa_output.pci-0000_00_03.0.hdmi-stereo 

    ## le 34 : correspond au sink-input : change tout le temps (entre deux morceaux youtube par exemple). Le active sink-input peut être récupéré grâce à l'autocomplétion, ou par la commande
    pactl list sink-inputs

    ## le sink vers lequel transférer : à nouveau on peut utiliser son nom (sort en auto via l'autocomplétion) ou ou son numéro, qu'on obtient comme vu plus haut avec
    pactl list sinks
    ```

#### pistes pour programme de gestion du volume : 
pacmd stat : pour la ligne default sink name notamment

pactl list sinks short : pour récupérer plus facilement le sink actif
