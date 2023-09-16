# Lowengeist's secret ingredient (2/2)
## Énnoncé
Vous avez trouvé la clé ? Allez, maintenant déchiffrez ce message !

PS : Vous pouvez très bien déchiffrer avant de trouver la clé !
## Solution
On se connecte de nouveau au serveur avec netcat et on recommence à essayer différents motifs à chiffrer. Certains révèlent des informations particulièrement intéressantes :
| Clair | Chiffré |
|-|-|
| ab | cs |
| aba | csz |
| abaa| cszp |
| acaa | ctap|

On en déduit plusieurs choses :
* Il est fort probable que les lettres soient encodées de la manière suivante : a=0,b=1,c=2,...,z=25
* Le caractère chiffré au rang *n* dépend du clair au rang *n* e *n-1*
Avec tous ces éléments on peut conjecturer la façon dont est calculé la lettre du message chiffré au rang *n* (en encodant les lettres comme expliqué précédement) :
* chiffre[0] = (cle[0]+clair[0])%26
* chiffre[n] = (cle[n]+clair[n]+clair[n-1])%26
Il reste donc maintenant à écrire le code pour déchiffrer le message :
```python
alphabet = "abcdefghijklmnopqrstuvwxyz"
enc= "orugguhkazpjqxgcsoasjyrirvnopctozotvbdclbcsdtzsfkkzcwliq"
key = "crypternestpaschiffrer"

dec = ""
def nb(lettre):
	return alphabet.index(lettre)

for i in range(len(enc)):
	if i==0:
		dec += alphabet[(nb(enc[i])-nb(key[i%len(key)]))%26]
	else:
		dec += alphabet[(nb(enc[i])-nb(key[i%len(key)])-nb(dec[i-1]))%26]

print(dec)
```
Output :
```
moijemetdescornichonsparcequelesolivescestvraimentpasbon
```