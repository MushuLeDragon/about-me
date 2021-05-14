# Documentation

## Liste des utilisateurs
- Groupe : monecgp
  - lmartos
  - ltwk
  - gpla
  - eprad
  - bandrieu

## Liste des serveurs

Liste des principaux serveurs utilisés au sein de CGP Soft Corp :
- **Hôte des Linux :** (TSE 2018)
- **monEcgp :** www.monecgp.fr > cgpsoft.dyndns.org:5000 > 192.168.0.126
- **Linux Prod SSH :** cgpsoft.dyndns.org:221 > 192.168.0.126:221
- **Linux Dev SSH :** cgpsoft.dyndns.org:222 > 192.168.0.129:222
- **Linux Dev HTTP :** cgpsoft.dyndns.org:4000 > 192.168.0.129
- **Rancher :** cgpsoft.dyndns.org:2222??? > 192.168.0.128
- **FTP CgpoWeb :** cgpsoft.dyndns.org:2121 > 192.168.0.114:???? (TSE 3408)

## Liste des dockers

| id         | name        | port            |
|------------|-------------|-----------------|
| PATRIMOI   | Patrimoine  | 8010            |
| PATRIMOI2  | patrimoine2 | 8062            |
| PATRIMOI3  | patrimoine3 | 8063            |
| SURAVENIR  | suravenir   | 8007            |
| ORION      | orion       | 8008            |

## Cgpo Test

Host Physique Windows Server TSE : __cgposoft.dyndns.org:2018__

VM Linux :
- Debian Prod : 192.168.0.126
  - Nom machine : cgpoweb
  - Domaine : home
  - root / 2018WBCgp
  - User : twkloic / WbCgp2018
  - Route : __cgposoft.dyndns.org:8800__ > __192.168.0.126:8005__
  - SSH : __cgposoft.dyndns.org:221__ > __192.168.0.126:221__
- Debian Dev : 192.168.0.128
  - Nom machine : 
  - Domaine : home
  - root / 2018WBCgp
  - User : twkloic / WbCgp2018
  - Route : __cgposoft.dyndns.org:__ > __192.168.0.128:__
  - SSH : __cgposoft.dyndns.org:2222__ > __192.168.0.129:22__

# Les procédures

- Lire les logs Linux : `tail -f /var/log/apache2/error.log` dans le dossier `/var/log/apache2`. Les logs seront en direct.
- Ajouter le fichier `.env` :

```shell
# DB
MYSQL_ROOT_PASSWORD=root
MYSQL_USER=admin
MYSQL_PASSWORD=test
MYSQL_DATABASE=db_cgpoweb
MYSQL_DATABASE2=db_webclient
```

## Commandes Linux diverses

- Transformer un fichier .sh en exécutable : `sudo chmod +x filename.sh`
- Copier des fichiers distant grâce à une connexion SSH :
```shell
# De serveur à serveur depuis votre machine locale
scp -r -p user@serveur1:chemin/vers/dossier/source user@serveur2:chemin/vers/dossier/destination
# De serveur à serveur en étant connecté à un serveur
scp -r -p chemin/vers/dossier/source user@serveur2:chemin/vers/dossier/destination
```

## Créer une connexion SSH avec une session distante Linux

Pour ne plus avoir à taper son mot de passe lors d'une connexion à une session distante SSH Linux ([source remote vscode](https://code.visualstudio.com/docs/remote/troubleshooting#_configuring-key-based-authentication)):

- Générer la clé SSH (réponses par défaut) : `ssh-keygen -o -t rsa -b 4096 -C "email@example.com"`
- Récupérer le clé ou la partager :
  - Récupérer : `cat ~/.ssh/id_rsa.pub`
  - Partager diretement sa clé avec l'hôte distant : `ssh-copy-id -p 22 your-user-name-on-host@host-fqdn-or-ip-goes-here` 

## Ajouter/Supprimer des utilisateurs

