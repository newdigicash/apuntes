# Generar un CSR con Keytool
El comando [keytool][urlKeytool], de Java, administra las llaves secretas. 
El uso más común es para generar un [CSR][urlCSR].

Antes de generar el [CSR][urlCSR], es necesario crear un keystore con [keytool][urlKeytool]. 
En este [tutorial][urlDigicert] se indican los pasos para [generar el certificado][urlDigicert]. 

Aquí hay [otro tutorial][urlGlobalSign] breve para [generar un CSR][urlGlobalSign].

## 1. Metadatos

### Autor
@newdigicash
### Versión
0.6

## 2. Observaciones
+ En la [web de Digicert][urlDigicert] hay un [wizard][urlDigicert] para armar los parámetros de [keytool][urlKeytool].
+ A partir de Java 8 el parámetro es *-genkeypair* ya que *genkey* es la versión antigua.
+ Es necesario especificar la validez del keystore y del csr, ya que por defecto dura 90 días.

## 3. Contenido 
Para [generar un CSR][urlDigicert], con la [keytool][urlKeytool] hay que seguir estos pasos.

**Paso 1**. Generar el keystore llamado _mikey.jks_ para _dominio.com_ válido por 365 días. 
En name/lastname, para single-domain ingresar _www\.dominio\.com_ o 
para wildcard escribir _\*.dominio\.com_. El keystore almacena una llave 
pública y una llave privada.

~~~
sudo keytool -genkeypair -alias dominio.com -keyalg RSA -validity 365 -keystore mikey.jks -keysize 2048
~~~

**Paso 2**. Generar el CSR llamado _dominio.com.csr_ válido por 365 días. Delimitado por *CERTIFICATE REQUEST*.

~~~
sudo keytool -certreq -alias dominio.com -validity 365 -keystore mikey.jks -file dominio.com.csr
~~~

Para obtener la llave privada hay que continuar con estos pasos.

**Paso 3**. [Importar a PKCS#12 desde JKS][urlExportarCert], esto crea _mikey.p12_ 
con passphrase igual al _mikey.jks_
~~~
sudo keytool -importkeystore -srckeystore mikey.jks -destkeystore mikey.p12 -srcalias dominio.com -srcstoretype jks -deststoretype pkcs12
~~~

**Paso 4**. Comprobar el archivo generado usando la passphrase ingresada antes. 
Debe mostrar tipo _PKCS12_ y el fingerprint.
~~~
sudo keytool -list -v -keystore mikey.p12 -storepass mipassphrase
~~~

**Paso 5**. [Exportar la llave privada][urlTutoExportKey], esto crea _private.key_ 
desde el keystore [PKCS#12][urlP12Params] sin encriptación (y sin contraseña). 
Delimitada por *PRIVATE KEY* o *RSA PRIVATE KEY*.

Este paso necesita [Openssl][urlOpenssl] y [algunos parámetros][urlP12Params]. 

En _unix_ hay que ingresar la passphrase al momento de ejecutar el comando. 
El parámetro _-nodes_ exporta la llave privada sin encriptación

~~~
sudo openssl pkcs12 -in mikey.p12 -out private.key -nocerts -nodes
~~~

En _windows_ hay que indicar la passphrase como parámetro, luego ejecutar el comando.
~~~
openssl pkcs12 -in mikey.p12 -out private.key -nocerts -nodes -passin 'pass:mipassphrase'
~~~

Al final se obtiene 2 keystores _mikey.jks_ y _mikey.p12_, el CSR _dominio.com.csr_ 
para enviar a la CA, la _private.key_, la passphrase de al menos 6 caracteres, 
y el alias que puede ser _dominio.com_.

**Paso 6. Opcional**. En caso de querer exportar la llave con encriptación, 
hay que agregar el parámetro _-passout_ e indicar la contraseña de la llave exportada, 
y quitar _-nodes_. Como se muestra a continuación.
~~~
openssl pkcs12 -in mikey.p12 -out private.key -nocerts -passin 'pass:mipassphrase' -passout 'pass:otrapassphrase'
~~~

Esto crea la llave *private.key* delimitada por *ENCRYPTED PRIVATE KEY*.

Para convertir la llave privada encriptada con AES, ejecutar el comando 
e indicar la passphrase.
~~~
sudo openssl rsa -in private.key -aes256 -out privateAES.pem
~~~

Esto crea la llave *privateAES.pem* delimitada por *RSA PRIVATE KEY*

## 4. Fuentes
Doc oficial sobre keytool <https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html>

Doc sobre CSR <https://www.dondominio.com/help/es/242/que-es-csr/>

Doc sobre PKCS#12 <https://en.wikipedia.org/wiki/PKCS_12>

Parámetros de PKCS#12 <https://www.openssl.org/docs/man1.0.2/man1/pkcs12.html>

Tutorial para exportar la llave <https://dzone.com/articles/extracting-a-private-key-from-java-keystore-jks>

[//]: # (referencias citadas)
[urlKeytool]: https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html
[urlCSR]: https://www.dondominio.com/help/es/242/que-es-csr/
[urlDigicert]: https://www.digicert.com/kb/csr-ssl-installation/tomcat-keytool.htm
[urlGlobalSign]: https://support.globalsign.com/digital-certificates/digital-certificate-installation/java-keytool-generate-csr
[urlExportarCert]: https://www.calazan.com/how-to-convert-a-java-keystore-jks-to-pem-format
[urlOpenssl]: https://www.openssl.org
[urlP12Params]: https://www.openssl.org/docs/man1.0.2/man1/pkcs12.html
[urlTutoExportKey]: https://dzone.com/articles/extracting-a-private-key-from-java-keystore-jks
