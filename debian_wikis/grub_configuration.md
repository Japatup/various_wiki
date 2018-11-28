## configuration de grub
### tags : grub, boot, demarrage, os, disque dur,partiton, amorcage

le paramétrage de grub se fait dans le fichier :etc/default/grub

#### 1) choix de la durée d'attente par défaut: 
simplement modifier la variable suivante. Ici, on patiente une seconde avant de valider le choix par défaut

```
GRUB_TIMEOUT=1 ## durée de l'attente
GRUB_DEFAULT=0 ## choix par défaut (0 pour le premier choix, 1 pour le second choix etc)

```

Enfin, on lance la commande update-grub afin de modifier le fichier /boot/grub/grub.cfg de 
manière automatique


#### 2) grub après ajout d'un disque dur
*source : https://www.linux.com/learn/how-rescue-non-booting-grub-2-linux%20*

Après ajout d'un nouveau disque, il se peut que le démarrage plante, car le sda sera devenu sdb.

**Solution**  :

- dans grub, taper "c" pour rentrer en console grub
- lister les différents disques : 
    ```
    grub> ls
    (hd0) (hd0,msdos2) (hd0,msdos1), hd1, (...)
    ```
- vérifier quel est le disque (la partition en fait) sur lequel se trouve le /boot : 
    ```
    grub> ls (hd0,1)/
    lost+found/ bin/ boot/ cdrom/ dev/ etc/ home/  lib/
    lib64/ media/ mnt/ opt/ proc/ root/ run/ sbin/ 
    srv/ sys/ tmp/ usr/ var/ vmlinuz vmlinuz.old 
    initrd.img initrd.img.old
    grub> cat (hd0,1)/etc/issue ## pour vérifier la distro sur laquelle on se trouve
    Debian 9.0 \n \l
    ```
- redéfinir manuellement l'emplacement des images linux et initrd à lancer.
    ```
    grub> set root=(hd0,1) ## on précise la partition sur laquelle on a trouvé notre système
    grub> linux /boot/vmlinuz-3.13.0-29-generic root=/dev/sdb1 ## on précise le path de l'image de linux à lancer, ainsi que la partition sur laquelle il faudra la chercher une fois sur debian (potentiellement, c'était sda1 avant ajout du disque, et c'est désormais sdb1, à modifier donc)
    grub> initrd /boot/initrd.img-3.13.0-29-generic ## on spécifie l'emplacement de l'image des librairies additionnelles
    grub> boot ## et on boote !
    ```
- une fois le système retrouvé, pour éviter d'avoir à refaire la manip à chaque fois (surtout que grub console est en qwerty), on met à jour la config grub (en root) : 
    ```
    update-grub
    ```


