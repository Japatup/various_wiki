## configuration du power management
### tags : veille, power management, lid, fermeture capot, 

#### désactiver la mise en veille à la fermeture du capot (du lid)

- on modifie le fichier `/etc/systemd/logind.conf` en y modifiant les lignes suivantes comme suit : 
	```
	[Login]
	HandleLidSwitch=ignore
	HandleLidSwitchDocked=ignore
	```
- on redémarre le service (ou on redémarre le système, ça marche aussi): 
	```
	systemctl restart systemd-logind.service
	```

voir également man logind.conf pour voir toutes les possibilités
