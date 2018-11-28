## installation et configuration (et tips) de docker
### tags : docker, container, image, export, build

installation de docker (source : https://docs.docker.com/install/linux/docker-ce/debian)

#### étapes : 

1) permettre à aptitude de taper sur du https. on a besoin des packages suivants : 
```
aptitude update
aptitude install apt-transport-https ca-certificates curl gnupg2 software-properties-common
```

2) télécharger et recenser la clef GPG : 
```    
curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add -

```
3) ajouter le repo docker dans les source.list.d :    :
        - création du fichier `/etc/apt/sources.list.d/docker.list`
        - et on y entre :
```
deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable

## on peut aussi faire : 
#add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"

## où le $(lsb_release -cs renvoie simplement "stretch")
```

4) On installe docker : 
```
aptitude update
aptitude install docker-ce
```

5) Optionnel : desactiver le démarrage automatique du service docker, si on n'en a pas besoin à chaque démarrage de la machine : 
```
systemctl disable docker.service
```

    
