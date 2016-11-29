SYSTÈME ET RÉSEAUX (LINUX)
**********************************************

Date de présentation : 02/12/16

Projet sur virtualisation légère:

####Définition :

Le chroot : (change root) est une commande des systèmes d'exploitation UNIX permettant de changer le répertoire racine d'un processus de la machine hôte.

Le changement de racine est un aspect des Unix offrant une alternative très intéressante à la virtualisation. Éminemment plus léger, mais surtout plus simple à mettre en oeuvre, qu'un VirtualBox, KVM, ou même docker, le petit utilitaire chroot peut vous rendre bien des services pour emprisonner un accès FTP, pour créer une machine de développement avec des versions de librairies différentes de celle de votre système principal ou encore simplement pour tester les derniers joujous dans une version instable de debian.

####Qu’est se qu’un Chroot ?

####La base :

Le système de fichiers *nix est basé sur une construction autour d’une racine (le /) sur laquelle les partitions sont ensuite « montées » se qui forme l’espace de fichier accessible.
Cette racine est alors la référence pour tous les chemins absolus menant aux fichiers.
Gràce à l’utilitaire CHROOT, on peut modifier cette racine (puisqu’elle n’est en faite qu’un paramètre du processus). Pour quoi faire ? Pour faire croire à ce processus que le dossier que l’on veut, devienne la racine et donc l’origine de tous les chemins absolus.

####A quoi sert un Chroot ?

La commande chroot permet de changer le répertoire racine d'un processus. Le processus est donc isolé, au niveau de l'accessibilité du système de fichier. Il ne peut accéder à l'ensemble du système de fichier.
Cet outil fait donc partie de la famille des isolateurs.

####Cadre d’usage :

Cette opération peut être utilisée dans divers cas :
- prison : empêche un utilisateur ou un programme de remonter dans l'arborescence et le cantonne à une nouvelle arborescence restreinte.

Ex : permet de lancer des processus critiques dans un dossier isolé afin de rendre moins facile (pas impossible) la compromission du reste du système de fichiers dans le cas de faille d’une faille de sécurité. On appelle ça « la mise en prison du logiciel (jail) ».

- changement d'environnement : permet de basculer vers un autre système linux (autre architecture, autre distribution, autre version). Nous détaillerons ici cette technique.

Ex : permet aussi, de créer plusieurs environnements qui obéissent à des règles différentes du reste du système. Par exemple, faire tourner un linux 32bits au sein d’un linux 64bits. La seule limitation est que le kernel (permet la communication entre eux des différents logiciels, matériels et composants) soit compatible avec les deux environnements.

Même si ces exemples semble similaire à de la virtualisation, il ne faut pas confondre les deux principes : le changement de racine n’émule (chercher à imiter) rien. Ce n’est que l’exploitation d’une priorité des processus unix.
Chaque processus chrooté accède donc au même matériel que les processus "normaux". De même 
ils tournent au sein du même kernel et partagent le même espace mémoire. Plus flagrant, les processus lancés dans le cadre d'un changement de racine sont parfaitement visible si l'on exécute une commande ps à partir d'un shell ‘‘normal’’ (shell : programme qui gère les invite de commande).
La force du changement de racine est donc d’être un principe limité mais simple, et qui ne souffre d'aucun problème de performance accompagnant généralement la virtualisation.

####Exemple utilisation chroot pour changer de système :
A réaliser en super utilisateur pour ne pas à écrire au début de chaque ligne de commande le 'sudo'.

#Installation

Chroot est installé par défaut dans les distributions courantes. Si jamais ce n'était pas le cas, vous pouvez le trouver normalement dans le paquet coreutils (au moins pour les distributions Debian et Mandrake).

	find . / | grep chroot

	whereis chroot

#Préparation du compte

Si vous ne l'avez pas déjà créée, il faut établir les paramêtres du dit compte.
Voici un exemple :

	`$ useradd -u 1001 -g 1001 -d /home/chroot/toto -s /bin/chroot -c exemple toto`

Notre utilisateur toto se voit attribuer les numéros 1001 d'utilisateur et 1001 de groupe et sera logé dans le répertoire /home/chroot/toto. Le shell qui lui permettra de se connecter est un petit script qui autorise l'emprisonnement de l'utilisateur à son arrivée.
De manière simple, ce script peut ainsi être écrit :

	`#!/bin/bash`
	`exec -c /usr/sbin/chroot /home/chroot/$USER /bin/bash`

Si nous utilisons la ligne de commande qui constitue ce script, dans une phase de test, une erreur va nous être retournée :

	`$/usr/sbin/chroot /home/chroot/toto /bin/bash`
	`/usr/sbin/chroot: /bin/bash: No such file or directory`

