## configuration de partage_d, le disque ntfs partagé avec windows
### tags : ntfs, partage, montage, mount, fstab, windows, partition

## les paquets à avoir pour que ça se passe bien 
Il faut avoir installé le paquet ntfs-3g, qui permet la gestion performante en lecture et écriture des partitions ntfs (pour nous, il s'était installé automatiquement comme dépendance de spacefm)

## comment monter automatiquement la partition correspondant au disque D: de Windows ?

Etapes : 
    - identifier la partition (ici, ce sera la 6 qui nous intéressera): 

    ```
    # fdisk -l
    Device         Start       End   Sectors   Size Type
    (...)
    /dev/sda5  408887296 410759167   1871872   914M Windows recovery environment)
    /dev/sda6  410761216 976771071 566009856 269,9G Microsoft basic data
    /dev/sda7  220143616 259205119  39061504  18,6G Linux filesystem
    (...)
    ̀̀```
    - trouver son uuid : 

    ```
    # blkid : 
    (...)
    /dev/sda6: LABEL="Disque local" UUID="6A285BDB285BA4BB" TYPE="ntfs" PARTLABEL="Basic data partition" PARTUUID="dc538ff9-07f7-415c-98de-74341d438006"
    (...)
    ```

    - le monter dans le fichier /etc/fstab. Y ajouter la ligne suivante : 

    ```
    UUID=6A285BDB285BA4BB   /mnt/partage_d  ntfs-3g rw,user,auto,exec,gid=1000,uid=1000,umask=002,utf8,codepage=850,shortname=mixed 0   0
    ```

    - monter l'ensemble des lignes du fstab : 
    ```
    mount -a
    ```

__Et si ça ne cuit pas ?__
Il est possible que mount -a nous crie dessus, en prétextant que le disque contient un système de fichier non propre. Comprendre que les fonctionnalités de démarrage rapide et de veille prolongée de windows sont gérées par le disque de manière un peu bizarre (en écrivant sur la partition en partant de la droite notamment)

Mais alors que faire ?
--> en solution oneshot, on va utiliser ntfsfix :

```
# ntfsfix /dev/sda6
mounting volume... The disk contains an unclean file system (0, 0).
Metadata kept in Windows cache, refused to mount.
FAILED
Attempting to correct errors... 
Processing $MFT and $MFTMirr...
Reading $MFT... OK
Reading $MFTMirr... OK
Comparing $MFTMirr to $MFT... OK
Processing of $MFT and $MFTMirr completed successfully.
Setting required flags on partition... OK
Going to empty the journal ($LogFile)... OK
Checking the alternate boot sector... OK
NTFS volume version is 3.1.
NTFS partition /dev/sda6 was processed successfully.

# umount /mnt/partage_d
# mount -a
```

--> pour une solution pérenne, on a finalement désactivé définitivement les fonctions de veille prolongée et de démarrage rapide côté windows (cf wiki install_debian_dual_boot) : 

- supprimer temporairement la fonctionnalité de veille prolongée (qui occupe un espace mémoire dédié à la fin de la partition C) : 
    - dans un cmd admin, taper : `powercfg -h off`
- supprimer temporairement l'espace swap, qui se trouve également en fin (ie à droite) de la partititon : 
    - manip à faire en tant qu'admin
    - panneau de configuration / système / à gauche cliquer sur Paramètres système avancés
    - onglet paramètres système avancés section Performances (Effet visuels...) cliquer sur "Paramètres..."
    - onglet Avancé, Dans la zone Mémoire virtuelle, cliquez sur le bouton Modifier
    - sélectionner C: et choisir Aucun fichier d'échange -> Définir (NB : avant modif, on était en réglage automatique, soit la première case cochée tout en haut de la fenêtre)
