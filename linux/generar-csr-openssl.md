# Generar un CSR con OpenSSL
El comando [openssl][urlOpenssl] sirva para [tareas criptográficas][urlRepoOpenssl]. 
Los usos más comunes son para generar llaves y certificados. 

Para [generar un CSR][urlDigicert], aquí hay un [tutorial][urlDigicert]. 
Para [generar la llave y luego un CSR][urlTutoGenSsl], aquí está otro [tutorial][urlTutoGenSsl].

## 1. Metadatos

### Autor
@newdigicash
### Versión
0.1

## 2. Observación

Para windows hay [binarios][urlBinWin] y están [aquí][urlBinWin]. 
En [Cygwin][urlCygwin] hay un [paquete para openssl][urlPckOpenssl].

## 3. Contenido 
### 3.1 Generar un certificado nuevo y una llave nueva
Para generar un CSR llamado _dominio\.com\.csr_ y llave nueva llamada *privada.key*. 
En *common name*, para single-domain ingresar *www\.dominio\.com*, y para wildcard 
escribir _\*.dominio\.com_.
~~~
sudo openssl req -new -newkey rsa:2048 -nodes -keyout privada.key -out dominio.com.csr
~~~

### 3.2 Generar un CSR en 2 pasos
Genera la llave *privada.key*
~~~
sudo openssl genrsa -out privada.key 2048
~~~

Genera el CSR  *dominio\.com\.csr*
~~~
openssl req -new -days 3650 -key privada.key -out dominio.com.csr
~~~
Si ya se tiene la llave, solo es necesario crear el certificado. 

## 4. Fuentes
Doc de OpenSSL <https://www.openssl.org/docs/manmaster/man1/openssl.html>

[//]: # (referencias citadas)
[urlRepoOpenssl]: https://github.com/openssl/openssl
[urlOpenssl]: https://www.openssl.org/docs/manmaster/man1/openssl.html
[urlTutoGenSsl]: https://www.ssl.com/how-to/manually-generate-a-certificate-signing-request-csr-using-openssl/
[urlDigicert]: https://www.digicert.com/es/creacion-de-sfc-apache.htm
[urlBinWin]: https://wiki.openssl.org/index.php/Binaries
[urlPckOpenssl]: https://cygwin.com/packages/summary/openssl-src.html
[urlCygwin]: https://www.cygwin.com
