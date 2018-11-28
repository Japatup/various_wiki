## Suivre un lien symbolique avec git, alors que c'est impossible
### tags : git, fstab, mount, bind, symlink, link, 

#### Contexte : 

On souhaite pouvoir suivre dans un repo git un fichier qui est hors du repo (typiquement un fichier de config dans ~ ou dans ~/config/...)

#### Solution :

**(pour la soluce, passer directement à la méthode 3)**

- méthode 1 (ne fonctionne pas) : on créé simplement un lien symbolique vers le fichier dans tracké par git. Mais alors Git ne verra comme contenu du fichier que l'adresse du vrai fichier (../../../.config/mon_vrai_ficier) --> échec.

- méthode 2 (ne fonctionne pas) : on créé simplement un lien symbolique vers le fichier dans tracké par git. Là, git voit le contenu du fichier. Cependant, si l'on pull le remote mis à jour depuis une autre source et que le fichier en question a été modifié, Git va briser le lien physique en mettant à jour le fichier. En effet, Git ne modifie pas les fichiers, il les détruit et les recréé. Mais ne les recréé pas comme des liens physiques --> échec.

- méthode 3 (méthode OK, mais sudo nécessaire) : on monte le répertoire du fichier de conf à suivre dans un dossier à l'intérieur du projet git : 
    ```
    mount --bind /dossier/du/fichier /dossier/du/dir/git/ou/monter
    ## autrement dit mount --bind source destination, comment pour un ln. (le dossier de montage (destination) doit déjà exister)
    ```

Et ainsi, le fichier de conf dans son emplacement initial et sa version suivie par git sont vraiment identiques et on ne relève pas de problème lors des pull.

#### implémentation à long terme

les effets de la commande mount disparaissent au redémarrage de l'ordi --> on doit monter le bind dans /etc/fstab :

```
/dossier/du/fichier /dossier/du/dir/git/ou/monter none defaults,bind 0 0
```
