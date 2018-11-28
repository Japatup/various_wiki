## installer windows depuis une clef usb rendue bootable
### tags : bootable, usb, flashdisk, windows, bios, os


#### modop :
1) télécharger l'image iso à installer :
    - via l'outil MediaCreationTool de windows : voir https://www.microsoft.com/fr-fr/software-download/windows10
    - ou sur la page suivante sinon : https://www.microsoft.com/fr-fr/software-download/windows10ISO

    L'outil doit normalement servir à télécharger le fichier iso de la distro windows spécifiée, puis rendre une clef usb bootable à l'aide de cette image.
    
    Cependant, l'outil nous renvoyé le code erreur 0x80004005 - 0xA001A, et n'a as terminé la procédure.
On l'a de fait uniquement utilisé pour télécharger le fichier iso.

2) création d'une usb bootable :
    - n'a pas fonctionné avec dd : le pc bootait bien sur la clef, mais demande de drivers usb spécifiques, signe d'une
écriture non réussie du fichier iso.
    - a fonctionné avec rufus 3.3 : voir https://rufus.ie/fr_FR.html. Il s'agit d'un exécutable windows, et nous n'avons pas trouvé d'alternative linux. Pour installer windows 10, il faut donc windows (quitte à monter une vm si besoin)
    
3) Autoriser l'installation depuis le bios    
    - Rufus ne peut pas créer de signature windows officielle comme l'aurait fait MediaTools.
    Pour que le bios accepte de booter sur la clef une fois flashée, il faut au préalable aller dans le bios, configuration avancée (F7 sur les Asus Vivobook),Security, Disable Boot Security.
    
    - On change ensuite l'ordre d'amoréage pour placer la clef en prio et go !
