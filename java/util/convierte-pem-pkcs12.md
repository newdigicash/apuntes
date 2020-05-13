# Keystore PKCS12 desde CRT/PEM
El [comando openssl][urlParamP12] permite crear una keystore a partir 
de los certificados emitidos por una CA.

Hay un [tutorial][urlPem2Jks] que indica cómo [convertir de PEM a JKS][urlPem2Jks].
## 1. Metadatos

### Autor
@newdigicash
### Versión
0.1

## 2. Observación

[Tomcat][urlTomcatSsl] ofrece soporte para PKCS12 para [configurar SSL][urlTomcatSsl] 
y [Jetty][urlJettySsl] también.

## 3. Contenido 

### 3.1 Pre requisitos
Hay que [obtener un certificado SSL][urlNotasSsl] desde una CA. 
Los archivos necesarios son:
+ certificado primario de dominio. Ejemplo: *certificate.crt*
+ certificado chain (root + intermediate) de la CA. Ejemplo: *ca_bundle.crt*
+ llave privada. Ejemplo: *private.key* 

### 3.2 Generar una keystore en formato PKCS12

**Paso 1**. Consolidar los certificados individuales en un [archivo pem][urlPem]. 
~~~
sudo cat certificate.crt ca_bundle.crt >> consolidado.pem
~~~
También se puede crear un archivo consolidado manualmente. 
El archivo resultante debe respetar el [orden del formato pem][urlPem]: 
*certificado primario*, *certificado intermediate* y *certificado root*.

**Paso 2**. Generar una keystore en PKCS#12. El parámetro _-passin_ es necesario 
solamente si la llave privada está encriptada. 
~~~
openssl pkcs12 -export -inkey private.key -in consolidado.pem -out mikeystore.p12 -name dominio.com -passin 'pass:passphraseKey' -passout 'pass:passphraseP12'
~~~

**Paso 3**. Comprobar el archivo generado usando la passphrase ingresada antes. 
Debe mostrar tipo _PKCS12_, 1 entrada y el fingerprint.
~~~
sudo keytool -list -keystore mikeystore.p12 -storepass mipassphrase
~~~

**Paso 4**. Importar de PKCS12 a JKS con [keytool][urlParamKeytool]. 
Si la passphrase de origen *-deststorepass*  y destino *-srcstorepass* 
son diferentes, es necesario agregar el [parámetro][urlParamKeytool] *-destkeypass* 
con la passphrase nueva.
~~~
keytool -importkeystore -srckeystore mikeystore.p12 -srcstoretype PKCS12 -srcstorepass passphraseP12 -deststorepass passphraseJKS -alias dominio.com -deststoretype PKCS12 -destkeystore mikeystore.jks
~~~

El comando [keytool muestra un mensaje de advertencia al importar][urlAdvertenciaJks] 
indicando que jks es un formato propietario y recomienda migrar a PKCS12. 
Para prevenir esta advertencia al importar, el parámetro *-deststoretype PKCS12* 
pemite indicar el formato que tendrá el contenido de la keystore sin importar 
la extensión _.p12_ o _.jks_.

~~~
sudo keytool -list -keystore mikeystore.jks -storepass passphraseJKS
~~~

## 4. Fuentes
Doc sobre PKCS#12 <https://en.wikipedia.org/wiki/PKCS_12>

Parámetros de PKCS#12 <https://www.openssl.org/docs/man1.0.2/man1/pkcs12.html>

Parámetros de keytool <https://docs.oracle.com/javase/7/docs/technotes/tools/solaris/keytool.html>

Formato de PEM <https://www.digicert.com/kb/ssl-support/pem-ssl-creation.htm>

Tutorial de PEM a JKS <https://docs.cloudera.com/documentation/enterprise/5-10-x/topics/cm_sg_openssl_jks.html>

[//]: # (referencias citadas)
[urlParamP12]: https://www.openssl.org/docs/man1.0.2/man1/pkcs12.html
[urlPem]: https://www.digicert.com/kb/ssl-support/pem-ssl-creation.htm
[urlNotasSsl]: https://github.com/newdigicash/apuntes/blob/master/security/certificado-ssl.md
[urlPem2Jks]: https://docs.cloudera.com/documentation/enterprise/5-10-x/topics/cm_sg_openssl_jks.html
[urlParamKeytool]: https://docs.oracle.com/javase/7/docs/technotes/tools/solaris/keytool.html
[urlAdvertenciaJks]: https://support.oracle.com/knowledge/Middleware/2376435_1.html
[urlTomcatSsl]: http://tomcat.apache.org/tomcat-9.0-doc/ssl-howto.html
[urlJettySsl]: https://www.eclipse.org/jetty/documentation/current/configuring-ssl.html#loading-keys-and-certificates-via-pkcks12
