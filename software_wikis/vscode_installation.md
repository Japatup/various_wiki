## installation et configuration de vs_code
### tags : code, editeur, vscode, microsoft, .deb

## installer vs-code : 
	- on récupère le .deb sur le site de vs-code (https://code.visualstudio.com/docs/?dv=linux64_deb)
	- on le stocke dans ~/Documents/Appli_et_updates/vscode
	- en root, on l'installe :
```
dpkg -i <nom_du_paquet>
```
	- puis on installe les dépendances (regarder les messages de warnings après l'install pour savoir quoi installer. En l'occurence, il me fallait :
		- libnss3
		- libgconf-2-4
```
aptitude install libness3
```
et aptitude m'a directement proposé d'en profiter pour installer ce qui pouvait réparer code, à savoir les autres paquets.





