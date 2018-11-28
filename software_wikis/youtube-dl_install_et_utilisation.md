## installation et utilisation de youtube-dl
### tags : youtube, download, opt, 
shAWDM5vd

#### 1) install
on suit le modop indiqué sur le github de l'outil : https://github.com/rg3/youtube-dl#installation

- téléchargement : 
    ```
    mkdir -p ~/Documents/Applis_et_updates/youtube-dl
    cd ~/Documents/Applis_et_updates/youtube-dl
    curl -L https://yt-dl.org/downloads/latest/youtube-dl -o $(pwd/youtube-dl)
    ```
- copie dans /opt/youtube-dl : 
    ```
    mkdir -p /opt/youtube-dl
    cp ~/Documents/Applis_et_updates/youtube-dl/youtube-dl /opt/youtube-dl
    ```
- référencement comme exécutable
    ```
    chmod a+rx /opt/youtube-dl/youtube-dl
    ln -s /opt/youtube-dl/youtube-dl /usr/local/bin/youtube-dl
    ```
- installer également ffmpeg si pas encore fait. ffmpeg gèrera notamment la partie conversion des fichiers audio si nécessaire.
#### 2) utilisation    

- exemple pour télécharger une playlist : 
    ```
    youtube-dl --extract-audio --audio-format mp3 --audio-quality 0 --playlist-end 41 https://www.youtube.com/maplaylist
    
    youtube-dl --extract-audio --audio-format mp3 --audio-quality 0 --no-playlist https://www.youtube.com/maplaylist
    ## to download only the selected video if the url corresponds to a playlist
    
    ## NB : --audio-quality 0 signifie best quality, vs 9 qui signifie worst quality
    ```
