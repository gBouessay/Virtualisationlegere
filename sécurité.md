**Docker sécurité**

***1-créer une partition séparée pour docker***

L'ensemble des données de docker sont stockées dans /var/lib/docker. C'est le répertoire par défaut que docker va utiliser pour stocker les images et les containers.
Le problème est que ce répertoire est situé à a racine / et donc si Docker remplit ce répertoire, le répertoire racine le sera aussi. Ce qui rendra le système hôte inutilisable.

****Solution :**** 
Ce problème peut aussi se rencontrer lorsqu'on récupère une image mal intentionné qui va alors remplir la mémoire de notre hôte volontairement.
Les solutions pour éviter ce problème sont donc de créer une partition physique séparée pour le répertoire /var/lib/docker dès l'installation de docker.
Ou alors de créez une partion logique avec Logical Volume Manager. 
Ces solutions vont doncdéfinir un quota de mémoire alloué à vos containers et vos volumes pour docker sans endommager la partition racine de l'hôte.

***2-Communications entre les containers***

Par défaut la communication entre tous les containers est possible dans utiliser la fonction link. Ainsi comme dans le cas précédant une mauvaise image pourrait donc avoir accès à tout ce qui se passe sur le réseau docker de votre hôte.
C'est une faille particulièrement dangereuse car la plupart du temps, il n'y a pas de connexion sécurisé entre les containers.

****Solution : ****
Interdir le comportement par défaut. Ainsi seul les containers liés entre eux par la fonction link seront capable de communiquer entre eux.

***3-Ne pas utiliser privileged pour n'importe quel image***

Quand un container est lancé avec le mot clé -privileged. Docker va lui donner tous les droits, y compris celui de lancer de nouveaux container sur l'hôte.

Donc si une image demande à être lancé avec l'opotion privileged il faut au préalable se demander pourquoi elle a besoin de ce droit et ce dont elle a besoin. Ainsi la bonne pratique est de lui accorder que les droits dont elle a besoin pour fonctionner et non pas tous comme il l'est demandé.

source : https://w3blog.fr/2016/02/23/docker-securite-10-bonnes-pratiques/