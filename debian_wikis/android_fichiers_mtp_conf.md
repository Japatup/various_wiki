## partage de fichiers avec les terminaux android : en MTP
### tags : mtp, fs, usb, mount, android, xperia, fichiers, transfert, jmtpfs

#### comment échanger des fichiers avec un appareil android ?

Le montage standard avec `mount` ne fonctionne pas, car aucun nouveau disque de type sdb ou sdc ne s'affiche avec fdisk -l après la connexion usb

--> On utilise le protocole MTP du téléphone :

- à la connexion, choisir (sur le tel) d'utiliser le transfert en MTP
- avoir installé jmtpfs : aptitude install jmtpfs
- créer le point de montage (en root ou sudo) : mkdir -p /mnt/tel_mtp
- monter le téléphone : jmtpfs /mnt/tel_mtp -o allow_other -o uid=1000 -o gid=1000
- la mémoire du tel est désormais accessible de façon standard avec l'explorateur de fichiers sur /mnt/tel_mtp
- pour démonter le téléphone : un bon vieux umount /mnt/tel_mtp suffit.

#### règle sudo pour monter les devices mtp sans passer par root
pb du jmtpfs : besoin de faire le montage en root pour avoir les droits d'écriture sur le device mtp. Autrement, seule la lecture est autorisée --> on va utiliser sudo et automatiser tout ça.

Démarche : 
- on a créé le script `~/Documents/01_scripts_admin/mount_mtp.sh` (voir repo i3_various_scripts), + symlink dans `/usr/local/bin/mount_mtp`.

    Ce script permet simplement de lancer la commande jmtpfs vue plus haut.
- une fois le script créé, on rend root propriétaire du fichier + seul à posséder les droits d'écriture. On évite ainsi que le script, qui bénéficiera d'une règle sudo en NOPASSWD puisse être modifié.
    ```
    chown root:root mount_mtp.sh
    chmod 755 mount_mtp.sh
    ```

- pour lancer cette commande en sudo, on créé le fichier de conf `/etc/sudoers.d/mount_permissions` : 
    ```
    yourNameHere ALL=(ALL) NOPASSWD:/usr/local/bin/mount_mtp
    ```
- il est désormais possible pour le user yourNameHere de lancer le script mount_mtp. On créé un simple alias dans ~/.zshrc : 
    ```
    alias mount_mtp="sudo mount_mtp"
    alias umount_mtp="sudo mount_mtp -u"
    ```

*Voir le wiki sudo pour les détails*
