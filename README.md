# Laravel 10.10 - Template

Este repositorio sirve de base para los proyectos basados en el framework php Laravel (https://laravel.com) versión 10.10 que deban empaquetarse en un contenedor Docker basado en la imagen maomuriel/ubun22_04-php8_1-apache2_4:0.2.4.

Este proyecto no incluye ninguna librería adicional o ejecución de los comandos de instalación de Laravel.

## Requisitos

Se necesita que la máquina donde se va instalar el proyecto incluya:

- Git
- Docker. [Docker Desktop](https://docs.docker.com/desktop/setup/install/windows-install/) para plataformas Windows , [Docker Engine](https://docs.docker.com/engine/install/) para las demás plataformas.


## Instalación


1. Tener instalados: docker (engine o desktop según la plataforma) y git en su máquina.

2. Crear una carpeta para el proyecto:

```
$ mkdir /path/to/project
```

3. Clonar este proyecto en la carpeta recién creada:

```
$ cd /path/to/project
$ git init
$ git branch -m "main"
$ git remote add origin git@github.com:mmuriel/laravel10-10_template.git
$ git pull origin main
```

4. Generar el archivo docker-compose.yml a partir del archivo /path/to/project/docker-compose.yml.template

```
$ cp docker-compose.yml.template docker-compose.yml
```

5. Ajustar los valores a las rutas correctas en el sistema local de archivos, de los valúmenes en el archivo recien copiado docker-compose.yml

6. Copiar los archivos plantilla, de configuración del servidor web apache (que corre dentro del contenedor crm) desde /path/to/project/docs/apache2/ a la carpeta /path/to/project/apache2/sites-available/, si la carpeta sites-available no existe, se debe crear. Los archivos que se almacenan en sites-available deben tener la extensión .conf.

```
$ cp /path/to/project/docs/apache2/000-default.conf.template /path/to/project/docs/apache2/sites-available/000-default.conf
$ cp /path/to/project/docs/apache2/default-ssl.conf.template /path/to/project/docs/apache2/sites-available/default-ssl.conf
```

7. Arrancar los contenedores:

```
$ docker compose up -d &
```

8. Instalar las dependencias de laravel, para esto se debe ingresar al contenedor "crm"

```
$ docker exec -it crm /bin/bash
```
Una vez dentro del contenedor, ejecutar la instalación de las dependencias:

```
$ cd /var/www/app/
$ composer install
```

9.  Crear el archivo .env a paritr del archivo .env.example, este archivo es necesario para la operación de Laravel. Estando dentro del contenedor "crm", ejecutar el siguiente comando:

```
$ cp /var/www/app/.env.example /var/www/app/.env
``` 

Una vez copiado el archivo de variables de entorno del framework, ajustar los valores de acuerdo a la instalación local.

10. Asegurarse que la carpeta de almacenamiento de Laravel (/path/to/project/storage) tenga los permisos adecuados de escritura.

```
$ chmod -Rf 777 /var/www/app/storage/
``` 

11. Generar un API KEY del proyecto, como el comando: php artisan key:generate

```
$ cd /var/www/app/
$ php artisan key:generate
```

12. Comprobar que el proyecto se ejecuta correctamente apuntando el navegador web a la URL: http://localhost:8300 (ajustar el valor del puerto, de acuerdo a la configuración del contenedor "crm" en el archivo docker-compose.yml)


FIN.

## Post Instalación.

Este proyecto se pretende como una plantilla de instalación de Laravel para proyectos basados en este stack/framework, por lo que el repositorio git final del mismo no debe ser este repositorio.

Por lo tanto, luego de finalizada la instalación se debe eliminar el repositorio git local y generar un nuevo repositorio git, con una posible vinculación a un repositorio remoto nuevo, así:

- Eliminar la carpeta .git/ de la raiz del proyecto. 

```
$ cd /path/to/project/
$ rm -Rf .git/
```

- Crear un nuevo repositorio git para el nuevo proyecto.

```
$ cd /path/to/project/
$ git init .
$ git branch -m "main"
```  

