## zsh installation (et ohmyzsh également)
tags : zsh, ohmyzsh, bash, zsh-newuser-install, opt


## Comment installer ohmyzsh ?

Il suffit d'installer au préalable zsh et git, via APT
Puis, on exécute la commande suivante : 

``` 
sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```

qui va télécharger et exécuter le script d'installation et de configuration de ohmyzsh

Pour info, le script a été conservé dans /opt/ohmyzsh par la suite

## configuration de zsh

# choix du shell par défaut
se fait avec la commande `chsh` pour change shell.
On remplira ensuite le chemin suivant : \usr\bin\zsh


## gestion des corrections : 
pour ne pas activer les corrections (qui nous amènent parfois 
à remplacer des commandes connues par des commandes corrigées inconnues !)
dans le .zshrc, laisser en commentaire la ligne

```
#ENABLE_CORRECTION="true"
```
