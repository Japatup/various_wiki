## configuration de urxvt
### tags : urxvt, qt5, terminal,

#### installation du urxvtConfig
Il s'agit d'un outil graphique pour aider à la configuration de URxvt. L'outil va simplement écrire le fichier .Xdefaults ou .Xresources pour nous.

Modop détaillé sur la page du projet : https://github.com/daedreth/URXVTConfig

Etapes : 
- Builder urxvtConfig : 
    - Installer les dépendances : 
        - qt5 : ```aptitude install qt5-default```
        - imagemagick ```aptitude install imagemagick```
        - fontconfig (déjà installé pour ma part)
    - télécharger le code source : 
        ```
        cd ~/Documents/Applis_et_updates
        git clone https://github.com/daedreth/URXVTConfig.git
        ```        
    - builder le programme et l'installer : 
        ```    
        cp -r ~/Documents/Applis_et_updates/URXVTConfig /opt
        cd /opt/URXVTConfig/source
        qmake -qt=qt5 source/URXVTConfig.pro
        make
        sudo make install
        ```
- lancer et utiliser uxrvtconfig, avec la commande urxvtconfig :)
- (il est ensuite tout à fait possible de désinstaller qt5-default, maintenant que le package est compilé)

