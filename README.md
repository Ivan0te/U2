# Implantación de una web estática con Apache

!!! success "Objetivos de la práctica"

    - Instalar y configurar un servidor web Apache2.
    - Crear distintos **Host Virtuales** en apache2 que nos permiten tener sitios web diferenciados.
    - Acceder a cada **Host Virtual** con un determinado nombre de dominio.
  
## Instalación de Apache2

Para empezar, tendremos que empezar a realiar la instalacion de ***Apache2***. Para ello, pondremos los siguientes comandos:

```
sudo apt update && sudo apt upgrade -y
sudo apt install apache2 -y
```

## Habilitar módulo de host virtuales

Para habilitar esta caracteristica, pondremos el siguiente comando en la terminal:

```
sudo a2enmod vhost_alias
```

Generalmente, esto viene ya habilitado por defecto.

## Configurar Host Virtuales

Crearemos un directorio para cada sitio web: **iaw2425.com** y **docu-iaw2425.com**.

```
sudo mkdir -p /var/www/iaw2425.com/index.html
sudo mkdir -p /var/www/docu-iaw2425.com/index.html
```

Asignaremos permisos.

```
sudo chown -R $USER:$USER /var/www/iaw2425.com
sudo chown -R $USER:$USER /var/www/docu-iaw2425.com
```

Crear archivos configuración para hosts virtuales.

- Para iaw2425.com:
    
    ```
    sudo nano /etc/apache2/sites-available/iaw2425.com.conf
    ```

    Añadiremos el siguiente contenido:

    ```
    <VirtualHost *:80>
    ServerAdmin admin@iaw2425.com
    ServerName iaw2425.com
    ServerAlias www.iaw2425.com
    DocumentRoot /var/www/iaw2425.com/site/

    <Directory /var/www/iaw2425.com/site>
        AllowOverride All
    </Directory>

     ErrorLog ${APACHE_LOG_DIR}/iaw2425_error.log
    CustomLog ${APACHE_LOG_DIR}/iaw2425_access.log combined
    </VirtualHost>
    ```

- Para docu-iaw2425.com:

    ```
    sudo nano /etc/apache2/sites-available/docu-iaw2425.com.conf
    ```

    Añadiremos el siguiente contenido:

    ```
    <VirtualHost *:80>
    ServerAdmin admin@docu-iaw2425.com
    ServerName docu-iaw2425.com
    ServerAlias www.docu-iaw2425.com
    Redirect / http://docu-iaw2425.com:8081/
    </VirtualHost>

    <VirtualHost *:8081>
    ServerAdmin admin@docu-iaw2425.com
    ServerName docu-iaw2425.com
    ServerAlias www.docu-iaw2425.com
    DocumentRoot /var/www/docu-iaw2425.com/site/ 

    <Directory /var/www/docu-iaw2425.com/site>
        AllowOverride All
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/docu-iaw2425_error.log
    CustomLog ${APACHE_LOG_DIR}/docu-iaw2425_access.log combined
    </VirtualHost>
    ```

## Habilitar los Host Virtuales

```
sudo a2ensite iaw2425.com.conf
sudo a2ensite docu-iaw2425.com.conf
```

## Configurar archivo ***/etc/ports.conf***

Editaremos el siguiente fichero:

```
sudo nano /etc/apache2/ports.conf
```

Y nos aseguraremos de que contengan estas lineas, si no las tiene, las agregaremos:

```
Listen 80
Listen 8081
```

## Reiniciaremos Apache

Reiniciaremos el servidor apache con el siguiente comando:

```
sudo systemctl restart apache2
```

## Configuracion de hosts

Para poder acceder a los sitios con los nombres de dominios, tendremos que editar el siguiente fichero:

```
sudo nano /etc/hosts
```

Y añadiremos las siguientes lineas:

```
127.0.0.1   iaw2425.com
127.0.0.1   www.iaw2425.com
127.0.0.1   docu-iaw2425.com
127.0.0.1   www.docu-iaw2425.com
```

## Verificación de paginas

IAW2425.COM

![iaw2425.com](../images/iaw2425.png)

DOCU-IAW2425.COM

![docu-iaw2425.com](../images/docu-iaw2425.png)
