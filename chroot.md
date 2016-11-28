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

####Cadre d’usage :

Beaucoup d’utilisation différentes possibles :
Ex : permet de lancer des processus critiques dans un dossier isolé afin de rendre moins facile (pas impossible) la compromission du reste du système de fichiers dans le cas de faille d’une faille de sécurité. On appelle ça « la mise en prison du logiciel (jail) ».
Ex : permet aussi, de créer plusieurs environnements qui obéissent à des règles différentes du reste du système. Par exemple, faire tourner un linux 32bits au sein d’un linux 64bits. La seule limitation est que le kernel (permet la communication entre eux des différents logiciels, matériels et composants) soit compatible avec les deux environnements.

Même si ces exemples semble similaire à de la virtualisation, il ne faut pas confondre les deux principes : le changement de racine n’émule (chercher à imiter) rien. Ce n’est que l’exploitation d’une priorité des processus unix.
Chaque processus chrooté accède donc au même matériel que les processus "normaux". De même 
ils tournent au sein du même kernel et partagent le même espace mémoire. Plus flagrant, les processus lancés dans le cadre d'un changement de racine sont parfaitement visible si l'on exécute une commande ps à partir d'un shell ‘‘normal’’ (shell : programme qui gère les invite de commande).
La force du changement de racine est donc d’être un principe limité mais simple, et qui ne souffre d'aucun problème de performance accompagnant généralement la virtualisation.

####A voir aussi :

SQUASHFS, commande chroot, 

*Lexique :*
**Systèmes de fichiers *nix :**
Systèmes de fichiers : permet d’ordonner les fichier que l’on créer dans le disque dur de l’ordinateur afin de pouvoir les retrouver.
Sur Microsoft Windows, impossible de modifier les propriétés d'un fichier (renommer, déplacer, supprimer…) lorsqu’il est ouvert par un programme ; cette restriction n'existe pas sur les systèmes de fichiers de type Unix (ext2, ext3, ReiserFS…). La raison est que sur les systèmes de fichiers *nix, les fichiers sont indexés selon un numéro, appelé inode ou i-node, et que chaque inode possède de nombreux attributs associés à lui, tels les droits d'accès, l'horodatage, la taille du fichier etc. Lors du renommage ou supression du fichier encore ouvert dans l’éditeur de texte, il garde des liens avec ce fichiers et c’est là la différence des systèmes de fichier *nix.

**Kernel :**
Un noyau de système d'exploitation, ou simplement noyau, ou kernel (anglais), est une des parties fondamentales de certains systèmes d'exploitation. Il gère les ressources de l'ordinateur et permet aux différents composants — matériels et logiciels — de communiquer entre eux.

**FTP:**
Cela signifie File Transfer Protocol (Protocole de transfert de fichier). Il s'agit d'un moyen codifié d'échanger des fichiers entre plusieurs ordinateurs.

*Sources :*
https://fr.wikipedia.org/wiki/Chroot
http://artisan.karma-lab.net/tag/chroot
https://doc.ubuntu-fr.org/systeme_de_fichiers
https://fr.wikipedia.org/wiki/Noyau_de_syst%C3%A8me_d'exploitation

