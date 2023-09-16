# Lowengeist's secret ingredient (1/2)
## Énnoncé
Bon, on a un problème.

Lowengeist ne veut pas nous donner son ingrédient secret pour son martini ! Pour nous narguer, il nous a laissé son ingrédient, mais ce petit fifrelin l'a chiffré ! Prouvez que ous êtes meilleurs que Lowengeist et déchiffrez ce message !

Le flag de la première étape est la clé de chiffrement Format : Star{cledechiffrement}
`nc cody.hackademint.org 31493`
## Solution
On se connecte au serveur avec la commade netcat : `nc cody.hackademint.org 31493`
```
Bienvenue dans loracle, qui chiffre ce que vous rentrez. Vous devez dechiffrer : orugguhkazpjqxgcsoasjyrirvnopctozotvbdclbcsdtzsfkkzcwliq
message en clair : 
```
En essayant différentes chaînes de caractères et notament avec la lettre 'a'.
On obtient, si l'on rentre `aaaaaa` comme message en clair le message codé : `crypte`. Comme on a un début de mot qui est renvoyé on va continuer avec un peu plus de 'a' :
```
message en clair : aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
crypternestpaschiffrercrypternestpa
```
On essaye le flag : Star{crypternestpaschiffrer} qui est valide !