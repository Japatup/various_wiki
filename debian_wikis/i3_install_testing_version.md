## installation de i3 testing, soit la version 4.15-1 de 201803 (au lieu de 4.13-1 de 201611 sur stable)
### tags : debian, testing, sources.list, sid, unstable, apt, aptitude, apt-get, buster, i3, i3wm, list_hidden,

#### 1) dans sources.list.d, on ajoute le fichier /etc/apt/sources.list.d/officielDebian_testing_Buster.list comme suit : 
```
deb http://deb.debian.org/debian/ testing main
deb http://deb.debian.org/debian/ testing-updates main
deb http://security.debian.org/ testing/updates main
```

#### 2) vérifier la disponibilité des versions avec aptitude : 
```
aptitude update
aptitude show i3
```
#### 3) installer i3 testing en mode simulation : 
```
aptitude -V -s install i3

# Les paquets suivants ont des dépendances non satisfaites :
#  libc6-dev : Dépend: libc6 (= 2.24-11+deb9u3) but 2.27-8 is to be installed
#  libc-dev-bin : Dépend: libc6 (< 2.25) but 2.27-8 is to be installed
# Les actions suivantes permettront de résoudre ces dépendances :
# 
#      Mettre à jour les paquets suivants :                             
# 1)     libc-dev-bin [2.24-11+deb9u3 (now, stable) -> 2.27-8 (testing)]
# 2)     libc6-dev [2.24-11+deb9u3 (now, stable) -> 2.27-8 (testing)]       
```

#### 4) si ok, install pour de vrai :)
NB : pour assurer la montée en version de certaines librairies, l'install va potentiellement être amenée à restarter certains services (cron dans notre cas)
```
aptitude install i3

# Restarting services possibly affected by the upgrade:
# cron: restarting...done.
# Services restarted successfully.
# (Reading database ... 93092 files and directories currently installed.)
# Preparing to unpack .../libc-bin_2.27-8_amd64.deb ...
# Unpacking libc-bin (2.27-8) over (2.24-11+deb9u3) ...
# Setting up libc-bin (2.27-8) ...
# Updating /etc/nsswitch.conf to current default.
# (Reading database ... 93092 files and directories currently installed.)
# Preparing to unpack .../archives/i3_4.15-1_amd64.deb ...
# Unpacking i3 (4.15-1) over (4.13-1) ...
# Preparing to unpack .../i3-wm_4.15-1_amd64.deb ...
# Unpacking i3-wm (4.15-1) over (4.13-1) ...
# Processing triggers for mime-support (3.60) ...
# Setting up i3-wm (4.15-1) ...
# Installing new version of config file /etc/i3/config ...
# Installing new version of config file /etc/i3/config.keycodes ...
# Processing triggers for desktop-file-utils (0.23-1) ...
# Setting up libc-l10n (2.27-8) ...
# Processing triggers for man-db (2.7.6.1-2) ...
# Setting up libc-dev-bin (2.27-8) ...
# Setting up libc6-dev:amd64 (2.27-8) ...
# Setting up locales (2.27-8) ...
# Installing new version of config file /etc/locale.alias ...
# Generating locales (this might take a while)...
#   fr_FR.UTF-8... done
# Generation complete.
# Setting up i3 (4.15-1) ...
                                                    
# État actuel : 800 (-8) upgradable.

## vérification de l'upgrade de i3 :
sources.list.d i3 --version   

# i3 version 4.15 (2018-03-10) © 2009 Michael Stapelberg and contributors
```

#### 5) maj de i3status également

```
aptitude install -V -s i3status

# Les NOUVEAUX paquets suivants vont être installés :     
#   libconfuse2{a} [3.2.2+dfsg-1]  
# Les paquets suivants seront ENLEVÉS : 
#   libconfuse1{u} [3.0+dfsg-2]  
# Les paquets suivants seront mis à jour : 
#   i3status [2.11-1 -> 2.12-1]  libconfuse-common [3.0+dfsg-2 -> 3.2.2+dfsg-1]  
# 2 paquets mis à jour, 1 nouvellement installés, 1 à enlever et 798 non mis à jour.
# Il est nécessaire de télécharger 100 ko d'archives. Après dépaquetage, 10,2 ko seront utilisés.

aptitude install i3status
```
#### 6) maj en sid
Même fonctionnement que précédemment, en ajoutant le fichier /etc/apt/sources.list.d/officielDebian_sid.list : 
```
deb http://deb.debian.org/debian/ unstable main
```
PI : Pour passer en sid, les paquets suivants sont mis à jour : 
- i3 [4.15-1 -> 4.16-1]
- i3-wm [4.15-1 -> 4.16-1]  
- libglib2.0-0 [2.50.3-2 -> 2.58.1-2]  

#### 6) exclusion des paquets testing de l'index apt
En effet, on ne souhaite pas migrer l'ensemble de nos paquets sur testing. En conséquence, une fois l'opération d'installation terminée, on dissimule le dernier fichier sources.list ajouté et on remet à jour l'index : 

```
mv /etc/apt/sources.list.d/officielDebian_testing_Buster.list{,_hidden}
aptitude update
```

