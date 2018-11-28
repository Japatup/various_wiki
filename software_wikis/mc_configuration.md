## configuration de mc tags : mc, midnight commander, file manager, 
### raccourcis clavier, keybind

#### comment profiter de la fonctionnalité "laisse mon terminal dans le wd où tu étais rendu à la fermeture ?
--> plutôt que de lancer mc, on lance (grâce à un alias) : `alias mc='. 
/usr/lib/mc/mc-wrapper.sh'`


## récap des principaux raccourcis clavier

- ctrl+x p : copie dans la console le chemin du dossier où l'on se trouve 
ctrl+shift+enter : copie dans la console le lien complet du fichier surligné

- alt+, : toogle l'agencement des deux paneaux : left vs right ou top vs down 
alt+t : loop entre 4 modes d'affichage pour le panneaux en cours (dont le 
"long", bien pratique pour les longs noms de fichiers alt+i : synchronise 
l'autre à la position du panneau actif ctrl+u : intervertit les deux 
panneaux

- alt+o : montre dans l'autre panneau le contenu du dossier surligné, et passe à la ligne suivante : pratique pour bourriner sur la commande et faire le tour de levels -1
alt+shift+h : montre l'historique des positions
ctrl+x s : créé un lien symbolique (dans l'autre panneau)
ctrl+x i : affiche dans l'autre panneau les infos du fichier surligné

## récap des paramètres inportants à modifier (directement dans les menus)
dans Option/options des panneaux / navigation : cocher mouvement à la lynx. Cela permet de se 
déplacer uniquement avec les flèches, sans avoir besoin du f3 et du home/f3
