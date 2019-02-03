## installation de raspbian en headless + passage du rootfs sur un disque dur externe
### tags : raspbian, raspberry pi, os, rootfs, headless, ssh, dd, partition, fdisk, resize2fs

#### 1) install de raspbian et config du ssh headless
Etapes :
- téléchargement de l'image raspbian lite stretch : https://www.raspberrypi.org/downloads/raspbian/
- déplacement de l'image : 
    ```
    mv ~/Téléchargements/2018-11-13-raspbian-stretch-lite.zip ~/Documents/Applis_et_updates/Raspberry_pi/2018-11-13-raspbian-stretch-lite.zip
    ```
- unzip : 
    ```
    cd ~/Documents/Applis_et_updates/Raspberry_pi
    unzip 2018-11-13-raspbian-stretch-lite.zip
    ```
- copie de l'image sur une carte microSD (disons sdb) :
    - vérifier que sdb n'est pas monté
    - puis : 
        ```
        dd if=~/Documents/Applis_et_updates/Raspberry_pi/2018-11-13-raspbian-stretch-lite.img of=/dev/sdb bs=4M status=progress conv=fsync && sync
        ```
- activation du server ssh côté pi. On ajoute simplement un fichier nommé `ssh` sur la partition boot (celle en FAT32) de la carte microSD désormais copiée : 
    ```
    fdisk -l ## identifier la paritition boot, la plus petite des deux sur la SD
    mkdir -p /mnt/clef
    mount /dev/sdb1 /mnt/clef
    touch /mnt/clef/ssh
    ```
- si connexion en wifi côté raspberry pi, alors on configure la connection wireless. On créé sur cette même partition boot un fichier `wpa_supplicant.conf` : 
    ```
    touch /mnt/clef/ssh
    ```
    Et on y met : 
    ```
    country=fr
    update_config=1
    ctrl_interface=/var/run/wpa_supplicant

    network={
    scan_ssid=1
    ssid="<wifi_lan_name>"
    psk="<wiki_lan_password>"
    }
    ```
- avec ce niveau de config, le pi se verra allouer une adresse ip dynamique (ie choisie par le routeur) une fois connecté au réseau. Pour se connecter au pi, il faudra au préalable trouver cette adresse :
    - on identifiera le réseau local : 
        ```
        ip address
        ## renvoie notamment, dans la section wlp2s0 : 192.168.0.X/24.
        ## => mon adresse ip locale est 192.168.0.X
        ## => le sous réseau est 192.168.0.0/24) (<=> 192.168.0.*) 
        route -n
        ## renvoie l'adresse de la gateway : 192.168.0.1 dans mon cas
        ```
    - on pourra ensuite scanner le réseau à l'aide de la commande nmap : 
        ```
        nmap -sP 192.168.0.0/24
        ## ou
        nmap -sn 192.168.0.0/24
        ```
        à faire en tant que root pour voir s'afficher les noms des cartes réseaux, et ainsi deviner qui est le pi (affiché comme Raspberry Pi Foundation)
- Pour configurer dès à présent une adresse ip fixe, voir le wiki `network_and_headless_rescue_config`.
    
#### 2) préparation du hdd qui accueillera le rootfs
Etapes : 
- montage du disque (il se trouve être en ntfs) : 
    ```
    mount /dev/sdc1 /mnt/clef
    ```
- copie des fichiers qui étaient présents dessus au cas où (pdf de garantie, numéro de série et autres exécutables Windows) :
    ```
    cp -R /mnt/clef/* /mnt/partage_d/Applis_et_updates/Maxtor_hdd/
    ```
- manipulation des partitions : 
    - démontage du disque : 
        ```
        umount /mnt/clef
        ```
    - création des partitions nécessaires : 
        ```
        fdisk /dev/sdc
        g ## créé une table de partition au format GPT
        n ## on créé une partition primaire qui sera le rootfs
        ## partition number : laisser 1 par défaut
        ## first sector : laisser 2048 par défaut
        ## last sector : entrer : +20G
        F ## pour voir l'espace encore utilisable
        n ## on créé un seconde partition qui sera dédiée au stockage des données
        ## partition number : laisser 2 par défaut
        ## first sector : laisser la valeur par défaut
        ## last sector : entrer : +910G        
        n ## on créé un seconde partition qui sera dédiée au swap
        ## partition number : laisser 3 par défaut
        ## first sector : laisser la valeur par défaut
        ## last sector : entrer : laisser la valeur par défaut (on finit de remplir le disque)
        t ## on va changer le type de la troisième partition à Linux Swap
        ## partition number : entrer 3
        L ## pour lister l'ensemble des codes de types de partitions
        ## puis trouver le code de Linux Swap (chez nous c'était le 19)
        ## Hex code : entrer 19
        p ## pour vérifier que l'état des partitions nous convient, et pour noter le numéro du disque (disk identifier)
        w ## on écrit les changements
        ```
    - formatage des partitions en ext4 : 
        ```
        mkfs -t ext4 /dev/sdc1
        mkfs -t ext4 /dev/sdc2
        ```
        
