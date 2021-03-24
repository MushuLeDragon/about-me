# Docker

## Commands

- Add Docker to the Linux group when installing : `sudo usermod -aG docker $USER`.
- Get the date of the container : `docker exec -it CONTAINER date && cat /etc/timezone`.
- Change the date/hour of the container : `dpkg-reconfigure tzdata` then answer question.
- Installer xDebug :
- Installer Nano / Vim :

## Database

MySql query / import file / dump / restore ([source](https://gist.github.com/spalladino/6d981f7b33f6e0afe6bb)):

```shell
# Backup
docker exec CONTAINER /usr/bin/mysqldump -uroot -ptoor DATABASE > backup.sql
# Restore
cat backup.sql | docker exec -i CONTAINER /usr/bin/mysql -uroot -proot DATABASE

# Import a SQL file
docker exec -i CONTAINER mysql -u USER -pPASSWORD DATABASE < path/to/sql/file.sql

# Query from outside the container
docker exec -it CONTAINER mysql -u USER -pPASSWORD DATABASE -e "show columns from user;"
```
