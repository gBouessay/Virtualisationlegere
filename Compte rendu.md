LA VIRTUALISATION LEGERE
==


I-Présentation
-


II-Pourquoi la virtualisation légère
-


III-Chroot
-

Définition :
============
Le chroot : (change root) est une commande des systèmes d'exploitation UNIX permettant de changer le répertoire racine d'un processus de la machine hôte.

Le changement de racine est un aspect des Unix offrant une alternative très intéressante à la virtualisation. Éminemment plus léger, mais surtout plus simple à mettre en oeuvre, qu'un VirtualBox, KVM, ou même docker, le petit utilitaire chroot peut vous rendre bien des services pour emprisonner un accès FTP, pour créer une machine de développement avec des versions de librairies différentes de celle de votre système principal ou encore simplement pour tester les derniers joujous dans une version instable de debian.

####Qu’est se qu’un Chroot ?

La base :
=========
Le système de fichiers *nix est basé sur une construction autour d’une racine (le /) sur laquelle les partitions sont ensuite « montées » se qui forme l’espace de fichier accessible.
Cette racine est alors la référence pour tous les chemins absolus menant aux fichiers.
Gràce à l’utilitaire CHROOT, on peut modifier cette racine (puisqu’elle n’est en faite qu’un paramètre du processus). Pour quoi faire ? Pour faire croire à ce processus que le dossier que l’on veut, devienne la racine et donc l’origine de tous les chemins absolus.

####A quoi sert un Chroot ?

Cadre d’usage :
===============
Beaucoup d’utilisation différentes possibles :
Ex : permet de lancer des processus critiques dans un dossier isolé afin de rendre moins facile (pas impossible) la compromission du reste du système de fichiers dans le cas de faille d’une faille de sécurité. On appelle ça « la mise en prison du logiciel (jail) ».
Ex : permet aussi, de créer plusieurs environnements qui obéissent à des règles différentes du reste du système. Par exemple, faire tourner un linux 32bits au sein d’un linux 64bits. La seule limitation est que le kernel (permet la communication entre eux des différents logiciels, matériels et composants) soit compatible avec les deux environnements.

Même si ces exemples semble similaire à de la virtualisation, il ne faut pas confondre les deux principes : le changement de racine n’émule (chercher à imiter) rien. Ce n’est que l’exploitation d’une priorité des processus unix.
Chaque processus chrooté accède donc au même matériel que les processus "normaux". De même 
ils tournent au sein du même kernel et partagent le même espace mémoire. Plus flagrant, les processus lancés dans le cadre d'un changement de racine sont parfaitement visible si l'on exécute une commande ps à partir d'un shell ‘‘normal’’ (shell : programme qui gère les invite de commande).
La force du changement de racine est donc d’être un principe limité mais simple, et qui ne souffre d'aucun problème de performance accompagnant généralement la virtualisation.


IV-Docker
-


Docker est une plateforme de virtualisation par container ou conteneur en français. Qui nous permet de créer de nouvelles machines qui vont contenir uniquement une ou plusieurs applications. Ainsi pour partager votre projet il vous suffira simplement de mettre en place le docker créer sur une autre machine pour y avoir accès de façon parfaitement indépendante.

Ainsi la virtualisation par conteneur que Docker propose permet à un système Linux de contenir un ou plusieurs processus dans un environnement indépendant de l'hôte et des conteneurs entres eux. A l'inverse de la virtualisation par hyperviseur de type 2 tel que VMware ou VirtualBox qui pour chaque hôte invité va virtualiser un environnement complet.

###A)Container#


Un container est une instance active d'une image. On lance un container afin d'y exécuter un ou plusieurs processus et ce avec la commande run.

`docker run mariabd`

Dans le cas précis on demande à Docker de lancer un container à partir de l'image de mariadb. Ici docker va donc chercher l'image mariadb sur l'hôte ou alors la télécharger automaniquement si elle n'y est pas présente. 


On peut aussi ajouter des options à notre commande run afin d'accéder à l'intérieur du container :

`docker run -ti mariadb /bin/bash`

Ainsi notre container est lancé et nous nous retrouvons comme connecté à l'intérieur de notre container.


On peut voir la liste des containers lancé via la commande :