#### 3) connexion au pi

L'histoire de l'adresse ip fixe déterminée n'a pas fonctionné. Ou alors je n'ai pas tout compris... Toujours est-il qu'on va devoir scanner le réseau local pour trouver le pi.

Etapes : 
- chercher le pi
    ```
    nmap -sP 192.168.0.0/24
    ## on localise l'adresse du pi, deux lignes au dessus (oui oui, au dessus !) de la ligne "Raspberry Pi foundation"
    ```
- s'y connecter : 
    ```
    ssh pi@192.168.0.26 ## bien préciser pi@, car autrement on finit en root puisqu'on est actuellement en root sur notre machine, hors on n'a pas encore défini le mot de passe pour root...
    ## le mot de passe par défaut pour l'utilisateur pi est raspberry
    ```
- sur le Pi, se mettre en root : 
    ```
    sudo su
    ```

#### 4) déplacement du rootfs (opérations sur le pi)
- on branche le hdd désormais formaté sur le pi
- on cherche son sdX avec fdisk -l : chez nous, ce sera sda
- on recopie le rootfs actuellement sur la SD sur la partition numéro 1 créée plus tôt : 
    ```bash
    ## on monte le disque
    mkdir -p /mnt/hdd
    mount /dev/sda1 /mnt/hdd
    ## on monte en ro la partition / que l'on va copier
    mkdir -p /mnt/rootfs_ro
    mount --bind -o ro / /mnt/rootfs_ro
    ## on prépare la copie
    shopt -s dotglob ## option bash : permet de considérer les fichiers cachés comme des fichiers normaux, et donc des les copier comme les autes
    ## on copie !
    rm -rf /mnt/hdd/*
    cp -r --archive /mnt/rootfs_ro/*  /mnt/hdd
    ```
- ensuite, on modifie le fichier cmdline.txt présent sur la première partition de la SD /dev/mmcblk0p1, par défaut montée dans /boot. L'idée est de modifier le référencement du rootfs : 
    - trouver le PARTUUID de sda1 : 
        ```
        ls -l /dev/disk/by-partuuid/
        ## ce sera 2f4b1c45-4592-434d-b74d-bba462874700
        ```
    - modifier le /boot/cmdline.txt en conséquence : 
        ```
        cd /boot
        nano cmdline.txt
        ```
        de sorte d'avoir : 
        ```
        root=PARTUUID=2f4b1c45-4592-434d-b74d-bba462874700 
        ## à la place de root=/dev/mmcblk0p2. X est la lettre du hdd, et Y le numéro de la partition sur laquelle on a copié le rootfs    
        
        ## on aurait pu écrire root=/dev/sda1, mais en cas de nouveau disque monté sur le pi l'ordre des affectations de disque risque de changer, transformant notre sda et sdb, et cassant de fait la procédure de boot...
        ```
- enfin, on modifie le /etc/fstab () pour déclarer le changement de montage (c'est bien le /etc/fstab présent sur le hdd que l'on modifie, pas celui de la SD puisque le pi n'y cherchera désormais plus le rootfs) : 
    ```
    nano /mnt/hdd/etc/fstab
    ```
    de sorte d'avoir :
    ```
    /dev/mmcblk0p1    /boot           vfat    defaults,ro       0       2
    #/dev/mmcblk0p2   /               ext4    defaults,noatime  0       1
    ## on remplace mmcblk0p2 par notre sdXY : 
    PARTUUID=2f4b1c45-4592-434d-b74d-bba462874700  /               ext4    defaults,noatime  0       1
    ```
    On en a aussi profité pour mettre la partition mmcblk0p1 en ro, histoire de vraiment préserver la sd, même sur sa partie /boot
- reboot : 
    ```
    reboot
    ```
- on patiente
- reconnexion en ssh : 
    ```
    nmap -sP 192.168.0.0/24 ## pour vérifier
    ssh pi@192.168.0.26
    ```
- on checke la taille de la partition root : 
    ```
    df -h
    ## si root renvoie 20Go et non pas 1.8Go comme précédemment sur la SD, on est bons ! --> on est bons :)
    ```
