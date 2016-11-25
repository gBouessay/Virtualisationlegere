LA VIRTUALISATION LEGERE
==


I-Présentation
-

###B)Pourquoi la virtualisation légère#


***Les Avantages*** 


  
La virtualisation légère comporte plusieurs avantages, un des premiers, et la principale différence est la virtualisation du système d'exploitation.  
La virtualisation lourde virtualise le système d'exploitation (l'OS) tandis que la virtualisation légère ne fonctionne qu'en système de conteneur, qui ne contienne qu'au maximum que les applications/librairie.  
  
*Pour illustrer c'est propos voici deux images*  

![Virtualisation légère](http://blog.nicolargo.com/wp-content/uploads/2014/05/S%C3%A9lection_180.png "légère")
*virtualisation légère*  

[virtualisation lourde](http://images.google.fr/imgres?imgurl=http%3A%2F%2Fcdn-scraplogo.pearltrees.com%2F0f%2Fca%2F0fca6a2aa3952b4032924254df876f41-pearlsquare.jpg%3Fv%3D2&imgrefurl=http%3A%2F%2Fwww.pearltrees.com%2Ft%2Fvirtualisation%2Fid15465957&h=250&w=250&tbnid=xG73NcOdnc9sCM%3A&vet=1&docid=4n5MHIaKDuEnCM&itg=1&ei=vuE1WLOjGoL-ad-5o6gI&tbm=isch&client=ubuntu&iact=rc&uact=3&dur=258&page=0&start=0&ndsp=40&ved=0ahUKEwiz-s_ewr_QAhUCfxoKHd_cCIUQMwgkKAgwCA&bih=985&biw=1855 "lourde")  
*virtualisation lourde*  
  
Le système hôte est ainsi géré directement par Docker.  


####Principe et ressource  

La virtualisation traditionnelle (dite lourde) permet via un hyperviseur, de simuler une ou plusieurs machine physique sous forme de machine virtuelle (VM). C'est VM intègre lui-même un OS sur laquelle les applications qu'elles contiennent sont exécuté.  
À l'inverse un conteneur fait directement appel à l'OS de sa machine hôte pour réaliser ses appels système et exécuter seulement les applications.  
Le rôle de l'hyperviseur est alors assuré par un moteur de conteneurisation, tel que Docker, qui s'installe par-dessus le système d'exploitation hôte.  
  
  

Comme la virtualisation légère ne virtualisant pas le système d'exploitation, ceci permet de lancer les conteneurs de manière très rapide car il n'y a pas de phase de démarrage ni d'initialisation de cette couche.  
Le temps de lancement d'un conteneur est alors égal au temps de lancement des applications qu'il contient.  
Grâce à ceci la taille d'un conteneur est relativement petite.
Un Pc n'a pas réellement de limite de conteneur qui peut tourner. la limite proviendra de l'occupation mémoire/cpu/réseau de nos applications.  


####La portabilité  

Un autre avantage de la virtualisation légère est sa portabilité, grace à sa souplesse et légèreté car il n'embarque pas d'OS, il est totalement possible de concevoir un conteneur sur son Pc portable puis de le déployer sur son infrastructure de production physique ou virtuelle.  
Par exemple une machine virtuelle réalisée avec la virtualisation lourde pourra peser plusieurs Go, alors qu'un container nu représentera seulement quelque Mo.
Un serveur donné accueillera de 10 à 100 fois plus d'instances de conteneurs que d'instances d'application sur VM.  
  
  
La portabilité de ces containers peut se faire grâce à des clouds, seulement il faudra que les clouds en présence soient optimisés pour les accueillir. Amazone Web Services, Microsoft Azure, google Compute, OVH ou encore DigitalOcean font partie des clouds pouvant recevoir ces containers.  
Cela permet de basculer les containers d'un environnement de développement ou de test à un environnement de production facilement, ce qui n'est pas le cas des VM.  

  

####Pour les développeurs  
  
Un autre avantage est pour les développeurs, du fait de la disparition de l'OS intermédiaire des VM, ils bénéficieront aussi mécaniquement d'une pile applicative plus proche de celle de l'environnement de production. Docker pourra permettre dans le même temps de concevoir une architecture de test plus agile, chaque container de test pouvant intégrer une brique de l'application.  
  
IBM a réalisé un comparatif de performance entre Docker et KVM((Kernel-based Virtual Machine) qui est un hyperviseur.  
Il en conclut que Docker égale ou excède les performances de cette technique de virtualisation open source - et ce dans tous les cas testés dans le cadre du comparatif. Pour Big Blue, la performance des containers Docker se rapproche même de celle des serveurs machines nus. En éliminant la couche de virtualisation, consommatrice en ressources machines, Docker permettrait de réduire la consommation de RAM de 4 à 30 fois. Ce qui contribuait à optimiser l'utilisation des serveurs, et par conséquent la consommation d'énergie - qui reste l'un des principaux postes de dépense des data centers.  



II-Pourquoi la virtualisation légère
-


III-Chroot
-


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


Léxique
-

* KVM  : « Pour Virtual Machine Kernel-based » est une solution de virtualisation complète pour Linux sur du matériel x86 compatibles « Intel VT ou AMD-V ».  

C'est un système optimisé pour la virtualisation de serveur. Pour virtualiser des systèmes de type desktop, on peut lui préférer virtualbox. KVM semble en effet plus performant en consommation de processeur mais plus lent pour l'émulation du périphérique graphique.

* Hyperviseur  : Outil de virtualisation qui permet à plusieurs systèmes d'exploitation (OS) de fonctionner simultanément sur une même machine physique.


Sources
-

<https://www.guillaume-leduc.fr/>

<https://w3blog.fr/2016/02/23/docker-securite-10-bonnes-pratiques/>

<http://blog.nicolargo.com/2014/06/virtualisation-legere-docker.html>
<http://www.journaldunet.com/solutions/cloud-computing/1146290-cloud-pourquoi-docker-peut-tout-changer/>  

<https://wooster.checkmy.ws/2014/06/virtualisation-open-source/>  
<https://www.supinfo.com/articles/single/3568-conteneurs-docker-lxc>
