# Moodle-Docker

Desarrollo de Moodle en entorno Docker con PHP 8.2 y base de datos MySQL. Versión de MOODLE_402_STABLE.

## Descripción

Este repositorio contiene la configuración necesaria para desplegar un entorno de desarrollo de Moodle utilizando Docker. El contenedor Docker incluye un servidor Apache, una base de datos MySQL y PHP 8.2 con todas las extensiones necesarias para la instalación y configuración de Moodle.

## Requisitos

- Docker
- Docker Compose

## Instalación

1. Clona este repositorio:

   ```bash
   git clone https://github.com/jmunoz-academia/Moodle-Docker.git
   cd Moodle-Docker
   ```
2. Construye y levanta los contenedores:

   ```bash
   docker-compose up -d
   ```

4. Accede a Moodle desde tu navegador en `http://localhost/moodle`.

## Servicios

- **Apache**: Servidor web para alojar Moodle.
- **MySQL**: Base de datos para almacenar la información de Moodle.
- **PHP 8.2**: Versión de PHP con todas las extensiones necesarias para Moodle.

## Configuración

El archivo `docker-compose.yml` incluye la configuración de los servicios:

```yaml
name: docker-git
services:
  mysql:
    container_name: mysql
    environment:
      - MYSQL_ROOT_PASSWORD=password
    image: mysql:latest
    volumes:
      - mysql_data:/var/lib/mysql  # Para persistir datos de la base de datos
    networks:
      - app-network  # Definir red para permitir la comunicación entre servicios

  php:
    container_name: apache
    build:
      context: ./php-docker
      dockerfile: Dockerfile
    ports:
      - 80:80
    volumes:
      - ./public-html:/var/www/html
      - ./moodledata:/var/www/moodledata
    depends_on:
      - mysql
    networks:
      - app-network

networks:
  app-network:  # Definir la red compartida para los contenedores

volumes:
  mysql_data:  # Definir volumen persistente para la base de datos
```

## Extensiones de PHP

El contenedor de PHP 8.2 incluye las siguientes extensiones necesarias para Moodle:

- mysqli
- pdo_mysql
- gd
- xml
- intl
- opcache
- curl
- mbstring
- zip
