# Certificado SSL en Nginx
[Nginx][urlNginxSsl] permite la configuración de un [certificado SSL][urlTutoSsl]. 

Hay un [tutorial][urlTutoDigicert] que explica la [configuración del certificado][urlTutoDigicert] 
y [aquí][urlTutoConfigSsl] hay [otro tutorial][urlTutoConfigSsl] que lo complementa.

Las [indicaciones para hacer la redirección][urlRedirectSSL] hacia el protocolo seguro 
están [aquí][urlRedirectSSL].

## 1. Metadatos

### Autor
@newdigicash
### Versión
0.1

## 2. Observación
En caso de falla al arrancar Nginx luego de la configuración, hay que revisar 
la causa en el [log][urlTutoErrorLog] *cat /var/log/nginx/error.log*

## 3. Contenido

### 3.1 Pre requisitos
Hay que [obtener un certificado SSL][urlTutoSsl] desde una entidad certificadora. 
Los archivos necesarios son:
+ certificado primario de dominio. Ejemplo: *certificate.crt*
+ certificado chain (root + intermediate) de la CA. Ejemplo: *ca_bundle.crt*
+ llave privada. Ejemplo: *private.key*

### 3.2 Instalación y configuración
Para configurar el certificado en dominios y subdominios, las 
indicaciones son las mismas. La diferencia está en el [tipo de certificado][urlTutoSsl] 
*wildcard* que soporta subdominios.

#### 3.2.1 Ubuntu

**Paso 1**. Combinar el certificado de dominio y el chain  emitido por la CA.

~~~
sudo cat certificate.crt ca_bundle.crt >> bundle.crt
~~~

**Paso 2**. Buscar el archivo de configuración en los sitios disponibles. 
El archivo predeterminado es _default_. En caso de subdominios o dominios 
será *dominio.com* o similares.

~~~
sudo ls -la /etc/nginx/sites-available/
sudo vim /etc/nginx/sites-available/default
~~~

**Paso 3**. Copiar el fragmento de server y pegar a continuación, 
en el mismo archivo *default* o el que corresponda. De esta forma 
el primer server atiende el 80 y el segundo server el 443.

~~~
server {
...
}
~~~

**Paso 4**. Revisar y agregar la configuración del segundo server 
en *default* para que atienda el *443*

~~~
listen 443 ssl;
ssl on;
ssl_certificate		/ruta/bundle.crt
ssl_certificate_key	/ruta/private.key
server_name 	dominio.com;
~~~

**Paso 5**. Reiniciar nginx

~~~
sudo service nginx restart
~~~

### 3.3 Post instalación

#### 3.3.1 Habilitar el puerto 443
[Habilitar el puerto 443 en el firewall][urlTutoFw] y/o router del server

#### 3.3.2 Redirección a protocolo seguro

**Paso 1**. Buscar el archivo de configuración en los sitios disponibles.
~~~
sudo ls -la /etc/nginx/sites-available/
sudo vim /etc/nginx/sites-available/default
~~~

**Paso 2**. Revisar y agregar la [configuración del server 80][urlRedirectSSL] 
correspondiente para hacer la redirección permanente. 
El archivo predeterminado es *default*.
~~~
listen 80;
server_name dominio.com www.dominio.com;
return 301 https://$host$request_uri;
~~~

Hay [otras formas de configurar la redirección][urlNginxSsl] en el server block.

## 4. Fuentes
Configuración SSL <http://nginx.org/en/docs/http/configuring_https_servers.html>

Tutorial de configuración <https://www.digicert.com/es/instalar-certificado-ssl-nginx.htm>

[//]: # (referencias citadas)
[urlNginxSsl]: http://nginx.org/en/docs/http/configuring_https_servers.html
[urlTutoSsl]: https://github.com/newdigicash/apuntes/blob/master/security/certificado-ssl.md
[urlTutoDigicert]: https://www.digicert.com/es/instalar-certificado-ssl-nginx.htm
[urlTutoErrorLog]: https://www.keycdn.com/support/nginx-error-log
[urlTutoConfigSsl]: https://softwarecrafters.io/devops/configurar-servidor-https-nginx
[urlRedirectSSL]: https://linuxize.com/post/redirect-http-to-https-in-nginx
[urlTutoFw]: https://github.com/newdigicash/apuntes/blob/master/linux/firewall-linux.md
