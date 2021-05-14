# Installer PHP

`tcpdump -n`
`tcpdump -n port 80`

- Lister les outils php installés : `apt list \*php\* --installed`
  - Désinstaller PHP : `sudo apt purge php8.*`

- Installer PHP : 
  - Ubuntu : `sudo apt install -y php php-xdebug`
  - Debian : `sudo apt install -y php8.0 php-xdebug`
    - Pour installer les dépendances Debian suivre le tuto [ici](https://www.rosehosting.com/blog/how-to-install-php-7-2-on-debian-9/)
- Redémarrer PHP/Apache2 : `sudo systemctl restart apache2`

## Modules PHP

```shell
sudo apt install ca-certificates apt-transport-https 
wget -q https://packages.sury.org/php/apt.gpg -O- | sudo apt-key add -
echo "deb https://packages.sury.org/php/ stretch main" | sudo tee /etc/apt/sources.list.d/php.list
```
- Installer les modules PHP : `sudo apt -y install php-xdebug php-curl php-gd php-intl php-json php-mbstring php-mongodb php-mysql php-xml php-zip`
- Installer les modules PHP 8.0 : `sudo apt -y install php-xdebug php8.0-curl php8.0-gd php8.0-intl php8.0-json php8.0-mbstring php8.0-mongodb php8.0-mysql php8.0-xml php8.0-zip`
- Lister les modules : `apt list php-\*`
