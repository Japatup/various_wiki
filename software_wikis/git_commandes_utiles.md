## commandes utiles sur git
### tags : git, git.config, 

#### config de git
- gestion des mots de passe : 
    ```
    git config --global credential.helper 'cache --timeout 7200' 
    ## fait en sorte que le mot de passe ne soit demandé qu'une seule fois toutes les 2h (7200 secondes). Les identifiants sont conservés en RAM uniquement et disparaissent après le timeout
    ```
- gestion de l'autocrlf : 
    ```
    git config --global core.autocrlf input
    ## détermine le type de caractères de fin de ligne qui git appliquera à chaque checkout.
    ## si true, git placera des crlf, ce qui peut être embettant pour linux car les shebang et makefile veulent des lf
    ## si input, git placera des lf
    ## si false, git rendra le format initial, tel que commité
    ```
- gestion de l'éditeur de texte par défaut : 
    ```
    git config --global -e ## pour accéder au fichier de config
    ```
    puis dans le fichier de config, ajouter : 
    ```
    [core]
    editor = 'code' --wait ## pour utiliser visual studio code
    ```
