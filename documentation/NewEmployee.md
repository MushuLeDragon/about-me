# Installation

Liste des programmes et logiciels à installer sur Windows :

- Pour dev :
   - Git : [Linux](https://git-scm.com/download/linux) ou [Git for Windows](https://git-scm.com/downloads)
   - GitFlow (installée via Git for Windows) ou pour [Linux](https://github.com/nvie/gitflow/wiki/Linux)
   - Docker avec [Docker Desktop for Windows](https://docs.docker.com/docker-for-windows/install/)
   - [PHP](https://www.php.net/downloads.php)
   - [Composer](https://getcomposer.org/download/)
   - [Symfony](https://symfony.com/download)
  
# SSH link to Bitbucket

Pour Bitbucket il semble que les clés **ed25519** fonctionnent mieux sur Windows, sinon générer une clé **RSA**.

## Générer sa clé SSH

Tuto [ici](https://docs.gitlab.com/ee/ssh/README.html#generate-an-ssh-key-pair)

## Récupérer sa clé SSH

Tuto [ici](https://docs.gitlab.com/ee/ssh/README.html#see-if-you-have-an-existing-ssh-key-pair)

## Enregistrer sa clé SSH sur son profil d'hébergeur Git

Ajouter sa clé SSH à son compte Bitbucket (ou tout autre hébergeur Git) pour pouvoir se connecter au Remote. Lien pour Bitbucket [ici](https://bitbucket.org/account/settings/ssh-keys/)
