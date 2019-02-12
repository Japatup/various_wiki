## installation et notes sur de Azkaban (linkedin)
### tags : ordonnanceur, scheduler, flow, workflow, DAG, azkaban, linkedin

#### installation en mode solo server
- installation de gradle et de java (version 8 mini)
    ```
    # aptitude install gradle ## installe la version 3.2 de gradle, soit trop vieille pour compiler azkaban.
    ## on va plutôt installer gradle from tarball
    curl -o ~/Documents/Applis_et_updates/gradle/gradle-5.2.1-bin.zip https://services.gradle.org/distributions/gradle-5.2.1-bin.zip
    unzip -d /opt gradle-5.2.1-bin.zip ## créera le dossier /opt/gradle-5.2.1
    
    # aptitude install default-jdk ## le build fonctionne avec la version oracle de java, et non avec le default-jdk des repos
    ## on installe donc java en tarball également.
    ## téléchargement depuis https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html, et on choisit dk-8u201-linux-x64.tar.gz (il nous faut une jdk 8, la compilation ne fonctionnera pas en jdk11)
    tar -xzf jdk-8u201-linux-x64.tar.gz -C /opt ## créera le dossier jdk1.8.0_201
    mv /opt/jdk{1.8.0_201,} ## renommage en "jdk"
    ```
- téléchargement des sources de azkaban
    ```
    mkdir -p /opt/azkaban
    cd /opt/azkaban
    git clone https://github.com/azkaban/azkaban.git
    mv azkaban{,_sources} 
    ```
- build avec gradle
    ```
    export JAVA_HOME="/opt/jdk"
    export PATH=$PATH:/opt/gradle-5.2.1/bin
    export PATH=$PATH:/opt/jdk/bin
    cd /opt/azkaban/azkaban_sources
    ./gradlew build installDist ## lance le téléchargement de tout plein de choses dont gradle a besoin pour la compilation. Ces fichiers seront stockés par défaut dans /root. Il est possible de les supprimer après le build (d'autant qu'elle représentent environ 1Go...)
    ```
- on lance le serveur azkaban: 
    ```
    cd /opt/azkaban/azkaban_sources/azkaban-solo-server/build/install/azkaban-solo-server
    bin/start-solo.sh
    # bin/shutdown-solo.sh ## si besoin de stopper le serveur
    ```
- les logins par défaut sont disponibles dans le fichier `/opt/azkaban/azkaban_sources/azkaban-solo-server/build/install/azkaban-solo-server/conf/azkaban-users.xml`. A date, `login:azkaban` et `mdp:azkaban`.

## notes sur azkaban : 
trucs biens et clef en main
    - via sla, apparamment possible de définir des conditions de timeout, à la tâche ou au DAG. Se configure sur la webui
    - la gestion des droits de modif, read et running des dags est de base bien fichue. A voir avec une configuration ldap, mais ça claque bien quand même.
    - le graphe de suivi des temps d'exécution a l'air de bien cuire.
    - et les temps d'exécution sont infimes. Peut-être parce qu'on est en local du reste.
    - avoir le bon fuseau horaire, c'est vachement bien aussi !

trucs à creuser : 
    - beaucoup de réglages se passent dans l'ui, à commencer par le scheduling (EDIT: il est en fait possible de le définir dans le fichier yaml), mais aussi les failure_actions, les concurrency_options. Voir comment les définir dans le .yml, notamment pour pouvoir versionner les configs
    - niveau scheduling : pas possible de régler à la fois un jour du mois et un jour de la semaine via l'écriture qwartz. Pas vraiment gênant du reste, car ça ne risque pas d'arriver souvent qu'on veuille uniquement les lundi 3 par ex.
    - une fois le working dir configuré via la webui, azkaban balance toutes ses logs dedans. Il faudrait les mettre ailleurs.
    - bon dieu c'est chaud de passer des paramètres au job ailleurs que dans l'ui ! Ou alors j'ai pas bien compris où les placer ? Assez peu de doc.
    - pas de dépendances des tâches à leur bonne exécution passée (ie dans les run précédents) semble-t-il... ou alors fonctionnalité bien bien cachée, et puisque peu de doc...
    - au sein d'un même workflow, comment envoyer des tâches à des machines workers différentes ?