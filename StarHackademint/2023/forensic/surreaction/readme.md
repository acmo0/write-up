# Surréaction
## Énnoncé
Après un apéro bien arrosé, vous remarquez que Windows Defender a mis un fichier en quarantaine. Vous vous souvenez de rien. Essyaer de comprendre ce qu'il s'est passé

Retrouvez le nom d'origine du fichier sur internet

Format du flag : Star{nom_du_fichier.exe}
## Solution
On a à notre disposition un fichier Quarantine.zip
On le décompresse et on se retouve avec les dossiers suivants :
```
Quarantine/
├── Entries
│   └── {80059732-0000-0000-6667-EF3396D235E7}
├── ResourceData
│   └── 9F
│       └── 9F598F562DDCFB69FA21A077BAD87F01A3F6258E
└── Resources
    └── 9F
        └── 9F598F562DDCFB69FA21A077BAD87F01A3F6258E

```
Après quelques recherches sur internet on trouve que les fichiers sont chiffrés avec RC4. En continuant les recherches on tombe sur de magnifiques scripts python qui permettent de lire ces fichiers chiffrés et d'extraire le fichier mis en quarantaine. J'ai utilisé [ce script](https://github.com/knez/defender-dump/blob/1dd5ad8c873b0646fcf0383e7d03b5d5ea6daf39/defender-dump.py) que j'ai légèrement modifié au niveau des chemins utilisés de base pour que cela fonctionne correctement sous Linux.
On exécute :
```bash
python3 extract_quarantine.py Quarantine --dump
```
Output :
```
Exporting virus.exe
File 'quarantine.tar' successfully created
```
```bash
tar -xf quarantine.tar
```
On a alors un exécutable nommé `virus.exe`
En utilisant le site www.virustotal.com on obtient les noms suivants pour cet exécutable :
```
virus.exe
hrtc_false_positive.exe
virus_with_meta
virus
```
Le flag est Star{hrtc_false_positive.exe}