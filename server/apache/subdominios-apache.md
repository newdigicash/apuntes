# Subdominios en Apache 2.4
El [servidor Apache][urlApache] permite configurar [Virtual Hosts][urlVhost] 
para alojar varios sitios web en un mismo servidor. 

El principio de  [Virtual Host][urlVhost] basado en nombre es servir varias 
webs con una sola IP. En la [página de apache][urlApacheEjemplos] hay 
[ejemplos de configuración][urlApacheEjemplos].

## 1. Metadatos

### Autor
@newdigicash
### Versión
0.1

## 2. Observación
La advertencia por la falta de nombre de dominio predeterminado se corrige en 
el archivo de configuración de apache.

En ubuntu, en */etc/apache2/apache2.conf* hay que agregar *ServerName localhost*.

## 3. Contenido 

Pasos para [crer un subdominio][urlTutoSubdominio]. 
Para [crear varios dominios][urlTutoDominios] el procedimiento similar.

### 3.1 Ubuntu

**Paso 1**. Crear el directorio raíz y asignar carpeta al _usuario_

~~~
sudo mkdir -p /var/www/subdominio.dominio.com/public
sudo chown -R usuario /var/www/subdominio.dominio.com/public
sudo vim /var/www/subdominio.dominio.com/public/index.html
~~~

**Paso 2**. Configurar el virtual host
~~~
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/subdominio.dominio.com.conf
sudo vim /etc/apache2/sites-available/subdominio.dominio.com.conf
~~~
Revisar y modificar el archivo _subdominio.dominio.com.conf_
~~~
DocumentRoot /var/www/subdominio.dominio.com/public
ServerName subdominio.dominio.com
ServerAlias www.subdominio.dominio.com
~~~

**Paso 3**. Habilitar el virtual host con [a2ensite][urlCmdA2ensite].

~~~
sudo a2ensite subdominio.dominio.com.conf
~~~

**Paso 4**. Probar la configuración y reiniciar apache

~~~
sudo apachectl configtest
sudo service apache2 restart
~~~

## 4. Fuentes
Sitio oficial de Apache <https://httpd.apache.org/>

Doc oficial de VHost <https://httpd.apache.org/docs/2.4/vhosts/>

Ejemplos de configuración <https://httpd.apache.org/docs/2.4/vhosts/examples.html>

[//]: # (referencias citadas)
[urlVhost]: https://httpd.apache.org/docs/2.4/vhosts
[urlApache]: https://httpd.apache.org/
[urlApacheEjemplos]: https://httpd.apache.org/docs/2.4/vhosts/examples.html
[urlTutoSubdominio]: https://ekiketa.es/crear-tantos-subdominios-como-queramos-en-nuestro-linux-ubuntu
[urlTutoDominios]: https://www.digitalocean.com/community/tutorials/como-configurar-virtual-hosts-de-apache-en-ubuntu-16-04-es
[urlCmdA2ensite]: https://www.systutorials.com/docs/linux/man/8-a2ensite
