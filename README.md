# Docker_volume
Docker volume is a mechanism to store  the container generated data persistently. Volume persisting data in a container’s read writable layer



### Create volume for mysql
```
docker volume create V1_mysql

docker volume inspect  V1_mysql
```

### create container for mysql and mount on local machine
```
docker container run -d --name mysql-server  \
-p 3306:3306 \
--restart always \
-v V1_mysql:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD="mysqlroot123"  \
-e  MYSQL_DATABASE="wordpress" \
-e MYSQL_USER="wpuser"  \
-e MYSQL_PASSWORD="wpuser123"  \
mysql:8.0.33-debian
```


### create new databases in mysql server
```
docker container inspect mysql-server

 mysql -u root -pmysqlroot123 -h 172.17.0.2
 ```
 
 ### Remove the docker container
 ```
docker container stop mysql-server
docker container rm mysql-server
```
### The data in the volume still persist in the mount path
```
sudo ls -al /var/lib/docker/volumes/mysql-volume/_data
```
### Recreate the container with the wrong username and password
```
docker container run -d --name mysql-server  \
-p 3306:3306 \
--restart always \
-v V1_mysql:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD="mysqlroot123"  \
-e  MYSQL_DATABASE="wordpress" \
-e MYSQL_USER="wpuser123"  \
-e MYSQL_PASSWORD="wpuser345"  \
mysql:8.0.33-debian
```

### Recreate the container without environment variables
```
docker container stop mysql-server
docker container rm mysql-server
docker container run -d --name mysql-server  \
-p 3306:3306 \
--restart always \
-v V1_mysql:/var/lib/mysql \
mysql:8.0.33-debian
```
