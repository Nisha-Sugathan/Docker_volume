# Docker_volume
Docker volume is a mechanism to store  the container generated data persistently. Volume persisting data in a container’s read writable layer

### Prerequisites
Need to install docker on your machine
Add your username to docker group

### Docker Installation

```
sudo yum install docker -y &> /dev/null
sudo systemctl restart docker.service
sudo systemctl enable docker.service
sudo usermod -a -G docker ec2-user
```
![docker_installation](https://github.com/Nisha-Sugathan/Docker-Bind_mounting/assets/134600837/ba7797c4-9a73-4ce6-b593-2befa5850e0d)

### Create volume for mysql
```
docker volume create V1_mysql

docker volume inspect  V1_mysql
```
Screenshot of volume

![Volume](https://github.com/Nisha-Sugathan/Docker_volume/assets/134600837/3048c75f-da42-49d0-b8e2-b2a037e60c98)


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

Screenshot of MySQL access by collecting old information


![wrong_password](https://github.com/Nisha-Sugathan/Docker_volume/assets/134600837/a99459cd-8ca9-42bd-8574-63d3baba44a0)

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

Screenshot of MySQL access by collecting old information


![wrong_password](https://github.com/Nisha-Sugathan/Docker_volume/assets/134600837/a99459cd-8ca9-42bd-8574-63d3baba44a0)
