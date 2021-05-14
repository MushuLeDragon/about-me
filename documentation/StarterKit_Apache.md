# Installer Apache

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
