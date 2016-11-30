LA VIRTUALISATION LÉGÈRE
==




Tables des matières
-

###I-Présentation

####A)Présentation virtualisation légère(#yolo1)

####B)Pourquoi la virtualisation légère

###II-Chroot

###III-Docker

####A)Container

####B)Volumes

####C)Sécurité

###IV-Démonstration#

####A)Installation de docker

####B)Configuration

###Lexique#

###Sources#




I-Présentation
-

###A)Présentation de la virtualisation légère<a id="yolo1"></a>

####Définition et description.

Définition: la virtualisation consiste à faire fonctionner un ou plusieurs systèmes d'exploitation / applications comme un simple logiciel, sur un ou plusieurs ordinateurs - serveurs /système d’exploitation, au lieu de ne pouvoir en installer qu'un seul par machine.

Ces ordinateurs virtuels sont appelés Environment. La virtualisation de systèmes d'exploitation est une technique consistant à faire fonctionner en même temps, sur un seul ordinateur, plusieurs systèmes d'exploitation comme s'ils fonctionnaient sur des ordinateurs distincts.

*Pour illustrer cette explication voici un schéma*  

![Virtualisation légère](https://upload.wikimedia.org/wikipedia/commons/thumb/f/fa/Diagramme_ArchiHyperviseur.png/250px-Diagramme_ArchiHyperviseur.png)

####Pourquoi utiliser la virtualisation ?

La virtualisation de systèmes d'exploitation a plusieurs intérêts :

  -  Utiliser un autre système d'exploitation sans redémarrer son ordinateur, afin d'utiliser des programmes ne fonctionnant pas nativement dans Ubuntu ;
  -  Exploiter des périphériques ne fonctionnant pas dans Ubuntu mais fonctionnant dans d'autres systèmes d'exploitation ;
  -  Tester des systèmes d'exploitation en cours de développement sans compromettre un environnement quotidien stable ;
  -  Tester des logiciels dans des environnements contrôlés, isolés et sécurisés ;
  -  Transporter ses systèmes d'exploitation d'un ordinateur à l'autre, une machine virtuelle fonctionnant sur n'importe quel ordinateur disposant d'un hyperviseur compatible.

Les particuliers et les PME/PMI seront généralement plus intéressés par la perspective de faire fonctionner deux systèmes d'exploitation différents en même temps, afin d'exécuter des logiciels qui sont compatibles avec l'un mais pas avec l'autre. Les grandes entreprises, elles, ont de plus en plus recours à la virtualisation afin de gagner de la place dans les salles de serveurs, faciliter les installations et les redémarrages après incidents, développer et sécuriser les réseaux d'entreprises.

####Les solutions existantes.

La virtualisation d'un système est une méthode de virtualisation de serveurs dans laquelle le noyau d'un système d'exploitation permet l'existence de multiples instances d'espaces utilisateurs isolées au lieu d'une seule. Ces instances, parfois appelées conteneurs, conteneurs logiciels, moteurs de virtualisation ou encore prison (dans le cas de l'utilisation de chroot) peuvent ressembler à un vrai serveur du point de vue de l'utilisateur et de ses propiétaires.
Sur le système d'exploitation Unix-like, cette technologie peut être considérée comme une mise en œuvre avancée du mécanisme chroot standard. En plus des mécanismes d'isolement, le noyau fournit souvent des fonctionnalités de gestion des ressources pour limiter l'impact des activités d'un conteneur sur d'autres conteneurs.

###B)Pourquoi la virtualisation légère#

***Les Avantages*** 

La virtualisation légère comporte plusieurs avantages, un des premiers, et la principale différence est la virtualisation du système d'exploitation.  
La virtualisation lourde virtualise le système d'exploitation (l'OS) tandis que la virtualisation légère ne fonctionne qu'en système de conteneur, qui ne contienne qu'au maximum que les applications/librairie.  
  
*Pour illustrer ces propos voici deux images*  

![Virtualisation légère](http://blog.nicolargo.com/wp-content/uploads/2014/05/S%C3%A9lection_180.png "légère")
*virtualisation légère*  

[virtualisation lourde](http://images.google.fr/imgres?imgurl=http%3A%2F%2Fcdn-scraplogo.pearltrees.com%2F0f%2Fca%2F0fca6a2aa3952b4032924254df876f41-pearlsquare.jpg%3Fv%3D2&imgrefurl=http%3A%2F%2Fwww.pearltrees.com%2Ft%2Fvirtualisation%2Fid15465957&h=250&w=250&tbnid=xG73NcOdnc9sCM%3A&vet=1&docid=4n5MHIaKDuEnCM&itg=1&ei=vuE1WLOjGoL-ad-5o6gI&tbm=isch&client=ubuntu&iact=rc&uact=3&dur=258&page=0&start=0&ndsp=40&ved=0ahUKEwiz-s_ewr_QAhUCfxoKHd_cCIUQMwgkKAgwCA&bih=985&biw=1855 "lourde")  
*virtualisation lourde*  
  
Le système hôte est ainsi géré directement par Docker.  

####Principe et ressource  

La virtualisation traditionnelle (dite lourde) permet via un hyperviseur, de simuler une ou plusieurs machines physiques sous forme de machine virtuelle (VM). C'est VM intègre lui-même un OS sur laquelle les applications qu'elles contiennent sont exécutées.  
À l'inverse un conteneur fait directement appel à l'OS de sa machine hôte pour réaliser ses appels système et exécuter seulement les applications.  
Le rôle de l'hyperviseur est alors assuré par un moteur de conteneurisation, tel que Docker, qui s'installe par-dessus le système d'exploitation hôte.  

Comme la virtualisation légère ne virtualisant pas le système d'exploitation, ceci permet de lancer les conteneurs de manière très rapide car il n'y a pas de phase de démarrage ni d'initialisation de cette couche.  
Le temps de lancement d'un conteneur est alors égal au temps de lancement des applications qu'il contient.  
Grâce à ceci la taille d'un conteneur est relativement petite.
Un ordinateur n'a pas réellement de limite de conteneur qui peut tourner. La seule limite proviendra de l'occupation mémoire/cpu/réseau de nos applications.  

####La portabilité  

Un autre avantage de la virtualisation légère est sa portabilité, grace à sa souplesse et légèreté car il n'embarque pas d'OS, il est totalement possible de concevoir un conteneur sur son PC portable puis de le déployer sur son infrastructure de production physique ou virtuelle.  
Par exemple une machine virtuelle réalisée avec la virtualisation lourde pourra peser plusieurs Go, alors qu'un container nu représentera seulement quelque Mo.
Un serveur donné accueillera de 10 à 100 fois plus d'instances de conteneurs que d'instances d'application sur VM.  
  
  
La portabilité de ces conteneurs peut se faire grâce à des clouds, seulement il faudra que les clouds en présence soient optimisés pour les accueillir. Amazone Web Services, Microsoft Azure, google Compute, OVH ou encore DigitalOcean font partie des clouds pouvant recevoir ces conteneurs.  
Cela permet de basculer les conteneurs d'un environnement de développement ou de test à un environnement de production facilement, ce qui n'est pas le cas des VM.  

####Pour les développeurs  

Un autre avantage est pour les développeurs, du fait de la disparition de l'OS intermédiaire des VM, ils bénéficieront aussi mécaniquement d'une pile applicative plus proche de celle de l'environnement de production. Docker pourra permettre dans le même temps de concevoir une architecture de test plus agile, chaque container de test pouvant intégrer une brique de l'application.  

IBM a réalisé un comparatif de performance entre Docker et KVM((Kernel-based Virtual Machine) qui est un hyperviseur.  
Il en conclut que Docker égale ou excède les performances de cette technique de virtualisation open source - et ce dans tous les cas testés dans le cadre du comparatif. Pour Big Blue, la performance des conteneurs Docker se rapproche même de celle des serveurs machines nus. En éliminant la couche de virtualisation, consommatrice en ressources machines, Docker permettrait de réduire la consommation de RAM de 4 à 30 fois. Ce qui contribuait à optimiser l'utilisation des serveurs, et par conséquent la consommation d'énergie - qui reste l'un des principaux postes de dépense des data centers.  

II-Chroot
-

####Définition :

Le chroot : (change root) est une commande des systèmes d'exploitation UNIX permettant de changer le répertoire racine d'un processus de la machine hôte.

Le changement de racine est un aspect des Unix offrant une alternative très intéressante à la virtualisation. Éminemment plus léger, mais surtout plus simple à mettre en oeuvre, qu'un VirtualBox, KVM, ou même docker, le petit utilitaire chroot peut vous rendre bien des services pour emprisonner un accès FTP, pour créer une machine de développement avec des versions de librairies différentes de celle de votre système principal ou encore simplement pour tester les derniers logiciels dans une version instable de debian.

####Qu’est se qu’un Chroot ?

####La base :

Le système de fichiers *nix est basé sur une construction autour d’une racine (le /) sur laquelle les partitions sont ensuite « montées » se qui forme l’espace de fichier accessible.
Cette racine est alors la référence pour tous les chemins absolus menant aux fichiers.
Gràce à l’utilitaire CHROOT, on peut modifier cette racine (puisqu’elle n’est en faite qu’un paramètre du processus). Pour quoi faire ? Pour faire croire à ce processus que le dossier que l’on veut, devienne la racine et donc l’origine de tous les chemins absolus.

####À quoi sert un Chroot ?

La commande chroot permet de changer le répertoire racine d'un processus. Le processus est donc isolé, au niveau de l'accessibilité du système de fichier. Il ne peut accéder à l'ensemble du système de fichier.
Cet outil fait donc partie de la famille des isolateurs.

####Cadre d’usage :

Cette opération peut être utilisée dans divers cas :
- prison : empêche un utilisateur ou un programme de remonter dans l'arborescence et le cantonne à une nouvelle arborescence restreinte.

Ex : permet de lancer des processus critiques dans un dossier isolé afin de rendre moins facile (pas impossible) la compromission du reste du système de fichiers dans le cas de faille d’une faille de sécurité. On appelle ça « la mise en prison du logiciel (jail) ».

- changement d'environnement : permet de basculer vers un autre système linux (autre architecture, autre distribution, autre version). Nous détaillerons ici cette technique.

Ex : permet aussi, de créer plusieurs environnements qui obéissent à des règles différentes du reste du système. Par exemple, faire tourner un linux 32bits au sein d’un linux 64bits. La seule limitation est que le kernel (permet la communication entre eux des différents logiciels, matériels et composants) soit compatible avec les deux environnements.

Même si ces exemples semblent similaires à de la virtualisation, il ne faut pas confondre les deux principes : le changement de racine n’émule (chercher à imiter) rien. Ce n’est que l’exploitation d’une priorité des processus unix.
Chaque processus chrooté accède donc au même matériel que les processus "normaux". De même 
ils tournent au sein du même kernel et partagent le même espace mémoire. Plus flagrants, les processus lancés dans le cadre d'un changement de racine sont parfaitement visibles si l'on exécute une commande ps à partir d'un shell "normal" (shell : programme qui gère les invites de commande).
La force du changement de racine est donc d’être un principe limité mais simple, et qui ne souffre d'aucun problème de performance accompagnant généralement la virtualisation.

####Exemple utilisation chroot pour changer de système :
À réaliser en super utilisateur pour ne pas à écrire au début de chaque ligne de commande le 'sudo'.

####Installation

Chroot est installé par défaut dans les distributions courantes. Si jamais ce n'était pas le cas, vous pouvez le trouver normalement dans le paquet coreutils (au moins pour les distributions Debian et Mandrake).

	find . / | grep chroot

	whereis chroot

####Préparation du compte

Si vous ne l'avez pas déjà créée, il faut établir les paramêtres du dit compte.
Voici un exemple :

	$ useradd -u 1001 -g 1001 -d /home/chroot/toto -s /bin/chroot -c exemple toto

Notre utilisateur toto se voit attribuer les numéros 1001 d'utilisateur et 1001 de groupe et sera logé dans le répertoire /home/chroot/toto. Le shell qui lui permettra de se connecter est un petit script qui autorise l'emprisonnement de l'utilisateur à son arrivée.

De manière simple, ce script peut ainsi être écrit :

	#!/bin/bash
	exec -c /usr/sbin/chroot /home/chroot/$USER /bin/bash

Si nous utilisons la ligne de commande qui constitue ce script, dans une phase de test, une erreur va nous être retournée :

	$/usr/sbin/chroot /home/chroot/toto /bin/bash
	/usr/sbin/chroot: /bin/bash: No such file or directory

Que se passe-t-il donc ? La réponse est simple. L'invocation du shell, ici /bin/bash, se fait après le déplacement de la racine. Il cherche donc au pied de cette nouvelle racine un répertoire bin contenant l'utilitaire bash. Cependant, puisque nous n'avons jusqu'à lors inséré aucun outil, le système refuse la commande.

Que faire dans ce cas ? La réponse est assez simple, nous allons construire l'environnement nécessaire à l'utilisation de la prison. Cela signifie que chaque commande que vous désirerez utiliser dans cet espace restreint doit y être copié, dans le répertoire aproprié.

Avant de chercher à automatiser la tâche, commençons par le bash de tout à l'heure :

	$ cd /home/chroot/toto
	$ mkdir bin
	$ cp /bin/bash bin/bash

L'utilitaire est à présent copié.

Il faut également fournir les librairies nécessaires à son utilisation. Nous utiliserons l'outil ldd pour déterminer les fichiers nécessaires.

	$ ldd /bin/bash
	libncurses.so.5 => /lib/libncurses.so.5 (0x4001e000)
	libdl.so.2 => /lib/libdl.so.2 (0x4005a000)
	libc.so.6 => /lib/libc.so.6 (0x4005d000)
	/lib/ld-linux.so.2 => /lib/ld-linux.so.2 (0x40000000)

Comme dit, copions les librairies indispensableS au fonctionnement :

	$ mkdir lib
	$ cp /lib/libncurses.so.5 lib
	$ cp /lib/libdl.so.2 lib
	$ cp /lib/ld-linux.so.2 lib

	Voilà, il ne reste plus qu'à tester en s'identifiant sous l'utilisateur toto :
	toto@127.0.0.1's password:
	****************************************************************
	* Bienvenue dans l'environnement restreint qui vous est imparti *
	****************************************************************
	bash-2.05b$ pwd
	/
	bash-2.05b$

Notre premier travail, créer un espace restreint pour un utilisateur, est achevé.
La tâche ne s'arrête pas là. Tout comme nous avons copié le shell bash, il faut de même insérer tous les utilitaires nécessaires ou vitaux tels que ls, chmod, rm, etc...

Pour ne pas effectuer une tâche répétitive, il est plus intelligent de créer un script qui travaillera pour nous et qui aura le mérite d'être réutilisable :

	#!/bin/bash

	# On vérifie que le nom de l'utilisateur souhaité est bien passé en paramètre
	if [ "$#" != 1 ];
	then
	echo "Usage : $0 <login>"
	exit 255;
	fi

	# Nom d'utilisateur
	LOGIN=$1
	# Groupe attribué à l'utilisateur
	GROUP=chroot
	# Répertoire par défaut des shell chrootés
	REP=/home/chroot

III-Docker
-


Docker est une plateforme de virtualisation par container ou conteneur en français. Qui nous permet de créer de nouvelles machines qui vont contenir uniquement une ou plusieurs applications. Ainsi pour partager votre projet il vous suffira simplement de mettre en place le conteneur créé sur une autre machine pour y avoir accès de façon parfaitement indépendante.

Ainsi la virtualisation par conteneur que Docker propose permet à un système Linux de contenir un ou plusieurs processus dans un environnement indépendant de l'hôte et des conteneurs entres eux. À l'inverse de la virtualisation par hyperviseur de type 2 tel que VMware ou VirtualBox qui pour chaque hôte invité va virtualiser un environnement complet.

###A)Container#


Un container est une instance active d'une image. On lance un container afin d'y exécuter un ou plusieurs processus et ce avec la commande run.

	`docker run mariabd`

Dans le cas précis on demande à Docker de lancer un container à partir de l'image de mariadb. Ici docker va donc chercher l'image mariadb sur l'hôte ou alors la télécharger automatiquement si elle n'y est pas présente. 


On peut aussi ajouter des options à notre commande run afin d'accéder à l'intérieur du container :

	`docker run -ti mariadb /bin/bash`

Ainsi notre container est lancé et nous nous retrouvons comme connecté à l'intérieur de notre container.


On peut voir la liste des conteneurs lancés via la commande :

	`docker ps`

Où on va retrouver plusieurs informations : 
l'id de notre container
l'image de notre container 
ou encore depuis combien de temps il a été créer

Si on ajoute l'option -a à notre commande, elle nous permet de voir tous les conteneurs y compris ceux arrêtés.


Pour le moment on peut créer des bases de données dans notre container mariadb cependant lorsqu'on le stop et qu'on le redémarre les données qu'on avait ajoutées ne sont plus présente à l'intérieur de celui-ci.


###B)Volumes #


Les volumes sont des dossiers qui permettent le partage de données entre un ou plusieurs conteneurs ainsi que la persistance de celles-ci.


Les volumes sont initialisés à la création du container. Si l'image de base contient des données au point de montage spécifié, celles-ci sont copiées dans le volume lors de son initialisation.
Les volumes de données peuvent être partagés entre plusieurs conteneurs.
Les changements apportés aux données du volume sont pris en compte instantanément.
Les données restent stockées sur l'hôte même si le container vient à être supprimé ou relancé.


La gestion des volumes se fait avec l'option -v lors de l'exécution des commandes : docker run et docker create. 

	`docker run --name mariadb --volume /var/lib/docker/mysql:/var/lib/mysql -d mariadb`

On va créer ici le volume /var/lib/mysql dans le container où il sera accessible par défaut en lecture et écriture.
De plus via cette commande on décide où est stocké l'emplacement du volume sur l'hôte ici : /var/lib/docker/mysql

Ainsi dans notre docker mariadb on aura accès à toutes les bases de données stockées sur notre hôte dans le dossier /var/lib/docker/mysql et si on modifie ses données via sur le container elles seront modifiées sur notre hôte.

Cela nous permet donc d'arrêter/supprimer nos conteneurs mariadb tant que nous continuerons de mapper nos volumes nos données persisteront dans nos conteneurs.


On peut aussi monter des nouveaux conteneurs qui dépendent du volume d'autres conteneurs déjà existant.

Par exemple si on veut créer un autre container mariadb-bis en exécutant l'option --volumes-from mariabd on va monter les volumes de notre conteneur existant mariadb dans notre container mariadb-bis


###C)Sécurité#

####1-Créer une partition séparée pour docker

L'ensemble des données de docker sont stockées dans /var/lib/docker. C'est le répertoire par défaut que docker va utiliser pour stocker les images et les conteneurs.
Le problème est que ce répertoire est situé à la racine / et donc si Docker remplit ce répertoire, le répertoire racine le sera aussi. Ce qui rendra le système hôte inutilisable.

***Solution :*** 
Ce problème peut aussi se rencontrer lorsqu'on récupère une image mal intentionnée qui va alors remplir la mémoire de notre hôte volontairement.
Les solutions pour éviter ce problème sont donc de créer une partition physique séparée pour le répertoire /var/lib/docker dès l'installation de docker.
Ou alors de créer une partion logique avec Logical Volume Manager. 
Ces solutions vont donc définir un quota de mémoire alloué à vos conteneurs et vos volumes pour docker sans endommager la partition racine de l'hôte.


####2-Communications entre les conteneurs

Par défaut la communication entre tous les conteneurs est possible dans utiliser la fonction link. Ainsi comme dans le cas précédant une mauvaise image pourrait donc avoir accès à tout ce qui se passe sur le réseau docker de votre hôte.
C'est une faille particulièrement dangereuse car la plupart du temps, il n'y a pas de connexion sécurisé entre les conteneurs.

***Solution : ***
Interdir le comportement par défaut. Ainsi seul les conteneurs liés entre eux par la fonction link seront capable de communiquer entre eux.


####3-Ne pas utiliser privileged pour n'importe quel image

Quand un container est lancé avec le mot clé -privileged. Docker va lui donner tous les droits, y compris celui de lancer de nouveaux containers sur l'hôte.

***Solution :***
Donc si une image demande à être lancé avec l'option privileged il faut au préalable se demander pourquoi elle a besoin de ce droit et ce dont elle a besoin. Ainsi la bonne pratique est de lui accorder que les droits dont elle a besoin pour fonctionner et non pas tous comme il l'est demandé.


IV-Démonstration
-


###A)Installation de docker#

Il existe deux solutions pour installer docker. Premièrement le télecharger depuis les dépôts officiels. La seconde solution c'est d'utiliser le script d'installation fourni par Docker. Pour le télécharger il suffit de suivre les commandes suivantes: 


	'$ wget https://get.docker.com'
	'$ mv index.html script.sh'
	'$ chmod +x script.sh'
	'$ ./script.sh'


Et voilà, docker est maintenant installée sur votre machine.


###B)Configuration#

Si vous voulez vous éviter de taper *sudo* devant chaque commande Docker, ajoutez votre utilisateur au groupe Docker:


	'$ sudo addgroup [utilisateur] docker'

Il faut maintenant lancer le service Docker:


	'$ sudo service docker start'

Et vérifier s'il fonctionne correctement: 


	'$ docker run hello-world'

A la première exécution, Docker ne doit pas trouver l'image de l'application hello-word en local. Il va alors tenter de télécharger la dernière version. En cas de réussite, il exécute l'application qui affiche une simple page d'explication sur la sortie standard et s'arrête.

Lexique
-

* KVM  : « Pour Virtual Machine Kernel-based » est une solution de virtualisation complète pour Linux sur du matériel x86 compatibles « Intel VT ou AMD-V ».  
C'est un système optimisé pour la virtualisation de serveur. Pour virtualiser des systèmes de type desktop, on peut lui préférer virtualbox. KVM semble en effet plus performant en consommation de processeur mais plus lent pour l'émulation du périphérique graphique.

* Hyperviseur  : Outil de virtualisation qui permet à plusieurs systèmes d'exploitation (OS) de fonctionner simultanément sur une même machine physique.

* Systèmes de fichiers *nix :
Systèmes de fichiers : permet d’ordonner les fichier que l’on créer dans le disque dur de l’ordinateur afin de pouvoir les retrouver.
Sur Microsoft Windows, impossible de modifier les propriétés d'un fichier (renommer, déplacer, supprimer…) lorsqu’il est ouvert par un programme ; cette restriction n'existe pas sur les systèmes de fichiers de type Unix (ext2, ext3, ReiserFS…). La raison est que sur les systèmes de fichiers *nix, les fichiers sont indexés selon un numéro, appelé inode ou i-node, et que chaque inode possède de nombreux attributs associés à lui, tels les droits d'accès, l'horodatage, la taille du fichier etc. Lors du renommage ou supression du fichier encore ouvert dans l’éditeur de texte, il garde des liens avec ce fichiers et c’est là la différence des systèmes de fichier *nix.

* Kernel :
Un noyau de système d'exploitation, ou simplement noyau, ou kernel (anglais), est une des parties fondamentales de certains systèmes d'exploitation. Il gère les ressources de l'ordinateur et permet aux différents composants — matériels et logiciels — de communiquer entre eux.

* FTP:
Cela signifie File Transfer Protocol (Protocole de transfert de fichier). Il s'agit d'un moyen codifié d'échanger des fichiers entre plusieurs ordinateurs.

Sources
-

<https://www.guillaume-leduc.fr/>

<https://w3blog.fr/2016/02/23/docker-securite-10-bonnes-pratiques/>

<https://doc.ubuntu-fr.org/chroot>

<http://artisan.karma-lab.net/tag/chroot>

<http://www.tuto-it.fr/virtualisation.php>

<https://fr.wikipedia.org/wiki/Virtualisation>

<https://doc.ubuntu-fr.org/systeme_de_fichiers>

<http://doc.fedora-fr.org/wiki/Virtualisation#Les_techniques_de_la_virtualisation>

<http://www.systancia.com/fr/pourquoi-virtualiser>

<https://fr.wikipedia.org/wiki/Noyau_de_syst%C3%A8me_d'exploitation>

<https://en.wikipedia.org/wiki/Operating_system-level_virtualization>

<https://doc.ubuntu-fr.org/virtualisation#virtualisation_completeun_choix_privilegie_chez_le_particulier>

<http://blog.nicolargo.com/2014/06/virtualisation-legere-docker.html>

<http://www.journaldunet.com/solutions/cloud-computing/1146290-cloud-pourquoi-docker-peut-tout-changer/>  

<https://wooster.checkmy.ws/2014/06/virtualisation-open-source/>  

<https://www.supinfo.com/articles/single/3568-conteneurs-docker-lxc>

