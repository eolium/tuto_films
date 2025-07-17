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


4 - Pistes d'amélioration

On peut aussi tenter de réencoder la vidéo et l'audio (à voir si c'est mieux), mais la commande sera plus lente. On a alors :

```bash
ffmpeg -i audio.avi -i video.avi -c:v libx264 -c:a aac -strict experimental -map 0:a:0 -map 1:v:0 output.avi
```