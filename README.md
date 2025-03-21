# Moodle-Docker

Desarrollo de Moodle en entorno Docker con PHP 8.3 y base de datos MySQL.

## Descripción

Este repositorio contiene la configuración necesaria para desplegar un entorno de desarrollo de Moodle utilizando Docker. Incluye un servidor Apache, una base de datos MySQL y PHP 8.3 con todas las extensiones necesarias para la instalación y configuración de Moodle.

## Requisitos

- **Docker**: Instala [Docker Desktop](https://www.docker.com/products/docker-desktop/) en Windows o Docker en Linux (Ubuntu/Debian).
- **Docker Compose**: Requerido para gestionar los servicios.
- **Git**: Necesario para clonar el repositorio.

## Instalación

Sigue estos pasos para instalar y levantar el entorno:

1. **Clona el repositorio**:
   ```bash
   git clone https://github.com/jmunoz-academia/Moodle-Docker.git
   cd Moodle-Docker/moodle
   ```

2. **Construye y levanta los contenedores**:
   ```bash
   docker-compose up -d
   ```

3. **Verifica los contenedores activos**:
   ```bash
   docker ps
   ```

## Configuración

### 1. Configuración de permisos para `moodledata`
Una vez levantados los contenedores, se creará la carpeta `moodledata` en la raíz del proyecto. Ajusta los permisos:
```bash
chown -R 33:33 moodledata/
```

### 2. Configuración de la base de datos MySQL
Accede al contenedor de MySQL:
```bash
docker exec -it mysql bash
```
Dentro del contenedor, ejecuta:
```sql
mysql -u root -p
CREATE DATABASE moodle;
CREATE USER 'admin'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON moodle.* TO 'admin'@'localhost' WITH GRANT OPTION;
EXIT;
```
Sal con `exit`.

### 3. Descarga de Moodle en el contenedor Apache
Accede al contenedor de Apache e instala Git:
```bash
docker exec -it apache bash
apt update && apt install git
```
Descarga Moodle (versión 4.5 compatible con PHP 8.3):
```bash
git clone -b MOODLE_405_STABLE --single-branch https://github.com/moodle/moodle.git /var/www/html
```
Reinicia el servidor Apache:
```bash
service apache2 restart
```
Sal con `exit`.

### 4. Configuración de Moodle
- En la raíz del proyecto, se creará `public-html`, un enlace simbólico a `/var/www/html` (definido en `docker-compose.yml`).
- Visita `http://localhost/moodle` para iniciar la configuración web.
- En **Ajustes de base de datos**:
  - **Servidor de la base de datos**: `mysql` (nombre del contenedor, no `localhost`).
  - **Usuario**: `root` (o el definido en `docker-compose.yml`).
  - **Contraseña**: La de `MYSQL_ROOT_PASSWORD` en `docker-compose.yml` (por defecto, `password`).
- Copia el contenido de `config.php` generado y pégalo en `./public-html/moodle/config.php`. Recarga la página para continuar.

## Archivo `docker-compose.yml`

Configuración de los servicios:

```yaml
name: docker-git
services:
  mysql:
    container_name: mysql
    environment:
      - MYSQL_ROOT_PASSWORD=password
    image: mysql:latest
    volumes:
      - mysql_data:/var/lib/mysql
    networks:
      - app-network

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
  app-network:

volumes:
  mysql_data:
```

## Extensiones de PHP

El contenedor de PHP 8.3 incluye:
- `mysqli`
- `pdo_mysql`
- `gd`
- `xml`
- `intl`
- `opcache`
- `curl`
- `mbstring`
- `zip`
```

### Instrucciones para subir como `README.md`
1. Copia el contenido anterior en un archivo llamado `README.md`.
2. Colócalo en la raíz de tu repositorio (`Moodle-Docker`).
3. Súbelo a GitHub con:
   ```bash
   git add README.md
   git commit -m "Agrega README.md con instrucciones"
   git push origin main
   ```

Este formato es compatible con GitHub, incluye enlaces útiles (como el de Docker Desktop), y usa bloques de código para comandos y configuraciones, lo que lo hace fácil de leer y seguir. ¡Listo para tu repositorio!
