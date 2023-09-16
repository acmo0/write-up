# Memez
## Énnoncé
Les memes, c'est comme le saucisson et les chips, ça se partage. Mais bon, il faut quand même bien les sécuriser et là je crois que je me suis loupé. J'ai fait une capture réseau, tu peux vérifier que mes memes ont bien le petit cadenas vert ?

Dans la capture suivante, un utilisateur a visité son site web humoristique préféré sans que celui-ci mette en place de protections contre l'interception réseau comme TLS. Pourrez-vous retrouver le flag de notre ami ?
## Solution
On a comme fichier à télécharger une capture réseau nommée `memez.pcapng`.
On ouvre la capture réseau avec Wireshark puis on exporte tout les objets http en cliquant sur :
`Fichier > Exporter Objets > HTTP > Tout Enregister`
On regarde ensuite les images exportées et on tombe sur le flag dans l'image nommée `me-and-the-boys.png` !