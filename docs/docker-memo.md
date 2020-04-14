# Pull Images

## MySQL
https://hub.docker.com/_/mysql

```
$ docker pull mysql
```

## WordPress
https://hub.docker.com/_/wordpress/

```
$ docker pull wordpress
```

# Run Container

## MySQL
```
$ docker run --name db -v /Users/Libero_Imac/user_projects/__Docker/test/db:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=1234 -d mysql:5.7
```

## WordPress
```
$ docker run --name wp -v /Users/Libero_Imac/user_projects/__Docker/test/wp --link db:mysql -d -p 8080:80 wordpress
```

http://localhost:8080/wp-admin/
test / %^Gnn4gQv0v8iLOs4R

# Exec
```
$ docker exec -it wp bash
```

# Copy files

copy mysqld.cnf
```
$ docker cp db:/etc/mysql/mysql.conf.d/mysqld.cnf ./conf/mysql/
```

copy wp-config.php
```
$ docker cp wp:/var/www/html/wp-config.php ./conf/wp/
```

# Start/Stop Container

## Start
```
$ docker start [CONTAINER-NAME or CONTAINER-ID]
```

## Stop
```
$ docker stop [CONTAINER-NAME or CONTAINER-ID]
```

# Remove Containers

```
$ docker container ls -a
```

```
$ docker container rm [CONTAINER-ID]...[CONTAINER-ID]
```


# Docker Compose

1. Create an empty project directory.
```
$ mkdir wordpress && cd wordpress
```

2. Create `docker-compose.yml` file.
```
$ nano docker-compose.yml
```

Example `docker-compose.yml`
```yaml
version: '3.3'

services:
  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: 1234
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
      TZ: 'Asia/Tokyo'
    restart: always
    volumes:
      - db_data:/var/lib/mysql
  wp:
    depends_on:
      - db
    image: wordpress:latest
    ports:
      - '8080:80'
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
volumes:
  db_data: {}
```

3. Build the project
```
$ docker-compose up -d
```

4. Shutdown and cleanup
The command removes the containers and default network, but preserves your WordPress database.
```
$ docker-compose down
```

The command removes the containers, default network, and the WordPress database.
```
$ docker-compose down --volumes
```

@ref - https://docs.docker.com/compose/wordpress/
@ref - https://www.digitalocean.com/community/tutorials/how-to-install-wordpress-with-docker-compose
@ref - https://www.digitalocean.com/community/tutorials/how-to-remove-docker-images-containers-and-volumes
