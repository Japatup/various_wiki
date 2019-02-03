## récap gestion des réseaux sur le pi : 
### tags : ip, address, dhcpd, network, interfaces, 

#### configurer le pi headless sur plusieurs réseaux

##### Objectifs : 
- configurer le pi pour qu'il puisse se connecter automatiquement à plusieurs réseaux wifi pré établis.
- inclure dans cette liste des réseaux reconnus par le pi un hotspot généré à l'aide d'un smartphone. Le pi étant en headless, si le réseau domestique tombe ou si l'on déménage le pi, on pourra ainsi toujours s'y connecter grâce au hotspot wifi préconfiguré. (**NB : le pi ne digère pas les réseaux wifi en bande de fréquence 5GHz, mais uniquement les 2,4GHz. Sur votre android, réglez de fait la fréquence du hotspot en conséquence**)
- paramétrér une adresse ip statique différente sur chaque réseau ssid ( + réseau filaire si besoin)

##### Solution : 
- quels fichiers utiliser : 
	- `/etc/wpa_supplicant/wpa_supplicant.conf` : pour pouvoir renseigner les noms et clefs des réseaux ssid utilisés (wifi)
	- `/etc/dhcpcd.conf` : uniquement si l'on souhaite figer définir une ip statique sur certain réseaux. On aura besoin que le service dhcpcd.service soit activé (et enabled).
	- `/etc/network/interfaces` : sur rasbian stretch, on peut simplement __supprimer__ ce fichier, car tout est géré par dhcpcd.
	
- comment configurer ?
	- `/etc/wpa_supplicant/wpa_supplicant.conf` :
		```
		ctrl_interface=/var/run/wpa_supplicant
		update_config=1
		country=FR
		
		## nom et clef du premier réseau wifi
		network={
			ssid="mon_reseau_de_chez_moi"
			scan_ssid=1
			psk="le_mot_de_passe_de_ma_box"
			priority=0
			id_str="box_maison" ## ne sert que si l'on utilise /etc/network/interfaces pour gérer les interfaces virtuelles. Ici, on pourrait donc supprimer cette ligne
		}

		## deuxième réseau.
		## Ici, on paramètre un réseau de secours (typiquement un hotspot wifi créé par un smartphone) pour pouvoir se connecter au pi headless si le réseau domestique tombe ou si l'on déménage le pi.
		## La priorité est de fait plus élevée, pour que le pi se connecte à ce réseau dès qu'on le créé.
		network={
			ssid="mon_reseau_de_secours"
			scan_ssid=1
			psk="le_mot_de_passe"
			priority=100
			id_str="rescue_network" ## ne sert que si l'on utilise /etc/network/interfaces pour gérer les interfaces virtuelles. Ici, on pourrait donc supprimer cette ligne 
		}
		```
	- `/etc/dhcpcd.conf` pour déterminer les adresses ip fixes (sans précision sur un réseau, l'adresse ip pour ce réseau sera alors allouée dynamiquement) : 
		```
		## NB : pour trouver les adresses des DNS de la gateway, aller voir le fichier /etc/resolv.conf. Ou 8.8.8.8 pour utiliser le DNS google.
		
		## ip fixe sur le réseau filaire :
		interface enxb827eb91ca8f
		static ip_address=192.168.0.123/24
		static routers=192.168.0.1
		static domain_name_servers=8.8.8.8

		## ip fixe sur le wifi / réseau mon_reseau_de_chez_moi
		interface wlan0
		ssid mon_reseau_de_chez_moi
		static ip_address=192.168.0.123/24
		static routers=192.168.0.1
		static domain_name_servers=8.8.8.8

		## ip fixe sur le wifi / réseau mon_reseau_de_secours
		## ici, on ne met rien au sujet de ce réseau : pour s'y connecter, le pi demandera alors une ip dynamique
		ssid mon_reseau_de_secours
		static ip_address=192.168.43.123/24
		static routers=192.168.43.1
		static domain_name_servers=8.8.8.8
		```