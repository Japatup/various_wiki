## montage de data1 (895 Go) et du swap (1,5 Go)
### tags : hdd, mount, fstab, data1, swap, dphys, swapfile

#### montage de sda2 sur /mnt/data1
##### contexte : 
On a utilisé la premire partition du disque dur externe en guise de partition de boot (voir le wiki `install_raspbian_headless_on_hdd`)

On veut désormais monter la seconde partition (895 Go) sur /mnt/data1 : 

##### Modop 
- créer le dossier de montage : 
    ```
    mkdir -p /mnt/data1
    ```
- trouver (et noter) le partuuid (ou l'uuid) de sda2 : 
    ```
    blkid
    ```
- ajouter le montage dans `/etc/fstab`. On ajoute la ligne suivante : 
    ```
    PARTUUID=83847c4b-85f0-47f1-8b08-c7604394ef93 /mnt/data1 ext4 defaults 0 2
    ```
- appliquer fstab pour vérifier : 
    ```
    mount -a
    ```

#### montage du swap

##### contexte : 
La troisième partition du hdd est dédiée au swap. On va désormais tâcher de la monter

##### modop :
- on récupère le partuuid  et l'uuid de sda3 : 
    ```
    blkid
    ```
- dans `/etc/fstab`, on ajoute la ligne suivante, en utilisant le partuuid : 
    ```
    PARTUUID=3eb77420-ab31-4a16-8b35-6eec5e6765f0 none swap sw 0 0
    ```
- puis on déclare le swap : 
    ```
    mkswap /dev/sda3
    ```
- enfin, on active le swap, en utilisant l'uuid
    ```
    swapon -U a2807138-b4ef-4b97-8328-0f0def17a429
    ```
- on vérifie que le swap est bien pris en compte : 
    ```
    free -h
    ```
- on peut ensuite supprimer le fichier swap par défaut, et désactiver le comportement de fichier de swap géré par dphys-swapfile.service, puisqu'on utilise désormais fstab pour monter une partition dédiée au swap, et activée avec swapon :
    ```
    dphys-swapfile swapoff
    rm /var/swap
    systemctl stop dphys-swapfile.service
    systemctl disable dphys-swapfile.service
    ```