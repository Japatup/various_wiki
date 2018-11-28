## Ajouter un fond d'écran avec feh
### tags : feh, ecran, background, fond d'ecran, image

#### commande de base
Avec feh :

```
feh --bg-fill --no-fehbg 'lien/vers/l/image'
```

__Exemple :__

```
feh --bg-fill --no-fehbg '/~/Images/wallpapers/mon_image.jpg'
```

__NB :__  
--bg-fill : pour que l'image prenne toute la taille de l'écran. Contrairement à --bg-scale, on ne tord pas l'image pour a faire rentrer, on se contente de zoomer jusqu'à ce que tout l'écran soit rempli quitte à perdre une partie de l'image (les bords) en zoomant trop.

--no_fehbg : pour que feh n'écrive pas automatiquement le script sh ~/.fehbg qui contient la commande utilisée pour invoque feh (la commande ci-dessus en l'occurence)

#### lancement au démarrage de i3

Dans le fichier de config i3 ~/.config/i3/config, on ajoute : 

```
exec feh --bg-fill --no-fehbg '/home/r2/Images/wallpapers/greatness_graffiti_v3.jpg'
```

__NB:__ ne pas mettre le no-startup-id ici, car ne cuit pas.
