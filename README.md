# Despliegue de una aplicación Python con Flask y Gunicorn

## Instalar requisitos previos

![Instalar nginx/gunicorn/pipenv](capturas/01_instalar_nginx_gunicorn_pipenv.png)


## Comandos principales (paso a paso)

1. Actualizar el sistema e instalar pip para Python 3:

![Instalar python3-pip](capturas/02_instalar_python3-pip.png)

2. Instalar pipenv:

![Instalar pipenv](capturas/03_instalar_pipenv.png)

![Comprobar versión pipenv](capturas/04_comprobar_version_pipenv.png)

3. Instalar `python-dotenv` para cargar variables desde `.env`:

![Instalar python-dotenv](capturas/05_instalar_python-dotenv.png)

4. Crear la carpeta de la aplicación y ajustar permisos:

![Crear carpeta app y cambiar permisos](capturas/06_crear_carpeta_app_y_cambiar_permisos.png)

5. Crear el fichero de variables de entorno en `/var/www/app/.env` con el contenido:

![Crear archivo .env](capturas/07_crear_archivo_env.png)

6. Iniciar el entorno virtual con pipenv:

![Ejecutar pipenv shell](capturas/08_ejecutar_pipenv_shell.png)

7. Instalar dependencias dentro del entorno pipenv:

![Instalar flask y gunicorn](capturas/09_instalar_flask_gunicorn_con_pipenv.png)

## Archivos de la aplicación

Crea los ficheros `application.py` y `wsgi.py` en `/var/www/app`.

![Crear archivos application y wsgi](capturas/10_crear_archiuvos_application_wsgi.png)

## Pruebas locales

1. Probar con el servidor de desarrollo de Flask (solo para comprobación local):

![Ejecutar flask run](capturas/11_ejecutar_flask_run.png)

Accede desde el anfitrión a `http://72.62.239.208:5000` y se debería ver:

![Comprobar app desplegada en navegador](capturas/12_comprobar_app_desplegada_en_navegador.png)

2. Probar con Gunicorn (ya dentro del entorno virtual):

![Ejecutar gunicorn](capturas/13_ejecutar_gunicorn.png)

3. Anotar la ruta del binario de gunicorn (necesaria para el servicio systemd):

![Comprobar ruta gunicorn](capturas/14_comprobar_ruta_gunicorn.png)

## systemd: crear servicio para Gunicorn

Crea el archivo `/etc/systemd/system/flask_app.service` con el siguiente contenido (ajusta rutas y usuario a tu entorno):

![Crear archivo de servicio flask\_app](capturas/16_crear_archivo_de_servicio_flask_app.png)

Luego recarga systemd, habilita e inicia el servicio:

![Arrancar servicio flask\_app](capturas/17_arrancar_servicio_flask_app.png)

## Configuración de Nginx

Crea el fichero `/etc/nginx/sites-available/app.conf` con:

![Crear archivo nginx app.conf](capturas/18_crear_archivo_nginx_app_conf.png)

Activa el sitio creando un enlace simbólico y comprueba la sintaxis de Nginx:

![Crear link app.conf](capturas/19_crear_link_app_conf.png)

![Arrancar servicio nginx (final)](capturas/20_arrancar_servicio_nginx.png)

## Hosts del anfitrión

Antes de acceder por `app.izv` desde tu máquina anfitriona, añade la IP de la VM al `/etc/hosts` del anfitrión:

```
72.62.239.208 app.izv www.app.izv
```

## Comprobación final

Accede desde tu navegador del anfitrión a `http://app.izv/` o `http://www.app.izv/`. Se debería ver nuevamente:

![Comprobar en navegador por server\_name](capturas/21_comprobar_en_navegador.png)
