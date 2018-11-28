## configuration du rétro-éclairage du clavier
### tags : kbd, backlight, retroeclairage, leds, libinput, materiel, group kbd_backlight_users

## comment ça marche ? 
Dans l'idée, c'est simple : on modifie directement la valeur du fichier
`/sys/class/leds/asus::kbd_backlight/brightness`, en y mettant un 0 (rétroéclairage désactivé), un
1, un 2 ou un 3.

## la fonction qui va bien :
est stockée dans `~/Documents/01_scripts_admin/kbd_backlight.sh`, linkée -s vers `/usr/local/bin/kdb_backlight`
, et permet de modifier la valeur du dit fichier comme suit : 

```bash
kbd_backlight up
kbd_backlight down
kbd_backlight max
kbd_backlight off
kbd_backlight night
kbd_backlight 2
kbd_backlight show
```

## le problème des permissons : 
seul root peut écrire dans les fichiers du répertoire `/sys/class/leds/asus::kbd_backlight`

Par ailleurs, il n'est pas possible de définir les permissions une fois pour toutes car `/sys` est un "faux dossier",
qui se recréé à chaque démarrage en faisant un simple lien vers le kernel. En conséquence, il faut relancer à chaque démarrage le chmod for all.

## la création du service
Pour ce faire, on a : 

- la fonction `~/Documents/01_scripts_admin/kbd_backlight.sh`, qui ouvre notamment les permissions vers `/sys/class/leds/asus::kbd_backlight` en mode a+w.
- l'appel à ce script dans `/etc/rc.local` avec l'argument allowusers (par opposition à disallowusers, qui devrait ne servir à rien puisque déjà par défaut à chaque reboot) (rc.local possède donc la ligne : 

    ```
    /bin/bash -c "/usr/local/bin/kbd_backlight allowusers"
    ```

- l'activation de `/etc/rc.local` par un service en oneshot à chaque démarrage. Le service se nomme `rc-local.service` puisqu'il lance simplement le rc.local, et se trouve dans `/etc/systemd/system`. Il a la tete suivante : 
 
    ```
    [Unit]
    Description=/etc/rc.local Compatibility
    ConditionPathExists=/etc/rc.local

    [Service]
    Type=oneshot
    User=root
    ExecStart=/bin/bash -c "/etc/rc.local"

    [Install]
    WantedBy=multi-user.target
    ```

- Puis on l'a rendu enable et on l'a starté pour voir : 

    ```bash
    systemctl enable rc_local.service 
    systemctl start  rc_local.service 
    ```

Ajout de l'alias et des raccourcis clavier dans le .zshrc : 

```
alias kbd_backlight='/bin/bash /usr/local/bin/kbd_backlight'
## qu'on rendra exécutable pour tous, afin que tous les users puissent lancer le script)
```

Dans le .config/i3/config : 

```
bindsym XF86kbdBrightnessDown exec --no-startup-id kdb_backlight down
bindsym XF86kbdBrightnessUp exec --no-startup-id kdb_backlight up
```


et ça cuit, en terminal ou en raccourcis claviers :)



#### OLD : donner les droits permanents sur `/sys` (ne fonctionne pas => inutile)

On a créé un groupe kdb_backlight_users : 

```
addgroup kbd_backlight_users
```

puis on y a ajouté notre user : 
```
adduser yourUserName kbd_backlight_users
```

Ceci fait, on a autorisé l'accès en écriture aux groupe kbd_backlight_users : (on ne l'a pas fait sur tout le dossier, car il est rempli de liens symboliques de dossiers qui empêchent le chmod récursif de s'appliquer.

```
cd /sys/class/leds/asus::kbd_backlight
chgrp -R kbd_backlight_users brightness
chmod -R g+w brightness
```

**Mais à chaque redémarrage : le chmod est réinitialisé, rendant la manoeuvre caduque**