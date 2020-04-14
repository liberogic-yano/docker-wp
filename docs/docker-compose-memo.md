# Compose file

Completed the `docker-compose.yml` file
```yaml
version: '3.3'

services:
  db:
    image: mysql:5.7
    container_name: db
    environment:
      MYSQL_ROOT_PASSWORD: 1234
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
      TZ: Asia/Tokyo
    restart: always
    volumes:
      - db_data:/var/lib/mysql
    networks:
      - app-network

  wp:
    depends_on:
      - db
    image: wordpress:php7.3-fpm-alpine
    container_name: wp
    ports:
      - '8080:80'
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
    volumes:
      - wp:/var/www/html
    networks:
      - app-network

  web:
    depends_on:
      - wp
    image: nginx:1.15.12-alpine
    container_name: web
    restart: always
    ports:
      - '80:80'
    volumes:
      - wp:/var/www/html
      - ./conf:/etc/nginx/conf.d
    networks:
      - app-network

volumes:
  db_data: {}
  wp: {}
networks:
  app-network:
    driver: bridge
```

The `nginx.conf` file.
```
server {
        listen 80;
        listen [::]:80;

        server_name localhost;

        index index.php index.html index.htm;

        root /var/www/html;

        location ~ /.well-known/acme-challenge {
                allow all;
                root /var/www/html;
        }

        location / {
                try_files $uri $uri/ /index.php$is_args$args;
        }

        location ~ \.php$ {
                try_files $uri =404;
                fastcgi_split_path_info ^(.+\.php)(/.+)$;
                fastcgi_pass wp:9000;
                fastcgi_index index.php;
                include fastcgi_params;
                fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
                fastcgi_param PATH_INFO $fastcgi_path_info;
        }

        location ~ /\.ht {
                deny all;
        }

        location = /favicon.ico { 
                log_not_found off; access_log off; 
        }
        location = /robots.txt { 
                log_not_found off; access_log off; allow all; 
        }
        location ~* \.(css|gif|ico|jpeg|jpg|js|png)$ {
                expires max;
                log_not_found off;
        }
}
```
## TODO
- Use dotenv.


# Links
- @ref https://docs.docker.com/compose/
- @ref https://www.digitalocean.com/community/tutorials/how-to-install-wordpress-with-docker-compose
- @ref https://docs.docker.com/engine/swarm/configs/
- @ref https://docs.docker.com/compose/environment-variables/