Que se passe t'il donc ? La réponse est simple. L'invocation du shell, ici /bin/bash, se fait après le déplacement de la racine. Il cherche donc au pied de cette nouvelle racine un répertoire bin contenant l'utilitaire bash. Cependant, puisque nous n'avons jusqu'à lors inséré aucun outil, le système refuse la commande.

Que faire dans ce cas ? La réponse est assez simple, nous allons construire l'environnement nécessaire à l'utilisation de la prison. Cela signifie que chaque commande que vous désirerez utiliser dans cet espace restreint doit y être copié, dans le répertoire aproprié.

Avant de chercher à automatiser la tâche, commençons par le bash de tout à l'heure :

	`$ cd /home/chroot/toto`
	`$ mkdir bin`
	`$ cp /bin/bash bin/bash`

L'utilitaire est à présent copié.
Il faut également fournir les librairies nécessaires à son utilisation. Nous utiliserons l'outil ldd pour déterminer les fichiers nécessaires.

	`$ ldd /bin/bash`
	`libncurses.so.5 => /lib/libncurses.so.5 (0x4001e000)`
	`libdl.so.2 => /lib/libdl.so.2 (0x4005a000)`
	`libc.so.6 => /lib/libc.so.6 (0x4005d000)`
	`/lib/ld-linux.so.2 => /lib/ld-linux.so.2 (0x40000000)`

Comme dit, copions les librairies indispensable au fonctionnement :	

	`$ mkdir lib`
	`$ cp /lib/libncurses.so.5 lib`
	`$ cp /lib/libdl.so.2 lib`
	`$ cp /lib/ld-linux.so.2 lib`

	Voilà, il ne reste plus qu'à tester en s'identifiant sous l'utilisateur toto :

	`toto@127.0.0.1's password:`
	`****************************************************************`
	`* Bienvenue dans l'environnement restreint qui vous est imparti *`
	`****************************************************************`
	`bash-2.05b$ pwd`
	`/`
	`bash-2.05b$`

Notre premier travail, créer un espace restreint pour un utilisateur, est achevé.
La tâche ne s'arrête pas là. Tout comme nous avons copié le shell bash, il faut de même insérer tous les utilitaires nécessaires ou vitaux tels que ls, chmod, rm, etc...
Pour ne pas effectuer une tâche répétitive, il est plus intelligent de créer un script qui travaillera pour nous et qui aura le mérite d'être réutilisable :

	`#!/bin/bash`

	# On vérifie que le nom de l'utilisateur souhaité est bien passé en paramêtre

	`if [ "$#" != 1 ];`
	`then`
	`echo "Usage : $0 <login>"`
	`exit 255;`
	`fi`

	`# Nom d'utilisateur`
	`LOGIN=$1`
	`# Groupe attribué à l'utilisateur`
	`GROUP=chroot`
	`# Répertoire par défaut des shell chrootés`
	`REP=/home/chroot`

*Lexique :*
**Systèmes de fichiers *nix :**
Systèmes de fichiers : permet d’ordonner les fichier que l’on créer dans le disque dur de l’ordinateur afin de pouvoir les retrouver.
Sur Microsoft Windows, impossible de modifier les propriétés d'un fichier (renommer, déplacer, supprimer…) lorsqu’il est ouvert par un programme ; cette restriction n'existe pas sur les systèmes de fichiers de type Unix (ext2, ext3, ReiserFS…). La raison est que sur les systèmes de fichiers *nix, les fichiers sont indexés selon un numéro, appelé inode ou i-node, et que chaque inode possède de nombreux attributs associés à lui, tels les droits d'accès, l'horodatage, la taille du fichier etc. Lors du renommage ou supression du fichier encore ouvert dans l’éditeur de texte, il garde des liens avec ce fichiers et c’est là la différence des systèmes de fichier *nix.

**Kernel :**
Un noyau de système d'exploitation, ou simplement noyau, ou kernel (anglais), est une des parties fondamentales de certains systèmes d'exploitation. Il gère les ressources de l'ordinateur et permet aux différents composants — matériels et logiciels — de communiquer entre eux.

**FTP:**
Cela signifie File Transfer Protocol (Protocole de transfert de fichier). Il s'agit d'un moyen codifié d'échanger des fichiers entre plusieurs ordinateurs.

*Sources :*
<https://fr.wikipedia.org/wiki/Chroot>
<http://artisan.karma-lab.net/tag/chroot>
<https://doc.ubuntu-fr.org/systeme_de_fichiers>
<http://doc.fedora-fr.org/wiki/Virtualisation#Les_techniques_de_la_virtualisation>
<https://fr.wikipedia.org/wiki/Noyau_de_syst%C3%A8me_d'exploitation>
