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

2. Copia el archivo `config.docker-template.php` a `config.docker.php` y edita los valores según tus necesidades:

   ```bash
   cp config.docker-template.php config.docker.php
   ```

3. Construye y levanta los contenedores:

   ```bash
   docker-compose up --build
   ```

4. Accede a Moodle desde tu navegador en `http://localhost`.

## Servicios

- **Apache**: Servidor web para alojar Moodle.
- **MySQL**: Base de datos para almacenar la información de Moodle.
- **PHP 8.2**: Versión de PHP con todas las extensiones necesarias para Moodle.

## Configuración

El archivo `docker-compose.yml` incluye la configuración de los servicios:

```yaml
version: '3.8'

services:
  web:
    image: php:8.2-apache
    ports:
      - "80:80"
    volumes:
      - ./moodle:/var/www/html
    environment:
      - MOODLE_DOCKER=true

  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: example
      MYSQL_DATABASE: moodle
      MYSQL_USER: moodle
      MYSQL_PASSWORD: moodle

  php:
    image: php:8.2-fpm
    volumes:
      - ./moodle:/var/www/html
    environment:
      - MOODLE_DOCKER=true
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

## Contribuciones

Las contribuciones son bienvenidas. Por favor, abre un issue o un pull request para discutir cualquier cambio que desees realizar.

## Licencia

Este proyecto está licenciado bajo los términos de la licencia MIT. Consulta el archivo `LICENSE` para más detalles.
