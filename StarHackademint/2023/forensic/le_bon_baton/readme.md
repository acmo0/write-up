# Le bon bâton
## Énnoncé
Vous vous apprétez à jouer en famille au mölkky pendant l'apéro, sauf que personne ne trouve le bâton ! Il faut dire que cette apéro manque d'organisation... Il va falloir revenir en arrière pour le trouver.
## Solution
On a à notre diposition un fichier nommé `Jardin.img`.
On va tenter de monter la partition directement en tapant :
```bash
sudo mount Jardin.img mountpt
```
On a l'arborescence suivante :
```
mountpt
├── fs
│   ├── La bouffe
│   │   ├── Saucisson_sec_de_l'Aveyron.jpg
│   │   ├── Tapenade_01.jpg
│   │   ├── Tapenade.jpg
│   │   └── Tapenade,_tomme_et_saucisson.jpg
│   ├── Les Bâtons
│   │   ├── 640px-Mölkky_bábuk_a_fűben.jpg
│   │   ├── A_game_of_Mölkky.jpg
│   │   ├── Oikea_alkuasetelma_-_panoramio.jpg
│   │   └── Player,_Mölkky_game.jpg
│   ├── Les boules
│   │   ├── 640px-Jeux_de_Pétanque_à_Valbonne.jpg
│   │   ├── 640px-Pétanque_Spielsituation_2.jpg
│   │   ├── Petanque_117654532_7d4f89011b.jpg
│   │   ├── Pétanque_-_Muang_Khua.JPG
│   │   └── Pétanque_players_in_Cannes_(France)_2003.jpg
│   └── Pour la soif
│       ├── Cuvee-de-trolls-triple.jpg
│       ├── Gambetta_(apéritif).JPG
│       ├── Pastis_002.jpg
│       ├── Pastis_(4620701625).jpg
│       └── Pastis_(5484661091).jpg
└── .nilfs

```
Rien d'intéressant pour trouver un flag, cependant il y a un fichier qui attire l'attention : `.nilfs`
En cherchant sur internet on comprend qu'il s'agit d'une partition de type [NILFS](https://en.wikipedia.org/wiki/NILFS) qui a la particularité de comporter des checkpoints (d'où l'indice *Il va falloir revenir en arrière pour le trouver* contenu dans l'énnoncé).
La partition peut être montée avec `mount` en spécifiant le checkpoint que l'on veut avec les options :
```bash
sudo mount -t nilfs2 -r -o cp=<checkpoint number> Jardin.img mountpt
```
En essayant successivement les checkpoints on arrive à monter la partition avec la commande :
```bash
sudo mount -t nilfs2 -r -o cp=2 Jardin.img mountpt
```
On a alors l'arborescence suivante :
```
mountpt
├── fs
│   ├── La bouffe
│   │   ├── part2
│   │   ├── Saucisson_sec_de_l'Aveyron.jpg
│   │   ├── Tapenade_01.jpg
│   │   ├── Tapenade.jpg
│   │   └── Tapenade,_tomme_et_saucisson.jpg
│   ├── Les Bâtons
│   │   ├── 640px-Mölkky_bábuk_a_fűben.jpg
│   │   ├── A_game_of_Mölkky.jpg
│   │   ├── Oikea_alkuasetelma_-_panoramio.jpg
│   │   ├── part1
│   │   └── Player,_Mölkky_game.jpg
│   ├── Les boules
│   │   ├── 640px-Jeux_de_Pétanque_à_Valbonne.jpg
│   │   ├── 640px-Pétanque_Spielsituation_2.jpg
│   │   ├── Petanque_117654532_7d4f89011b.jpg
│   │   ├── Pétanque_-_Muang_Khua.JPG
│   │   └── Pétanque_players_in_Cannes_(France)_2003.jpg
│   └── Pour la soif
│       ├── Cuvee-de-trolls-triple.jpg
│       ├── Gambetta_(apéritif).JPG
│       ├── part3
│       ├── Pastis_002.jpg
│       ├── Pastis_(4620701625).jpg
│       └── Pastis_(5484661091).jpg
└── .nilfs

```
Le fichier part1 semble être une image car les premiers octets sont `FF D8 FF E0 00 10 4A 46 49 46 00 01`.
On concatène les trois fichiers avec python :
```python
flag_1 = None
with open('mountpt/fs/Les Bâtons/part1','rb') as f:
	flag_1 = f.read()
flag_2 = None
with open('mountpt/fs/La bouffe/part2','rb') as f:
	flag_2 = f.read()
flag_3 = None
with open('mountpt/fs/Pour la soif/part3','rb') as f:
	flag_3 = f.read()
with open('flag.jpg','w+b') as f:
	f.write(flag_1+flag_2+flag_3)
```
Le flag est écrit sur l'image que l'on vient d'obtenir !