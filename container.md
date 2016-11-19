**Qu'est ce qu'un container**

Une container est une instance active d'une image (à définir ?) On lance un conteneur afin d'y exécuter un ou plusieurs processus et ce avec la commande run.

ex : docker run mariabd

Dans le cas précis on demande à Docker de lancer un conteneur à partir de l'image de mariadb. Ici docker va donc chercher l'image mariadb sur l'hôte ou alors la télécharger automaniquement si elle n'y est pas présente. 

On peut aussi ajouter des options à notre commance run afin d'accéder à l'intérieur du container :
docker run -ti mariadb /bin/bash

Ainsi notre conteneur est lancé et nous nous retrouvons comme connecté à l'intérieur de notre conteneur.

On peut voir la liste des containers lancé via la commance docker ps 
Où on va retrouver plusieurs informations : 
l'id de notre container
l'image de notre container 
ou encore depuis combien de temps il a été créer

L'option -a a notre commande nous permet de voir tous les conteneurs y compris ceux arrêtés.

Pour le moment on peut créer des bases de données dans notre container mariadb cependant lorsqu'on le stop et qu'on le redémarre les données qu'on avait ajouté ne sont plus présente à l'intérieur de celui-ci.

Réponse au pb -> volume.

https://www.guillaume-leduc.fr/docker-comme-solution-de-virtualisation-les-conteneurs.html