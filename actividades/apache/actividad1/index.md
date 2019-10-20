# Apache actividad 1

## Sitio 1

Instalar servidor PHP:
```
sudo apt install php7.2
```
Habilitar módulo apache:
```
a2dismod php7.2
```

Creamos el directorio dónde estará el sitio php.
```
sudo mkdir /var/www/sitioPhp
```

Añadimos `Listen 82` a /etc/apache2/ports.conf

<br>


Creamos el fichero /etc/apache2/sites-available/sitio-php.conf 
```
<VirtualHost *:82>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/sitioPhp

        ErrorDocument 404 "Página no encontrada"

        ErrorLog /etc/logs/sitioPhp.log

        LogFormat "%t %h %m %>s" PhpLogFormat
        CustomLog /etc/logs/sitioPhpCustom.log PhpLogFormat
</VirtualHost>
```
