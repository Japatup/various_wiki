## configuration des commandes sudo
### tags : sudo, sudoers, root, permissions, 

#### ajout de règle sudo
On utilise la commande **visudo**.
Elle permet d'éditer le fichier /etc/sudoers sans faire de boulette.
On peut également éditer les fichiers présents dans le répertoire /etc/sudoers.d, qui seront chargés à l'appel de sudo
Ces fichiers doivent être en chmod 440.

#### configuration
liste des modifications ajoutées au fichier sudoers : 
- force le user à rentrer son mdp à chaque commande sudo (sauf NOPASSWD précisé dans la commande sudo)
    ```
    Defaults timestamp_timeout=0
    ```

#### ajout d'une permission dédiée pour un user
On autorise un user défini à lancer une commande ou un programme spécifique sans avoir besoin de saisir son password (notamment pour automatiser certains traitements) : 
```
yourUserName ALL=(ALL) NOPASSWD:/usr/local/bin/someRootProgram
```

si l'on souhaite que ne figurent pas dans la log sudo l'appel à cette commande, ajouter au fichier :
```
Cmnd_Alias SCRIPT = /usr/local/bin/someRootProgram, /usr/local/bin/someOtherRootProgram
Defaults!SCRIPT !syslog
```
