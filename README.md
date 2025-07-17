# Tuto rapide pour avoir des films en VF et bonne qualité

1 - Récupérer 2 fichiers, le premier avec la meilleure qualité vidéo possible (ou au moins souhaitée), et le second avec la meilleure qualité audio/VF trouvée

2 - Installer ffmpeg qui est un utilitaire de traitement vidéo et audio (toujours utile à avoir)

```bash
ffmpeg -version
```

Si cette commande renvoie une erreur, vous devez installer ffmpeg avec la commande suivante (sous linux) :

```bash
sudo apt install ffmpeg
```

Si vous êtes sous windows, rendez-vous sur https://ffmpeg.org/download.html, télécharger la version windows puis installez-la.


3 - Fusionner les 2 fichiers en un fichier en bonne qualité et en VF

```bash
# On suppose que vous avez audio.avi et video.avi, mais ça fonctionne avec n'importe quelles extensions de fichiers

ffmpeg -i audio.avi -i video.avi -c:v copy -c:a copy -map 0:a:0 -map 1:v:0 output.avi
```

Alors, expliquons rapidement ce que fait cette commande, qui génère un fichier output.avi à partir des 2 autres.

* `-i audio.avi -i video.avi` : spécifie les fichiers à utiliser (l'ordre compte !)

* `-c:v copy` : spécifie les codecs à utiliser, ici copy indique qu'on veut les même qu'à l'origine

* `-map 0:a:0` : redirige l'audio (a) du premier fichier (0) vers le fichier de sortie (0)

* `map 1:v:0`: redirige la vidéo (v) du second fichier (1) vers le fichier de sortie (0)


4 - Pour une meilleure qualité...

Pour améliorer la qualité visuelle et audio, au détriment du poids du fichier, et SURTOUT DU TEMPS DE CALCUL, on peut réencoder le film, et appliquer 2 passes pour optimiser l'espace. J'ai utilisé la commande suivante :

```bash
ffmpeg -i audio.avi -i video.mkv -c:v libx265 -b:v 3000k -c:a pcm_s16le -b:a 192k -pass 1 -f avi /dev/null && \
ffmpeg -i audio.avi -i video.mkv -c:v libx265 -b:v 3000k -c:a pcm_s16le -pass 2 output.avi
```

Ceci dit gros avertissement, cette commande équivaut à vouloir faire un rendu de film dans un logiciel de montage, en gros si vous n'avez pas de carte graphique c'est peut-être jouable mais sur une journée, et si vous avez un vieux pc oubliez. Sur un vieux pc qui me sert de serveur, une estimation du temps de calcul est de 160h (= 7 jours). Mais si vous avez un pc capable de ça et que vous avez un home cinéma, vous pouvez tenter l'expérience !