# Generar un certificado autofirmado con Keytool
El comando [keytool][urlKeytool], de Java, administra las llaves secretas. 
Los usos más comunes son para generar un [CSR][urlCSR] y los pares de llaves pública \/ 
privada.

Un [certificado autofirmado (self-signed certificate)][urlSelfSignedCert] sirve configurar 
SSL para desarrollo y pruebas locales.

Hay un [tutorial][urlTutoGenCert] que indica los pasos para [generar el certificado autofirmado][urlTutoGenCert].

En la [documentación oficial][urlKeytool] hay detalles de los [parámetros de keytool][urlKeytool].

## 1. Metadatos

### Autor
@newdigicash
### Versión
0.1

## 2. Observación
Un [certificado self-signed][urlSelfSignedCert] no está firmado por una CA y debe usarse 
solo para development.

## 3. Contenido 
Para [generar el certificado autofirmado][urlTutoGenCert] hay que seguir estos pasos.

**Paso 1**. Generar el keystore llamado _miselfsigned.jks_ para _localserver_. 
En name/lastname hay que ingresar _localserver_ El keystore almacena una llave 
pública y una llave privada.

~~~
sudo keytool -genkeypair -alias localserver -keyalg RSA -sigalg SHA256withRSA -validity 3650 -keystore miselfsigned.jks -keysize 2048
~~~

**Paso 2**. [Importar a PKCS#12 desde JKS][urlExportarCert], esto crea _miselfsigned.p12_ 
con passphrase igual al _miselfsigned.jks_. Con el parámetro *-deststoretype pkcs12*, la 
extensión _.p12_ o _.jks_ del archivo final es irrelevante.
~~~
sudo keytool -importkeystore -srckeystore miselfsigned.jks -destkeystore miselfsigned.p12 -srcalias localserver -srcstoretype jks -deststoretype pkcs12
~~~

**Paso 3**. Comprobar el archivo generado usando la passphrase ingresada antes. 
Debe mostrar tipo _PKCS12_ y el fingerprint.
~~~
sudo keytool -list -v -keystore miselfsigned.p12 -storepass mipassphrase
~~~

Para obtener la llave privada hay que continuar con estos pasos.

**Paso 4**. [Exportar la llave privada][urlTutoExportKey], esto crea _miprivate.key_ 
desde el keystore [PKCS#12][urlP12Params] sin encriptación (y sin contraseña). 
Delimitada por *PRIVATE KEY* o *RSA PRIVATE KEY*.

Este paso necesita [Openssl][urlOpenssl] y [algunos parámetros][urlP12Params]. 

En _unix_ hay que ingresar la passphrase al momento de ejecutar el comando. 
El parámetro _-nodes_ exporta la llave privada sin encriptación

~~~
sudo openssl pkcs12 -in miselfsigned.p12 -out miprivate.key -nocerts -nodes
~~~

En _windows_ hay que indicar la passphrase como parámetro, luego ejecutar el comando.
~~~
openssl pkcs12 -in miselfsigned.p12 -out miprivate.key -nocerts -nodes -passin 'pass:mipassphrase'
openssl rsa -in miprivate.key -out private.pem
~~~

Al final se obtiene 2 keystores _miselfsigned.jks_ y _miselfsigned.p12_, y la _miprivate.key_.

**Paso 5. Opcional**. En caso de querer exportar la llave con encriptación, 
hay que agregar el parámetro _-passout_ e indicar la contraseña de la llave exportada, 
y quitar _-nodes_. Como se muestra a continuación.
~~~
openssl pkcs12 -in miselfsigned.p12 -out miprivate.key -nocerts -passin 'pass:mipassphrase' -passout 'pass:otrapassphrase'
~~~

Esto crea la llave *miprivate.key* delimitada por *ENCRYPTED PRIVATE KEY*.

## 4. Fuentes
Wiki de certificado autofirmado <https://en.wikipedia.org/wiki/Self-signed_certificate>

Doc oficial sobre keytool <https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html>

Doc sobre CSR <https://www.dondominio.com/help/es/242/que-es-csr/>

OpenSSL <https://www.openssl.org>

Parámetros de PKCS#12 <https://www.openssl.org/docs/man1.0.2/man1/pkcs12.html>

[//]: # (referencias citadas)
[urlSelfSignedCert]: https://en.wikipedia.org/wiki/Self-signed_certificate
[urlKeytool]: https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html
[urlCSR]: https://www.dondominio.com/help/es/242/que-es-csr
[urlTutoGenCert]: https://community.pivotal.io/s/article/Generating-a-self-signed-SSL-certificate-using-the-Java-keytool-command?language=en_US
[urlExportarCert]: https://www.calazan.com/how-to-convert-a-java-keystore-jks-to-pem-format
[urlOpenssl]: https://www.openssl.org
[urlP12Params]: https://www.openssl.org/docs/man1.0.2/man1/pkcs12.html
[urlTutoExportKey]: https://dzone.com/articles/extracting-a-private-key-from-java-keystore-jks
