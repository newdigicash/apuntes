# Generar un CSR con Keytool
El comando [keytool][urlKeytool], de Java, administra las llaves secretas. 
El uso más común es para generar un [CSR][urlCSR].

Antes de generar el [CSR][urlCSR], es necesario crear una keystore con [keytool][urlKeytool]. 
En este [tutorial][urlDigicert] se indican los pasos para [generar el certificado][urlDigicert]. 

Aquí hay [otro tutorial][urlGlobalSign] breve para [generar un CSR][urlGlobalSign].

## 1. Metadatos

### Autor
@newdigicash
### Versión
0.1

## 2. Observación

En la [web de Digicert][urlDigicert] hay un [wizard][urlDigicert] para armar los parámetros de [keytool][urlKeytool].

## 3. Contenido 
Para [generar un CSR][urlDigicert], con la [keytool][urlKeytool] hay que seguir estos pasos.

**Paso 1**. Generar la keystore llamada _mikey.jks_ para _dominio.com_. 
En name/lastname, para single-domain ingresar _www\.dominio\.com_ o 
para wildcard escribir _\*.dominio\.com_.
~~~
sudo keytool -genkey -alias dominio.com -keyalg RSA -keystore mikey.jks -keysize 2048
~~~

**Paso 2**. Generar el CSR llamado _dominio.com.csr_
~~~
sudo keytool -certreq -alias dominio.com -keystore mikey.jks -file dominio.com.csr
~~~

Al final se obtiene _mikey.jks_, _dominio.com.csr_ que es la llave privada, 
la passphrase de al menos 6 caracteres, y el alias _dominio.com_.

## 4. Fuentes
Doc oficial <https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html>

Doc sobre CSR <https://www.dondominio.com/help/es/242/que-es-csr/>

[//]: # (referencias citadas)
[urlKeytool]: https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html
[urlCSR]: https://www.dondominio.com/help/es/242/que-es-csr/
[urlDigicert]: https://www.digicert.com/kb/csr-ssl-installation/tomcat-keytool.htm
[urlGlobalSign]: https://support.globalsign.com/digital-certificates/digital-certificate-installation/java-keytool-generate-csr
