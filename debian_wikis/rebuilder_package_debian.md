## Build tutorial : ou plutôt comment rebuild un package après l'avoir légèrement modifié
##source : https://wiki.debian.org/BuildingTutorial

## install préalable : on aura besoin de ça : 
```
aptitude install build-essential fakeroot devscripts
```

## télécharger les sources debian
(les sources git seront les mêmes pour la partie code, mais pas pour la partie structure. Mieux vaut donc retélécharger les sources depuis les dépôts debian)

Etapes :
- être certain d'avoir des deb-src dans les source.list
- apt-get update
- mkdir dossierOuStockerLaSourceTemp
- cd dossierOuStockerLaSourceTemp
- apt-get source <paquetabuilder>
- cd <paquetabuilder_version> ## <paquetabuilder_version> est le dossier qui a été téléchargé en plus des 3 autres fichiers. Dans le sous-dossier debian, on trouvera notamment l'executable `rules` qui lance le build
- aptitude build-dep <paquetabuilder> ## on télécharge également les paquets nécessaires au build. On pourra les virer après don't worry
- debuild -b -uc -us ## optionnel : c'est pour tester que le package se compile bien.
- sudo dpkg -i ../fdupes_1.50-PR2-3_<your arch>.deb ## optionnel : pour installer ce qu'on vient de builder
- désormais, modifier le code que l'on veut modifier, et relancer le debuild et le dpkg -i pour avoir le paquet corrigé comme on l'entendait
- debuild -b -uc -us ## optionnel : permet de créer également le package source, soit dans le même format que celui qu'on a récupéré avec le apt-get source du début avec les trois fichiers + le dossier
- et notre version du paquet a désormais supplanté la précédente.


Pour désinstaller les derniers paquets installés par aptitude : (en l'occurence, c'était ceux le build-dep) : 
On va voir dans la log aptitude `/var/log/aptitude`, on extrait la partie que l'on veut virer (ici avec le tail -85), on grep uniquement les install (surtout pas les hold qui étaient déjà là avant !), et on lance un remove dessus :

for i in $cat(/var/log/aptitude |tail -85 |grep INSTALL |cut -d] -f2 |cut -d: -f1); do aptitude -P remove $i ; done
