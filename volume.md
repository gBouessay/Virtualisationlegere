**Qu'est-ce qu'un volume de données Docker ?**

Les volumes sont des dossiers qui permettent le partage de données entre un ou plusieurs containers ainsi que la persistance de celles-ci.

***Caractéristique***

Les volumes sont initialisés à la création du container. Si l'image de base contient des données au point de montage spécifié, celles-ci sont copiés dans le volume lors de son initialisation.

Les volumes de données peuvent être partagés entre plusieurs containers.

Les changements apportés aux données du volume sont pris en compte instantanément.

Les données restent stockées sur l'hôte même si le container vient à être supprimé ou relancé.

***Ajouter un volume***

La gestion des volumes se fait avec l'option -v lors de l'éxécution des commandes : docker run et docker create. 

ex : docker run --name mariadb --volume /var/lib/docker/mysql:/var/lib/mysql -d mariadb

On va créer ici le volume /var/lib/mysql dans le container où il sera accessible par défaut en lecture et écriture.

De plus via cette commande on décide où est stocké l'emplacement du volume sur l'hôte ici : /var/lib/docker/mysql

Ainsi dans notre docker mariadb on aura accès à toutes les bases de données stocké sur notre hôte dans le dossier /var/lib/docker/mysql et si on modifie ses données via sur le container elles seront modifié sur notre hôte.

Celà nous permet donc d'arrêter/supprimer nos containers mariadb tant que nous continuerons de mapper nos volumes nos données persisteront dans nos containers.

***Autres cas d'utilisation*** 

On peut aussi monter des nouveaux containers qui dépendent du volume d'autres containers déjà existant.

Par exemple si on veut créer un autre container mariadb-bis en exécutant l'option --volumes-from mariabd on va monter les volumes de notre containers existant mariadb dans notre container mariadb-bis



source : https://www.guillaume-leduc.fr/docker-comme-solution-de-virtualisation-les-volumes.html