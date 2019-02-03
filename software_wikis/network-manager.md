## network-manager : config et tips
### tags : static ip, fixed ip, nmtui, nmcli, network-manager

#### comment paramétrer une ip fixe avec network manager ?
1) En passant par nmtui : 
    - choper les bonnes infos : 
        - l'adresse de la gateway (qu'on peut trouver avec un `netstat -nr`)
        - l'adresse des serveur dns par défaut : voir dans `/etc/resolv.conf`
        - le mask de sous réseau à utiliser : à trouver dans la commande `ip address` (généralement /24, soit 255.255.255.0)
    - dans nmtui : 
        - modifier une connexion
        - choisir la connexion à modifier
        - config iv4 : **Manuel**
        - afficher la config puis modifier comme suit : 
            - Adresses : <static address désirée>/<mask de sous domaine>. Concrètement : 192.168.0.123/24 si l'on souhaite répondre à l'adresse 192.168.0.123 sur le sous réseau 255.255.255.0
            - passerelle : <ip de la gateway>
            - Serveurs DNS : <les ip des serveurs DNS par défaut>
        - cocher :
            - Requiert l'adressage IPv4 pour cette connexion
            - Connecter automatiquement
            - Dispo à tous les users
        - valider et quitter nmtui
    - redémarrer le service NetworkManager : 
        ```
        systemctl restart NetworkManager.service
        ```

