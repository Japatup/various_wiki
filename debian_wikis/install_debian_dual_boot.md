## Installer Debian en dual boot (wiki à réaliser depuis windows)
### tags : dual boot, partition, iso, hibernation, defragmentation, windows, bios, uefi, boot secure control

NB : les manips windows décrites ici ont été réalisées sur windows 10.

### 1) faire de la place pour accueillir Debian
Ici, on va libérer de la place sur `C://`, afin de créer une partition vierge de 90 Go qui accueillera debian et son formatage ext4 lors de l'instal.

Pour réduire la partition `C://`, on doit : 

- libérer 90 Go sur `C://` si ce n'est pas déjà fait !
- supprimer temporairement la fonctionnalité de veille prolongée (qui occupe un espace mémoire dédié à la fin de la partition C) : 
    - dans un cmd admin, taper : `powercfg -h off`
- supprimer temporairement l'espace swap, qui se trouve également en fin (ie à droite) de la partititon : 
    - manip à faire en tant qu'admin
    - panneau de configuration / système / à gauche cliquer sur Paramètres système avancés
    - onglet paramètres système avancés section Performances (Effet visuels...) cliquer sur "Paramètres..."
    - onglet Avancé, dans la zone Mémoire virtuelle, cliquez sur le bouton Modifier
    - sélectionner `C:` et choisir Aucun fichier d'échange -> Définir (NB : avant modif, on était en réglage automatique, soit la première case cochée tout en haut de la fenêtre)
- défragmenter le disque `C://` (fonction accessible depuis l'explorateur de docs, propriétés de `C://`, puis onglet outils) (ça prend bien 2h)
- Puis réduire le volume `C://` depuis le gestionnaire des disques Windows (aussi appelé gestionnaire des espaces de stockage). On a réduit de sorte d'avoir 90 Go (soit 92 160 Mo) pour la partition linux, et le reste (environ 105 Go) pour Windows
- réactiver la fonctionnalité de veille prolongée : `powercfg -h on` (finalement non fait, car entraine des problèmes d'écriture sur le disque ntfs partagé entre linux et windows)
- réactiver le swap : reprendre la manip vu ci-dessus et la défaire (finalement non fait, car entraine des problèmes d'écriture sur le disque ntfs partagé entre linux et windows)

### 2) désamorcer le boot secure control dans le bios, afin de pouvoir booter sur autre chose que windows par défaut

Démarche : 

- Entrer dans le bios : 
    - Eteindre le pc par la démarche habituelle (démarrer, arrêter), mais en ayant maintenu shift avant de cliquer sur arrêter et jusqu'à l'arrêt du système
    - Avant de redémarrer le pc, maintenir f2, et ne plus le lâcher jusqu'à l'arrivée sur le menu du bios
    - Démarrer le pc, en maintenant f2 donc
- Dans la rubrique Security, mettre le paramètre Boot secure control à Deactivated
- F10 pour sauvegarder et quitter le bios
