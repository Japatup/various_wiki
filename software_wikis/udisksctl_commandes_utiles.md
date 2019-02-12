## udisksctl : comment monter des périphériques usb sans devoir passer root
### tags : mount, pmount, udisks, udisksctl, udev, usb

#### udisks, ça permet de faire quoi ?
- monter/démonter facilement des disques amovibles : 
	- pas besoin d'être root
	- pas besoin de préciser les options de r/W et mask pour pouvoir écrire sur les fs fat et ntfs
	- pas besoin de créer le point de montage : le périphérique est automatiquement nommé et monté dans /media/userName/nomDuPeripherique

#### commandes utiles : 
- pour monter :
```
udisksctl mount -b /sdbX
```
- pour démonter :
```
udisksctl umount -b /sdbX
```
