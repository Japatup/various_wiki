## installation de firefox (version mise à jour)
### tags : firefox, firefox-esr, opt, tar.bz2

#### contexte : 
on souhaite mettre à jour firefox, car la version disponible sur debian stable est gentiment obsolète.

#### modop : 
- on télécharge le dernier tar.bz2 disponible sur le site de mozilla : https://www.mozilla.org/fr/firefox/new/?utm_medium=referral&utm_source=support.mozilla.org
- on le sauve dans ~/Documents/Applis_et_updates
- on le décompresse : 
    ```
     tar xjf firefox-63.0.1.tar.bz2
    ```
- on copie le dossier firefox nouvellement créé dans /opt : 
    ```
    cp -r ./firefox /opt
    ```
- si firefox-esr était installé, on le désinstalle 
    ```
    aptitude remove firefox-esr
    ```
- on ajoute l'exécutable dans la liste des exécutables locaux en symlink : 
    ```
    ln -s /opt/firefox/firefox /usr/local/bin/firefox
    ```
- on est content