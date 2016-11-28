SYSTÈME ET RÉSEAUX (LINUX)
**********************************************

Présentation virtualisation légère :
===================================
description, pourquoi l'utilisé, solutions existante

####Définition et description.

Définition: La virtualisation consiste à faire fonctionner un ou plusieurs systèmes d'exploitation / applications comme un simple logiciel, sur un ou plusieurs ordinateurs - serveurs /système d’exploitation, au lieu de ne pouvoir en installer qu'un seul par machine.

Ces ordinateurs virtuels sont appelés Environment. La virtualisation de systèmes d'exploitation est une technique consistant à faire fonctionner en même temps, sur un seul ordinateur, plusieurs systèmes d'exploitation comme s'ils fonctionnaient sur des ordinateurs distincts.

####Pourquoi utiliser la virtualisation ?

La virtualisation de systèmes d'exploitation a plusieurs intérêts :

  -  Utiliser un autre système d'exploitation sans redémarrer son ordinateur, afin d'utiliser des programmes ne fonctionnant pas nativement dans Ubuntu ;
  -  Exploiter des périphériques ne fonctionnant pas dans Ubuntu mais fonctionnant dans d'autres systèmes d'exploitation ;
  -  Tester des systèmes d'exploitation en cours de développement sans compromettre un environnement quotidien stable ;
  -  Tester des logiciels dans des environnements contrôlés, isolés et sécurisés ;
  -  Transporter ses systèmes d'exploitation d'un ordinateur à l'autre, une machine virtuelle fonctionnant sur n'importe quel ordinateur disposant d'un hyperviseur compatible.

Les particuliers et les PME/PMI seront généralement plus intéressés par la perspective de faire fonctionner deux systèmes d'exploitation différents en même temps, afin d'exécuter des logiciels qui sont compatibles avec l'un mais pas avec l'autre. Les grandes entreprises, elles, ont de plus en plus recours à la virtualisation afin de gagner de la place dans les salles de serveurs, faciliter les installations et les redémarrages après incidents, développer et sécuriser les réseaux d'entreprises.

####Les solutions existantes.

Virtualisation complète, l'émulation, la paravirtualisation et l'Environnement virtuel*
Notre sujet traite uniquement sur la dernière de celle-ci, l'Environnement virtuel.

La virtualisation d'un système est une méthode de virtualisation de serveurs dans laquelle le noyau d'un système d'exploitation permet l'existence de multiples instances d'espaces utilisateur isolées au lieu d'une seule. Ces instances, parfois appelées conteneurs, conteneurs logiciels, moteurs de virtualisation ou encore prison (dans le cas de l'utilisation de chroot) peuvent ressembler à un vrai serveur du point de vue de l'utilisateurs et de ses propiétaires.
Sur le système d'exploitation Unix-like, cette technologie peut être considérée comme une mise en œuvre avancée du mécanisme chroot standard. En plus des mécanismes d'isolement, le noyau fournit souvent des fonctionnalités de gestion des ressources pour limiter l'impact des activités d'un conteneur sur d'autres conteneurs.

*Sources :*
http://www.tuto-it.fr/virtualisation.php
http://www.systancia.com/fr/pourquoi-virtualiser
https://doc.ubuntu-fr.org/virtualisation#virtualisation_completeun_choix_privilegie_chez_le_particulier
https://en.wikipedia.org/wiki/Operating_system-level_virtualization
