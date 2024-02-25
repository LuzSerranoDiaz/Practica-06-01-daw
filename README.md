# Practica-06-01-daw

## .env
```
MYSQL_ROOT_PASSWORD=root
MYSQL_DATABASE=prestashop
MYSQL_USER=ps_user
MYSQL_PASSWORD=ps_password
DOMAIN=dockerp06-01.ddns.net
```

## docker-compose

```yml
version: '3.4'

services:
  mysql:
    image: mysql:8.0
```
Utiliza la imagen mysql version 8.0
```yml
    environment: 
      - MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD}
      - MYSQL_DATABASE=${MYSQL_DATABASE}
      - MYSQL_USER=${MYSQL_USER}
      - MYSQL_PASSWORD=${MYSQL_PASSWORD}
```
Se indica los datos de usuario y la base de datos.
```yml
    volumes: 
      - mysql_data:/var/lib/mysql
    networks: 
      - backend-network
    restart: always
```
Se indica el volumen, la network y que cada vez que el contenedor se reinicie que el servicio tambien inicie.
```yml
  
  phpmyadmin:
    image: phpmyadmin:5.2
```
Utiliza la imagen phpmyadmin version 5.2
```yml
    ports:
      - 8080:80
```
Se abre los puertos 8080
```
    environment: 
      - PMA_HOST=mysql
```
Se indica el host
```
    networks: 
      - backend-network
      - frontend-network
    restart: always
    depends_on: 
      - mysql
```
Se indican las networks, que depende de mysql y que cada vez que el contenedor se reinicie que el servicio tambien inicie.
```

  prestashop:
    image: prestashop/prestashop:8
```
Utiliza la imagen phpmyadmin version 5.2
```yml
    environment: 
      - DB_SERVER=mysql
      - DB_NAME=${MYSQL_DATABASE}
      - DB_USER=${MYSQL_USER}
      - DB_PASSWORD=${MYSQL_PASSWORD}
```
Se indica los datos de usuario y la base de datos.
```yml
    volumes:
      - prestashop_data:/var/www/html
    networks: 
      - backend-network
      - frontend-network
    restart: always
    depends_on: 
      - mysql
```
Se indica el volumen, la network, que depende en mysql y que cada vez que el contenedor se reinicie que el servicio tambien inicie.
```yml
  

  https-portal:
    image: steveltn/https-portal:1
```
Utiliza la imagen phpmyadmin version 5.2
```yml
    ports:
      - 80:80
      - 443:443
```
Abre los puertos 80 y 443
```yml
    restart: always
```
Que el contenedor se reinicie que el servicio tambien inicie.
```yml
    environment:
      DOMAINS: '${DOMAIN} -> http://prestashop:80'
      STAGE: 'production'
```
Indica que al entrar al contenedor por los puertos redirije al servicio prestashop por el puerto 80
```yml
    networks:
      - frontend-network
```
Indica el network
```yml
volumes:
  mysql_data:
  prestashop_data:

networks: 
  backend-network:
  frontend-network:
```
Indica los volumenes y las network
