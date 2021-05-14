# Installer MariaDB

- Installer la base de données : `sudo apt install mariadb-server`
- Configurer la base de données (mdp root) : `sudo mysql_secure_installation`
- Se connecter à la base en root : `sudo mysql -u root -p`
- Créer le USER / PASSWORD :
- 
```shell
CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON *.* TO 'newuser'@'localhost';
```

- Se connecter à la base avec le USER sans SUDO : `mysql -u newuser -p`
- Importer un fichier .sql : `mysql -u admin -p < mon_fichier.sql`
