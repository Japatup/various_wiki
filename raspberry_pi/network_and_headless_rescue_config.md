## récap gestion des réseaux sur le pi : 
### tags : ip, address, dhcpd, network, interfaces, 

#### configurer le pi headless sur plusieurs réseaux

##### Objectif : 
- configurer le pi pour qu'il puisse se connecter automatiquement à plusieurs réseaux wifi pré établis.
- inclure dans cette liste des réseaux reconnus par le pi un hotspot généré à l'aide d'un smartphone. Le pi étant en headless, si le réseau domestique tombe ou si l'on déménage le pi, on pourra ainsi toujours s'y connecter grâce au hotspot wifi préconfiguré. (**NB : le pi ne digère pas les réseaux wifi en bande de fréquence 5GHz, mais uniquement les 2,4GHz. Sur votre android, réglez de fait la fréquence du hotspot en conséquence**)

##### Contraintes : 
- dans le fichier /etc/dhcpd.conf, on peut paramétrer une ip fixe par interface. Or ici, on veut pouvoir gérer plusieurs réseaux potentiels sur la même interface wlan0, et définir une certaine adresse ip sur l'un, une autre sur un second, et un comportement dynamique (automatique) sur un troisième. En fonction du réseau que le pi captera sur la carte wifi, il pourra donc changer de comportement de demande d'adresse ip.

##### Solution : 
- on ne va pas utiliser le fichier /etc/dhcpd.conf, mais les fichiers `/etc/wpa_supplicant/wpa_supplicant.conf` et `/etc/network/interfaces`
- dans `/etc/wpa_supplicant/wpa_supplicant.conf`, on configure les interfaces virtuelles voulues : 
	```
	ctrl_interface=/var/run/wpa_supplicant
	update_config=1
	country=FR

	## premier réseau wifi possible. 
	## on lui donne un nom, "box_sfr", ce qui créera une interface virtuelle utilisable dans /etc/network/interfaces
	network={
			ssid="<nom_du_reseau>"
			scan_ssid=1
			psk="<mot_de_passe_du_reseau>"
			priority=0
			id_str="box_sfr"
	}

	## deuxième réseau possible : c'est le réseau de secours en hotspot wifi via android, au cas où le réseau domestique n'est pas dispo
	## on fixe une priorité élevée. Ainsi, lorsque ce réseau est activé, le pi s'y connecte en priorité. Rescue first :)
	network={
			ssid="pi_rescue_network"
			scan_ssid=1
			psk="<mot_de_passe>"
			priority=100
			id_str="rescue_network"
	}
	
	## troisième réseau 
	network={
		(...)
	}
	```
- dans `/etc/network/interfaces`, on va configurer les stratégies d'allocations ip : 
	```
	# interfaces(5) file used by ifup(8) and ifdown(8)

	# Please note that this file is written to be used with dhcpcd
	# For static IP, consult /etc/dhcpcd.conf and 'man dhcpcd.conf'

	# Include files from /etc/network/interfaces.d:
	source-directory /etc/network/interfaces.d

	## l'interface lo : on n'y touche pas
	auto lo
	iface lo inet loopback
	
	## l'interface ethernet
	# on trouve son nom grâce à la commande ip address (ou ip a)
	allow-hotplug enxb827eb91ca8f
	iface enxb827eb91ca8f inet static
	address 192.168.0.146
	gateway 192.168.0.1
	netmask 255.255.255.0
	dns-nameservers 89.2.0.1 89.2.0.2

	## l'interface wlan0 : on lui dit d'aller voir le fichier /etc/wpa_supplicant/wpa_supplicant.conf
	allow-hotplug wlan0
	iface wlan0 inet manual
	wpa-roam /etc/wpa_supplicant/wpa_supplicant.conf

	## on configure le réseau "box_sfr" définit plus haut comme une interface virtuelle
	## et on définit une ip statique : 
	iface box_sfr inet static
	address 192.168.0.144
	gateway 192.168.0.1
	netmask 255.255.255.0
	dns-nameservers 89.2.0.1 89.2.0.2

	## quand au réseau de secours, on laisse l'allocation dynamique. On pourra utiliser nmap -sP 192.168.0.0/24 pour retrouver l'ip du pi une fois connecté
	iface rescue_network inet dhcp
	```
**NB : les services networking et dhcpcd fonctionnent sont tous les deux actifs**

## à faire :
ok : - ajouter la déclaration des adresses DNS sfr_box dans network interfaces !
- mettre au propre le wiki, et la version __ignore__ où j'aurai les noms des connexions et les mots de passe
- modifier le wiki intall_raspbian_headless(...) pour intégrer dès le début la connexion en ip statique
