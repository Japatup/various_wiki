## configuration ranger
### tags : ranger, gestionnaire fichiers, 

#### ajouter des commandes ou modifier les raccourcis clavier
Les commandes et raccourcis clavier sont spécifiés dans le fichier rc.conf.

1) le copier depuis sa version etc : 
    ```
    cp /etc/ranger/config/rc.conf ~/.config/ranger/
    ```
2) le modifier en y ajoutant/modifiant les commandes voulues. Dans nos modifs, chaque bloc de code customisé (ajouté/modifié) est précédé du commentaire "## modif :". Si présent dans le dossier config, ranger ne lira que cette version du fichier, et plus celle de /etc/ranger. Aussi, ne pas supprimer de commandes, car elles ne seraient plus chargées par ranger

On a ajouté : 
```
## modif :
map dU shell -p du --max-depth=1 -ha --apparent-size
map du shell -p du --max-depth=1 -ha --apparent-size | sort -h

## modif : (add en fait : )
map ll shell -p ls -FlAch --time-style=long-iso
map lm shell -p ls -FlAtch --time-style=long-iso
```