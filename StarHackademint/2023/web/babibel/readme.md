# Babibel
## Énnoncé
Nous suspectons qu'un utilisateur de ce site a des choses à cacher... Connectez-vous en tant que l'utilisateur Canari pour obtenir le flag.

https://p2t.hackademint.org/
## Solution
En allant sur le site indiqué dans l'énnoncé puis en allant dans l'onglet de connexion on tombe sur un formulaire avecnom d'utilisateur et mot de passe à remplir. Il va à priori falloir faire une injection sql pour se connecter en tant que Canari.
On essaye une injection classique :
| Champ | Valeur |
|-|-|
| Nom d'utilisateur | Canari |
| Mot de passe | ' OR 1=1;-- |

On arrive bien à se connecter, cependant on est connecté en tant que président et non Canara. Il va donc falloir tenter autre chose. Le problème est que lorsque notre requête SQL est exécutée le premier nom d'utilisateur qui ressort est Président et non Canari. Pour résoudre ce problème on injecte dans les champs les données suivantes :

| Champ | Valeur |
|-|-|
| Nom d'utilisateur | Canari';-- |
| Mot de passe | < n'importe quoi, ce ne sera pas pris en compte > |

On est maintenant connecté en tant que Canari est le flag est affiché sur la page !