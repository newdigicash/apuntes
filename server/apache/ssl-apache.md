# Certificado SSL en Apache
[Apache][urlApacheSsl] permite la configuración de un [certificado SSL][urlTutoSsl]. 

Hay un [tutorial][urlTutoDigicert] que explica la [configuración del certificado][urlTutoDigicert] 
y [aquí][urlOtroDigicert] hay [otro tutorial][urlOtroDigicert] que lo complementa.

Las [indicaciones para hacer la redirección][urlRedirectSSL] hacia el protocolo seguro 
están [aquí][urlRedirectSSL].

## 1. Metadatos

### Autor
@newdigicash
### Versión
0.4

## 2. Observación
En caso de falla al arrancar Apache luego de la configuración, hay que revisar 
la causa en el log *cat /var/log/apache2/error.log*

## 3. Contenido

### 3.1 Pre requisitos
Hay que [obtener un certificado SSL][urlTutoSsl] desde una entidad certificadora. 
Los archivos necesarios son:
+ certificado primario de dominio. Ejemplo: *dominio.crt*
+ certificado chain (root + intermediate) de la CA. Ejemplo: *bundle.pem*
+ llave privada. Ejemplo: *private.key*

### 3.2 Instalación y configuración
Para configurar el certificado en dominios y subdominios, las 
indicaciones son las mismas. La diferencia está en el [tipo de certificado][urlTutoSsl] 
*wildcard* que soporta subdominios.

#### 3.2.1 Ubuntu

**Paso 1**. Buscar el archivo de configuración en los sitios disponibles. 
El archivo predeterminado es _default-ssl.conf_. En caso de subdominios o dominios 
será *dominio.com-ssl.conf* o similares.

~~~
sudo ls -la /etc/apache2/sites-available/
sudo vim /etc/apache2/sites-available/default-ss.conf
~~~
**Paso 2**. Revisar y agregar la configuración del Virtual Host 
de *default-ssl.conf*. El puerto debe ser *443*.

~~~
SSLEngine on
SSLCertificateFile /ruta/dominio.crt
SSLCertificateKeyFile /ruta/private.key
SSLCertificateChainFile /ruta/bundle.pem
~~~

**Paso 3**. Activar el módulo SSL y habilitar la configuración 
_default-ssl_ o el que corresponda.
~~~
sudo a2enmod ssl
sudo a2ensite default-ssl
~~~

**Paso 4**. Probar la configuración y reiniciar apache

~~~
sudo apachectl configtest
sudo service apache2 restart
~~~

### 3.3 Post instalación

#### 3.3.1 Habilitar el puerto 443
[Habilitar el puerto 443 en el firewall][urlTutoFw] y/o router del server

#### 3.3.2 Redirección a protocolo seguro

**Paso 1**. Buscar el archivo de configuración en los sitios disponibles.
~~~
sudo ls -la /etc/apache2/sites-available/
sudo vim /etc/apache2/sites-available/000-default.conf
~~~

**Paso 2**. Revisar y agregar la [configuración en el Virtual Host][urlRedirectSSL] 
correspondiente. El archivo predeterminado es *000-default.conf*.
~~~
Redirect / https://dominio.com/
~~~

Hay [otras formas de configurar la redirección en el Virtual Host][urlRedirectSSL] y 
con [mod_rewrite][urlRedirectOficial].

### 3.4 Redirección alterna usando htaccess

Redireccinar cuando no sea protocolo seguro o cuando el dominio empiece con *www*.

~~~
RewriteEngine On
RewriteCond %{HTTPS} off [OR]
RewriteCond %{HTTP_HOST} ^www\. [NC]
RewriteRule ^(.*)$ https://dominio.com/$1 [R=301,L]
~~~

Esta configuración se basa en este [tutorial][urlTutoRedirectHtacess]. Otros 
[ejemplos de configuración][urlRedirectOficial] están en la 
[documentación oficial][urlRedirectOficial].

## 4. Fuentes
Configuración SSL en Apache <https://httpd.apache.org/docs/trunk/es/ssl/ssl_howto.html>

Tutorial de configuración <https://www.digicert.com/kb/csr-ssl-installation/ubuntu-server-with-apache2-openssl.htm>

Comando a2ensite <http://manpages.ubuntu.com/manpages/bionic/man8/a2ensite.8.html>

Comando a2enmod <http://manpages.ubuntu.com/manpages/bionic/en/man8/a2enmod.8.html>

Redirección SSL <https://httpd.apache.org/docs/2.4/rewrite/remapping.html>

[//]: # (referencias citadas)
[urlApacheSsl]: https://httpd.apache.org/docs/trunk/es/ssl/ssl_howto.html
[urlTutoSsl]: https://github.com/newdigicash/apuntes/blob/master/security/certificado-ssl.md
[urlTutoDigicert]: https://www.digicert.com/kb/csr-ssl-installation/ubuntu-server-with-apache2-openssl.htm
[urlOtroDigicert]: https://www.digicert.com/kb/csr-ssl-installation/apache-openssl.htm
[urlRedirectSSL]: https://cwiki.apache.org/confluence/display/HTTPD/RedirectSSL
[urlRedirectOficial]: https://httpd.apache.org/docs/2.4/rewrite/remapping.html
[urlTutoFw]: https://github.com/newdigicash/apuntes/blob/master/linux/firewall-linux.md
[urlTutoRedirectHtacess]: https://simonecarletti.com/blog/2016/08/redirect-domain-http-https-www-apache
