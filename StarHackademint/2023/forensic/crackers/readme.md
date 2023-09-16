# Crackers
## Énnoncé
C'était super hier soir, hein ? Je voulais te transmettre un fichier, mais je l'ai mis dans un zip et j'ai bien peur que le mot de passe soit perdu dans ma migraine. C'est sûrement simplement ce qui me passait par la tête sur le moment, je suis sûr que tu sauras te débrouiller.
Accedez au contenu chiffré du fichier.
## Solution
On télécharge le fichier `cracking.zip`. Quand on essaie de le décompresser il faut fournir un mot de passe.
On récupère les informations du fichier zip por ensuite le cracker avec john the ripper :
```bash
zip2john cracking.zip > cracking.txt
john cracking.txt --wordlist=/usr/share/eaphammer/wordlists/rockyou.txt
```
Output
```
Using default input encoding: UTF-8
Loaded 1 password hash (PKZIP [32/64])
Will run 8 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
icecream&pizza   (cracking.zip)
```
On décompresse le fichier .zip avec le mot de passe `icecream&pizza` et on obtient le flag en lisant le fichier `flag.txt`