`docker ps`

Où on va retrouver plusieurs informations : 
l'id de notre container
l'image de notre container 
ou encore depuis combien de temps il a été créer

Si on ajoute l'option -a a notre commande, elle nous permet de voir tous les containers y compris ceux arrêtés.


Pour le moment on peut créer des bases de données dans notre container mariadb cependant lorsqu'on le stop et qu'on le redémarre les données qu'on avait ajouté ne sont plus présente à l'intérieur de celui-ci.


###B)Volumes #


Les volumes sont des dossiers qui permettent le partage de données entre un ou plusieurs containers ainsi que la persistance de celles-ci.


Les volumes sont initialisés à la création du container. Si l'image de base contient des données au point de montage spécifié, celles-ci sont copiés dans le volume lors de son initialisation.
Les volumes de données peuvent être partagés entre plusieurs containers.
Les changements apportés aux données du volume sont pris en compte instantanément.
Les données restent stockées sur l'hôte même si le container vient à être supprimé ou relancé.


La gestion des volumes se fait avec l'option -v lors de l'éxécution des commandes : docker run et docker create. 

`docker run --name mariadb --volume /var/lib/docker/mysql:/var/lib/mysql -d mariadb`

On va créer ici le volume /var/lib/mysql dans le container où il sera accessible par défaut en lecture et écriture.
De plus via cette commande on décide où est stocké l'emplacement du volume sur l'hôte ici : /var/lib/docker/mysql

Ainsi dans notre docker mariadb on aura accès à toutes les bases de données stocké sur notre hôte dans le dossier /var/lib/docker/mysql et si on modifie ses données via sur le container elles seront modifié sur notre hôte.

Celà nous permet donc d'arrêter/supprimer nos containers mariadb tant que nous continuerons de mapper nos volumes nos données persisteront dans nos containers.


On peut aussi monter des nouveaux containers qui dépendent du volume d'autres containers déjà existant.

Par exemple si on veut créer un autre container mariadb-bis en exécutant l'option --volumes-from mariabd on va monter les volumes de notre containers existant mariadb dans notre container mariadb-bis


###C)Sécurité#

####1-Créer une partition séparée pour docker

L'ensemble des données de docker sont stockées dans /var/lib/docker. C'est le répertoire par défaut que docker va utiliser pour stocker les images et les containers.
Le problème est que ce répertoire est situé à a racine / et donc si Docker remplit ce répertoire, le répertoire racine le sera aussi. Ce qui rendra le système hôte inutilisable.

***Solution :*** 
Ce problème peut aussi se rencontrer lorsqu'on récupère une image mal intentionné qui va alors remplir la mémoire de notre hôte volontairement.
Les solutions pour éviter ce problème sont donc de créer une partition physique séparée pour le répertoire /var/lib/docker dès l'installation de docker.
Ou alors de créez une partion logique avec Logical Volume Manager. 
Ces solutions vont doncdéfinir un quota de mémoire alloué à vos containers et vos volumes pour docker sans endommager la partition racine de l'hôte.


####2-Communications entre les containers

Par défaut la communication entre tous les containers est possible dans utiliser la fonction link. Ainsi comme dans le cas précédant une mauvaise image pourrait donc avoir accès à tout ce qui se passe sur le réseau docker de votre hôte.
C'est une faille particulièrement dangereuse car la plupart du temps, il n'y a pas de connexion sécurisé entre les containers.

***Solution : ***
Interdir le comportement par défaut. Ainsi seul les containers liés entre eux par la fonction link seront capable de communiquer entre eux.


####3-Ne pas utiliser privileged pour n'importe quel image

Quand un container est lancé avec le mot clé -privileged. Docker va lui donner tous les droits, y compris celui de lancer de nouveaux container sur l'hôte.

***Solution :***
Donc si une image demande à être lancé avec l'option privileged il faut au préalable se demander pourquoi elle a besoin de ce droit et ce dont elle a besoin. Ainsi la bonne pratique est de lui accorder que les droits dont elle a besoin pour fonctionner et non pas tous comme il l'est demandé.


Sources
-

<https://www.guillaume-leduc.fr/>

<https://w3blog.fr/2016/02/23/docker-securite-10-bonnes-pratiques/>
