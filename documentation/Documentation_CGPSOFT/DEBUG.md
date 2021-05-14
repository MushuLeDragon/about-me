# Docker

S'il n'y a plus de visibilité sur l'ensemble des dockers voici les procédures à suivre :

1. Arret du service : `sudo systemctl stop docker`
2. Sauvegarde du repertoire docker : `cp -au /var/lib/docker /var/lib/docker.bk`
3. création du lien symbolique pour pointer sur overlay2 qui se trouve sur la partition home : `ls -s /var/lib/docker/overlay2 /home/docker/overlay2`
4. Demarrage du service : `sudo systemctl start docker `
