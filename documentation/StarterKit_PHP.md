# Installer PHP

`tcpdump -n`
`tcpdump -n port 80`

- Lister les outils php installés : `apt list \*php\* --installed`
  - Désinstaller PHP : `sudo apt purge php7.*`

- Installer PHP : 
  - Ubuntu : `sudo apt install -y php php-xdebug`
  - Debian : `sudo apt install -y php7.2 php-xdebug`
    - Pour installer les dépendances Debian suivre le tuto [ici](https://www.rosehosting.com/blog/how-to-install-php-7-2-on-debian-9/)
- Redémarrer PHP/Apache2 : `sudo systemctl restart apache2`