- Lister les profils utilisateur et les groupes : `cat /etc/passwd`
- Ajouter un utilisateur (username) : `adduser username`
- Choisir son mot de passe dans l'interface dédié et les informations souhaitées (vide = défaut)
- Ajouter l'utilisateur au groupe souhaité (username groupname) : `adduser username groupname` ou `usermod -aG groupname username` {TESTER LES 2 COMMANDES}
- Lister les groupes : `groups`
- Suppression d'un utilisateur et de son home : `deluser --remove-home <username>`

## Changer le port SSH et activer/désactiver la connexion SHH via ROOT

Dans le fichier de configuration `nano /etc/ssh/sshd_config` :
- Retirer le # de la ligne `# Port 22` puis modifier le numéro **22** par le numéro souhaité
- Modifier la ligne `PermitRootLogin yes` par `PermitRootLogin no` en fonction de la configuration souhaitée
- Redémarrer le service : `systemctl restart sshd`

## Le Cron Linux

Le cron (crontab) permet de créer des taches automatisées sous Linux.  
Tuto détaillé [ici](https://www.cyberciti.biz/faq/how-do-i-add-jobs-to-cron-under-linux-or-unix-oses/ "Tuto crontab jobs")
- `crontab -l` liste les différentes tâches créées (`crontab -u username -l` pour un utilisateur précis)
- `crontab -r` supprime tous les crons créés (`crontab -r -u username` supprime tous les crons d'un utilisateur)
- `crontab -e` édite le fichier cron pour ajouter/supprimer des tâches :
```shell
# crontab -e

# Example of job definition:
# .-------------- Minute (0 - 59)
# | .------------ Hour (0 - 23)
# | | .---------- Day of month (1 - 31)
# | | | .-------- Month (1 - 12) OR jan, feb, mar, apr...
# | | | | .------ Day of week (0 - 6) (Sunday=0 or 7) OR sun, mon, tue, wed, thu, fri, sat
# | | | | |
# * * * * * username command to be executed

# Ecrire toutes les minutes dans un fichier, ici la date/heure (all users)
* * * * * date >> /home/ubuntu/Documents/minute.txt
```

## Les commandes Git

- Retirer du dépôt les fichiers ignorés dans le `.gitignore` :

```shell
# remove specific file from git cache
git rm --cached filename

# remove all files from git cache
git rm -r --cached .
git add .
git commit -m ".gitignore is now working" 
```
## Les commandes Docker

- Ajouter l'utilisateur au groupe Docker : `sudo usermod -aG docker $USER`
- Extraire la date et le fuseau horaire d'un conteneur : `docker exec -it MON_CONTAINER_NAME date && cat /etc/timezone`
- Changer l'heure du container : `dpkg-reconfigure tzdata` puis répondre aux questions
- Installer xDebug :
- Installer Nano / Vim :

### Docker MySql

- Effectuer des dump MySql sur le CONTAINER ciblé ([source](https://gist.github.com/spalladino/6d981f7b33f6e0afe6bb)):
```shell
# Backup
docker exec CONTAINER /usr/bin/mysqldump -uroot -ptoor DATABASE > backup.sql
# Restore
cat backup.sql | docker exec -i CONTAINER /usr/bin/mysql -uroot -proot DATABASE
```

## docker-compose.yml

- Se connecter au registry pour pull les conteneurs : `docker login registry.example.com`
- Démarrer les conteneurs : `docker-compose up -d --build --force-recreate`
  - `-d` : pour distached pour lancer les conteneurs en arrière plan
  - `--build` : pour construire l'image avant le lancement
  - `--force-recreate` : pour recréer le conteneur
- Arrêter et supprimer les conteneurs : `docker-compose stop && docker-compose rm -f`
- Configurer l'heure des conteneurs (sources[ici](https://bobcares.com/blog/change-time-in-docker-container/) et [ici](https://stackoverflow.com/questions/39172652/using-docker-compose-to-set-containers-timezones))
`docker exec -it test-ws_java_1 date && cat /etc/timezone`
```shell
  environment:
    TZ: "Europe/Paris"
  command: >
    sh -c "ln -snf /usr/share/zoneinfo/$TZ /etc/localtime
    && echo $TZ > /etc/timezone"
```
ou
```shell
volumes:
  - "/etc/timezone:/etc/timezone:ro"
  - "/etc/localtime:/etc/localtime:ro"
```

## Changer l'IP d'une VM clonée

- Dans le dossier `/etc/network/` modifier le fichier `nano /etc/network/interfaces` comme suit :

```shell
source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
allow-hotplug eth0
iface eth0 inet static
      address 192.168.0.126
      netmask 255.255.255.0
      gateway 192.168.0.1
```

- Redémarrer le service : `service networking restart`

## Ajouter un utilisateur Linux et le lier à GitLab

### Ajouter un utilisateur Linux

[Source](https://www.digitalocean.com/community/tutorials/how-to-add-and-delete-users-on-ubuntu-16-04 "DigitalOcean")
- En root ajouter l'utilisateur : `adduser <username>`

```bash
Adding user 'utilisateur-à-ajouter' ...
Adding new group 'utilisateur-à-ajouter' (1001) ...
Adding new user 'utilisateur-à-ajouter' (1001) with group 'utilisateur-à-ajouter' ...
Creating home directory '/home/utilisateur-à-ajouter' ...
Copying files from '/etc/skel' ...
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
Changing the user information for utilisateur-à-ajouter
Enter the new value, or press ENTER for the default
        Full Name []: 
        Room Number []: 
        Work Phone []: 
        Home Phone []:
        Other []:
Is the information correct? [Y/n] y
```

- Voir les groupes d'un utilisateur : `groups <username>`
- Ajouter l'utilisateur aux différents groupes souhaités : 
```shell
usermod -aG sudo <username>
usermod -aG docker <username>
usermod -aG git <username>
```
- Se déconnecter et se reconnecter avec l'utilisateur afin de rendre effectif les droits SUDO
- Création de la clé SSH de l'utilisateur `ssh-keygen -t rsa -b 4096 -C "dev@cgp-soft.fr"`
- Affichage et récupération de la clé SSH pour l'intégrer à [GitLab.com](https://gitlab.com/profile/keys"GitLab") : `cat /home/<username>/.ssh/id_rsa.pub`
- Suppression d'un utilisateur et de son home : `deluser --remove-home <username>`

### Lier son profil GitLab à la machine
 
Aller dans le profil utilisateur et ajouter la clé SSH

## Installer Apache

[Tuto ici](https://www.cyberciti.biz/faq/how-to-install-linux-apache-mysql-php-lamp-stack-on-debian-9-stretch/ 'Tuto')
- Installer Apache2 : `sudo apt install apache2`
- Activer le ProxyPass : `sudo a2enmod proxy_http`
- Redémarrer le service : `sudo systemctl status apache2`
- Localisation du site : `/var/www/html`
- Modifier les ports exposés : `sudo nano /etc/apache2/ports.conf`
- Modifier les ports exposés : `sudo nano /etc/apache2/sites-available/000-default.conf` -> `DocumentRoot /var/www/mondossier/src`
- **Pour le portail et si .htaccess file** : modifier le fichier `/etc/apache2/sites-available/000-default.conf`. Sous la ligne : `DocumentRoot /var/www/portail-cgpo/src`. Rajouter :
```shell
<Directory /var/www/>
              Options Indexes FollowSymLinks MultiViews
              AllowOverride All
              Order allow,deny
              allow from all
</Directory>
```       

### [En local] Donner les droits à l'utilisateur pour effectuer des modifications dans /var/www/*

- Donner la permission seulement à l'utilisateur pour `/var/www`. Changer le groupe de l'utilisateur (ici joe) : `sudo chgrp joe /var/www`
- Ensuite il faut **chmod** le dossier pour pouvoir écrire avec le groupe joe : `sudo chmod 775 /var/www`
- Pour éditer/supprimer des fichiers : `sudo chown -R joe /var/www/*`
[Source](https://askubuntu.com/questions/239647/how-can-i-give-myself-permission-to-edit-files-in-var-www-without-having-to-use)
- Donner les droits à Apache2 pour qu'il RUN des scripts shell [ici](https://blogmotion.fr/systeme/executer-un-script-shell-avec-permission-root-en-php-1312) dans le fichier `sudo visudo`. Ajouter les lignes
  - `www-data ALL=(ALL) NOPASSWD: /var/www/portail-cgpo/src/admin/deploiement/deploy_docker.sh`
  - `www-data ALL=(ALL) NOPASSWD: /var/www/portail-cgpo/src/admin/deploiement/configure_ports.sh`

## Installer PHP

**Installer php7.2 au minimum**

`tcpdump -n`
`tcpdump -n port 80`

- Lister les outils php installés : `apt list \*php\* --installed`
  - Désinstaller PHP : `sudo apt purge php7.*`

- Installer PHP : 
  - Ubuntu : `sudo apt install -y php php-xdebug`
  - Debian : `sudo apt install -y php7.2 php-xdebug`
    - Pour installer les dépendances Debian suivre le tuto [ici](https://www.rosehosting.com/blog/how-to-install-php-7-2-on-debian-9/)
- Redémarrer PHP/Apache2 : `sudo systemctl restart apache2`

### Modules PHP

```shell
sudo apt install ca-certificates apt-transport-https 
wget -q https://packages.sury.org/php/apt.gpg -O- | sudo apt-key add -
echo "deb https://packages.sury.org/php/ stretch main" | sudo tee /etc/apt/sources.list.d/php.list
```
- Installer les modules PHP : `sudo apt -y install php-xdebug php-curl php-gd php-intl php-json php-mbstring php-mongodb php-mysql php-xml php-zip`
- Installer les modules PHP 7.2 : `sudo apt -y install php-xdebug php7.2-curl php7.2-gd php7.2-intl php7.2-json php7.2-mbstring php7.2-mongodb php7.2-mysql php7.2-xml php7.2-zip`
- Lister les modules : `apt list php-\*`

### Configuration

- La configuration de php se fait par le fichier `/etc/php/7.2/apache2/php.ini`. [Ici](https://git.php.net/?p=php-src.git;a=blob;f=php.ini-production;hb=HEAD 'php.ini') se trouve la derniere version du fichier `php.ini`
  - Tester la configuration trouvée sur [ce site](https://forums.docker.com/t/how-to-get-access-to-php-ini-file/68986/11) qui configure le fichier `conf.d` (dans un docker voici le chemin `/usr/local/etc/php/conf.d/`) et voir si l'ajout d'un fichier ini dans ce dossier **conf.d** overwrite le `php.ini`

**Les chemins peuvent changer selon les versions de php installées**

On peut configurer plusiers options dans le fichier `php.ini` grâce à la commande ` nano /etc/php/7.2/apache2/php.ini` :
- Activer/Désactiver les extentions souhaités en ajoutant/supprimant le `;` devant l'extention désirée -> `;extension=php_pdo_mysql.dll`
  - Activer l'utilisation de XML : `extension=php_xmlrpc.dll`
  - Modifier le temps maximum d'exécution d'un script PHP : `max_execution_time`
  - Augmenter la tailler max des fichiers upload via PHP : 
    - `upload_max_filesize = 2M` > `upload_max_filesize = 100M`
    - `post_max_size = 8M` > `post_max_size = 100M`

## Installer MariaDB

- Installer la base de données : `sudo apt install mariadb-server`
- Configurer la base de données (mdp root) : `sudo mysql_secure_installation`
- Se connecter à la base en root : `sudo mysql -u root -p`
- Créer le USER / PASSWORD :
```shell
CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON *.* TO 'newuser'@'localhost';
```
- Se connecter à la base avec le USER sans SUDO : `mysql -u newuser -p`
- Importer un fichier .sql : `mysql -u admin -p < mon_fichier.sql`

# Déploiement de MonEcgp sur un nouveau serveur

- Créer l'arborescence des projets sur le serveur :
  - Serveur de test : `mkdir /docker/monecgp/cabinets`
  - Serveur de production OVH : `/home/cgp/docker/monecgp/cabinets`
- Créer les **users** qui se connecteront au serveur ([ici](#ajouter-un-utilisateur-linux))
- Créer le groupe qui gèrera l'ensemble des projets ([ici](#ajouter-un-utilisateur-linux)) : `sudo addgroup monecgp`
- Ajouter les utilisateurs au groupe *monecgp* : `sudo usermod -aG monecgp $USER` / `sudo usermod -aG monecgp www-data` / `cat /etc/group`
- Changer le groupe principal du user commun : `sudo groupmod -g monecgp cgpsoft`
- Changer le groupe des dossiers du projet sur le serveur (groupname=monecgp) : 
```shell
sudo chown -R username ./file/folder
sudo chgrp -R groupname ./file/folder
sudo chmod -R g+swX ./file/folder
```
- Installer git, docker
- Installer apache2, méthode [ici](#installer-apache)
- Installer PHP, méthode [ici](#installer-php)
  - **[Ne pas faire > a tester]** Activer `extension=php_pdo_mysql.dll` dans les extentions de `php.ini`
- Insteller mariaDB et créer l'utilisateur
- Configurer Git :
  - `git config --global user.name "CgpSoftCorp"`
  - `git config --global user.email "dev@cgp-soft.fr"`
- Connecter en SSH au compte GitLab via la procédure suivance : [ici](#créer-une-connexion-ssh-avec-une-session-distante-linux)
- Se connecter au REGISTRY du projet **process-and-projects** et entrer les identifiants : `docker login registry.gitlab.com` (si errer essayer de redémarrer la machine)
  - **ERREUR** : `Error saving credentials: error storing credentials - err: exit status 1, out: 'Cannot autolaunch D-Bus without X11 $DISPLAY'` faire les commandes suivantes :
    - `sudo apt remove docker-compose`
    - `sudo apt remove golang-docker-credential-helpers`
    - Réinstaller `docker-compose` une fois la connexion au registry effectuée : `sudo apt install docker-compose`
- Télécharger l'ensemble des projets :
```shell
git clone git@gitlab.com:cgpsoft/ws-monecgp.git
git clone git@gitlab.com:cgpsoft/process-and-projects.git
git clone git@gitlab.com:cgpsoft/cgpo-web.git commun-cgpoweb
git clone git@gitlab.com:cgpsoft/webclient.git commun-webclient
git clone git@gitlab.com:cgpsoft/crm-erp-cgpo.git commun-crm
# Donner les droits au groupe d'éditer les projets

cd ./ws-monecgp
docker-compose up -d --build
cd ..
```
- Donner aux différents repository le groupe **monecgp**
- Passer l'ensemble des projets en dossier git partagé et changer le créateur et le groupe du repository : 
  - Changer le partage du projet en groupe : `git config core.sharedRepository group`
  - Changer l'*owner* et le *group* du projet : `sudo chgrp -R <gid> .` & `sudo chmod -R g+swX .` & `sudo chown -R <uid> .`
ATTENTION : Le projet **process-and-projects** à été transféré dans le projet **portail-cgpo**
- Donner les droits à Apache2 pour qu'il RUN des scripts shell [ici](https://blogmotion.fr/systeme/executer-un-script-shell-avec-permission-root-en-php-1312) dans le fichier `sudo visudo`. Ajouter les lignes
  - `www-data ALL=(ALL) NOPASSWD: /var/www/portail-cgpo/deploiement/deploy_docker.sh`
  - `www-data ALL=(ALL) NOPASSWD: /var/www/portail-cgpo/deploiement/configure_ports.sh`
- Importer la base de données **correspondance** du portail se trouvant dans le projet **process-and-projects** : `mysql -u username -p correspondance < portail.sql`
- Vérifier dans le projet **process-and-projects** :
  - Vérifier les identifiants à la base de données locale ligne 30/31 du fichier `deploy_docker.sh`
  - L'adresse IP 127.0.0.1 configurée ligne 57 du fichier `configure_ports.sh`
- Créer le fichier `conf.ini` dans le projet CGPOWEB : `echo ipAdressWS=IP_DE_LA_MACHINE_HOTE > ./commun-cgpoweb/src/commun/conf.ini`
- Aller dans le dossier process-and-projects : `cd process-and-projects`
- Lancer les différents scripts en respectant les SUDO :
  - `./deploy_docker.sh`
  - `sudo ./configure_ports.sh`
- Installer le site du **Portail MonEcgp** :
  - Aller dans le répertoire d'Apache : `cd /var/www/`
  - Télécharger le site du Portail : `git clone git@gitlab.com:cgpsoft/portail-cgpo.git`
  - Modifier les accès apache2 :
    - Configurer le port : `sudo nano /etc/apache2/ports.conf` > Mettre le port souhaité `Listen 80`
    - Créer le Vhost ou modifier le chemin d'acces du site : `sudo nano /etc/apache2/sites-available/000-default.conf ` > `DocumentRoot /home/cgp/docker/portail-cgpo/src/`
    - Redémarrer le service apache2 : `systemctl restart apache2`
- Mettre en place la sécurité du serveur avec le fichier de configuration `.htaccess` : 
```shell
nano commun-cgpoweb/src/.htaccess
nano commun-webclient/src/.htaccess
nano portail-cgpo/src/.htaccess
```

- Y insérer les valeurs :
```
Options -Indexes

php_value memory_limit 100M
php_value post_max_size 100M
php_value upload_max_filesize 100M
```


# Status du Firewall avant execution du script

Regarder les données et comparer avant/après execution du script firewall.sh.

```shell
Status IPV4
-----------------------------------------------
Chain INPUT (policy ACCEPT 11172 packets, 5382K bytes)
 pkts bytes target     prot opt in     out     source               destination         

Chain FORWARD (policy DROP 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination         
  524  808K DOCKER-USER  all  --  *      *       0.0.0.0/0            0.0.0.0/0           
  524  808K DOCKER-ISOLATION-STAGE-1  all  --  *      *       0.0.0.0/0            0.0.0.0/0           
  340 21774 ACCEPT     all  --  *      docker0  0.0.0.0/0            0.0.0.0/0            ctstate RELATED,ESTABLISHED
    5   300 DOCKER     all  --  *      docker0  0.0.0.0/0            0.0.0.0/0           
  179  786K ACCEPT     all  --  docker0 !docker0  0.0.0.0/0            0.0.0.0/0           
    0     0 ACCEPT     all  --  docker0 docker0  0.0.0.0/0            0.0.0.0/0           

Chain OUTPUT (policy ACCEPT 4339 packets, 695K bytes)
 pkts bytes target     prot opt in     out     source               destination         

Chain DOCKER (1 references)
 pkts bytes target     prot opt in     out     source               destination         
    0     0 ACCEPT     tcp  --  !docker0 docker0  0.0.0.0/0            172.17.0.2           tcp dpt:2002
    0     0 ACCEPT     tcp  --  !docker0 docker0  0.0.0.0/0            172.17.0.3           tcp dpt:3306
    0     0 ACCEPT     tcp  --  !docker0 docker0  0.0.0.0/0            172.17.0.4           tcp dpt:3306
    0     0 ACCEPT     tcp  --  !docker0 docker0  0.0.0.0/0            172.17.0.5           tcp dpt:3306
    0     0 ACCEPT     tcp  --  !docker0 docker0  0.0.0.0/0            172.17.0.6           tcp dpt:3306
    0     0 ACCEPT     tcp  --  !docker0 docker0  0.0.0.0/0            172.17.0.8           tcp dpt:80
    0     0 ACCEPT     tcp  --  !docker0 docker0  0.0.0.0/0            172.17.0.7           tcp dpt:80
    0     0 ACCEPT     tcp  --  !docker0 docker0  0.0.0.0/0            172.17.0.9           tcp dpt:80
    5   300 ACCEPT     tcp  --  !docker0 docker0  0.0.0.0/0            172.17.0.10          tcp dpt:80

Chain DOCKER-ISOLATION-STAGE-1 (1 references)
 pkts bytes target     prot opt in     out     source               destination         
  179  786K DOCKER-ISOLATION-STAGE-2  all  --  docker0 !docker0  0.0.0.0/0            0.0.0.0/0           
  524  808K RETURN     all  --  *      *       0.0.0.0/0            0.0.0.0/0           

Chain DOCKER-ISOLATION-STAGE-2 (1 references)
 pkts bytes target     prot opt in     out     source               destination         
    0     0 DROP       all  --  *      docker0  0.0.0.0/0            0.0.0.0/0           
  179  786K RETURN     all  --  *      *       0.0.0.0/0            0.0.0.0/0           

Chain DOCKER-USER (1 references)
 pkts bytes target     prot opt in     out     source               destination         
  524  808K RETURN     all  --  *      *       0.0.0.0/0            0.0.0.0/0           

-----------------------------------------------

status IPV6
-----------------------------------------------
Chain INPUT (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination         

Chain FORWARD (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination         

Chain OUTPUT (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination         
```

## Erreurs

### Problème de résolution de noms de domaine

#### Problèmes rencontrés pour Ping l'extérieur ou importer les dépots Git 

```shell
root@cgpoweb-dev:/var/www# git clone https://github.com/MushuLeDragon/nodejs-tuto.git
Clonage dans 'nodejs-tuto'...
fatal: unable to access 'https://github.com/MushuLeDragon/nodejs-tuto.git/': Could not resolve host: github.com
```

- Tenter de ping l'adresse www.google.fr, si ca ne passe pas tester de ping l'adresse ip de google en executant la  commande `nslookup www.google.fr` pour récupérer l'adresse ip 172.217.19.35.
- Résoudre le problème en ajoutant les **nameserver** dans le fichier `nano /etc/resolv.conf`
```shell
# Generated by NetworkManager
nameserver fd0f:ee:b0::1
nameserver 212.27.53.252
nameserver 8.8.8.8
```

#### Problème lors de l'appel de la page à la connexion
```
Échec de la connexion : SQLSTATE[HY000] [2002] php_network_getaddresses: getaddrinfo failed: Name or service not known
```
- Erreur lors de l'execution d'une page PHP
- Explication > Vérifier que le `env.php` ait bien toutes les connexions
- Solution > Mettre à jour le fichier :
```php
<?php 
# DB
define('MYSQL_USER', 'portailCgpo34');
define('MYSQL_PASSWORD', 'WbCgp2018*');
define('MYSQL_DATABASE', 'correspondance');
define('MYSQL_HOST', 'localhost');
define('MYSQL_PORT', '3306');
# Definition of the mysql user for the database connection
define('DOCKER_MYSQL_HOST', '127.0.0.1');
define('DOCKER_MYSQL_USER', 'cabinetmonEcgp');
define('DOCKER_MYSQL_USERPWD', '34980monEcgpCab');
# Dockers db name
define('DOCKER_DB_NAME_CRM', 'db_crm');
define('DOCKER_DB_NAME_CGPOWEB', 'db_cgpoweb');
define('DOCKER_DB_NAME_WEBCLIENT', 'db_webclient');
?>
```

#### Problème lors du déploiement d'un cabinet

- Erreur déploiement d'un cabinet, logs d'apache2 `sudo: no tty present and no askpass program specified`

Cela peut provenir d'une mauvaise adresse d'exécution du script renseigné dans le fichier **sudoers** `sudo visudo`. Vérifier que le chemin d'accès au script est correct.

#### Problème de perte de $_SESSION et redirection sur monEcgp.fr

- Vider le cache et tenter de se connecter aux 2 sites web.
- Vérifier les cookies des deux sites
