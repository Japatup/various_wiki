## installation de miniconda sur le pi
### tags : miniconda, arm, python

#### modop d'installation
- on télécharge le .sh d'installation sur le site de continuum : `http://repo.continuum.io/miniconda/Miniconda3-latest-Linux-armv7l.sh` (on aurait également pu le faire via un wget sur le pi). **bien prendre la version arm !**.
- on dépose temporairement le Miniconda3-latest-Linux-armv7l.sh sur le pi dans ~/Documents
- on rend le script exécutable
- on l'exécute :)
- le script nous demande de valider la licence, puis de spécifier le dossier d'installation. --> on installera dans `/opt/miniconda3`
- en fin d'installation, le script nous demande si l'on souhaite ajouter /opt/miniconda3/bin au path de root : répondre oui
- puis appliquer également cet ajout au path pour les autre users : 
    ```
    cat ~/.bashrc |grep PATH >> /home/<user_who_wants_miniconda>/.bashrc 
    ```
    
#### création d'un environnement
- sous le compte user (et non root), lancer : 
    ```
    conda create --name general python=3.4 ##(apparemment, on devra se contenter de python 3.4 sur le pi avec conda...)
    ```
- l'installation se réalise dans ~/.conda/envs/general.

#### activation et désactivation d'un environnement
- avec les commandes 
    - `source activate general`
    - et `source deactivate`s
