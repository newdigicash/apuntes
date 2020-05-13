# .Certificado SSL en Tomcat
[Tomcat][urlTomcatSsl] permite la configuración de un [certificado SSL][urlTutoSsl]. 

En la [documentación oficial][urlTomcatSsl] hay indicaciones para preparar los certificados y 
hacer la configuración de la keystore [PKCS12 o JKS][urlTomcatSsl]. 
Tomcat usa un [_Connector_][urlTomcatConf] para configurar y tiene [varios parámetros][urlTomcatConf].

Hay un [tutorial][urlTutoSsltomcat] que explica la [configuración del certificado][urlTutoSsltomcat].

## 1. Metadatos

### Autor
@newdigicash
### Versión
0.1

## 2. Observación
En caso de falla al arrancar Tomcat luego de la configuración, hay que revisar 
la causa en el log. El log puede estar en *cat /opt/tomcat/latest/catalina.out* 
o similares.

Para [instalar Tomcat en Ubuntu][urlTutoInstalaTomcat] hay un [tutorial][urlTutoInstalaTomcat] 
y [otro tutorial aquí][urlTutoTomcatUbuntu].

## 3. Contenido 

### 3.1 Pre requisitos

Hay que [generar un keystore en formato PKCS12][urlPem2P12] a partir de 
los certificados emitidos por una CA. El archivo necesario para esta 
configuración es:
+ almacén de llaves. Ejemplo *mikeystore.p12*

### 3.2 Instalación y configuración

#### 3.2.1 Ubuntu

**Paso 1**. Buscar el archivo de configuración en la carpeta *conf*. 
El archivo predeterminado es *server.xml*.
~~~
sudo ls -l /opt/tomcat/latest/conf
sudo vim /opt/tomcat/latest/conf/server.xml
~~~

**Paso 2**. Revisar la configuración del _Connector_ predeterminado 
en _server.xml_. Es necesario que redireccione al puerto 
del nuevo conector.
~~~
<Conector port="80" protocol="HTTP/1.1" 
	connectionTimeout="20000" 
	redirectPort="443" />
~~~

**Paso 3**. Revisar y agregar la [configuración del nuevo _Connector_][urlTomcatConf] 
en el mismo archivo _server.xml_.
~~~
<Conector 
	protocol="org.apache.coyote.http11.Http11NioProtocol" 
	port="443" maxThreads="150" 
	scheme="https" secure="true" SSLEnabled="true" 
	clientAuth="false" >
	<SSLHostConfig hostName="dominio.com" >
		<Certificate 
			certificateKeystoreFile="ruta/mikeystore.p12" 
			certificateKeystorePassword="mipassphrase" 
			type="UNDEFINED" />
	</SSLHostConfig>
</Conector>
~~~

**Paso 4**. Reiniciar Tomcat
~~~
sudo systemctl restart tomcat
~~~

O manualmente
~~~
sudo /opt/tomcat/latest/bin/shutdown.sh
sudo /opt/tomcat/latest/bin/startup.sh
~~~

### 3.3 Post instalación

#### 3.3.1 Habilitar el puerto correspondiente
[Habilitar el puerto 443 en el firewall][urlTutoFw] y/o router del server 
o el puerto que corresponda.

#### 3.3.2 Redirección a protocolo seguro

**Paso 1**. Buscar el archivo de configuración en la carpeta *conf*. 
El archivo predeterminado es *web.xml*.
~~~
sudo ls -l /opt/tomcat/latest/conf
sudo vim /opt/tomcat/latest/conf/web.xml
~~~

**Paso 2**. Agregar la restricción de seguridad en *web.xml*. 
El [parámetro _url-pattern_][urlWebPattern] indica la url a ser protegida.
~~~
<security-constraint>
	<web-resource-collection>
		<web-resource-name>toda-la-app</web-resource-name>
		<url-pattern>/*</url-pattern>
	</web-resource-collection>
	<user-data-constraint>
		<transport-guarantee>CONFIDENTIAL</transport-guarantee>
	</user-data-constraint>
</security-constraint>
~~~

El [parámetro _transport-guarantee_][urlTransportGuarantee] establece 
el nivel de protección a los datos. [*CONFIDENTIAL*][urlTransportGuarantee] 
garantiza que los datos no cambian por eso usa SSL y el *redirectPort* 
del _Connector_.

Hay otras [formas de configurar la restricción de seguridad][urlEjemploSecurityConst], 
he [aquí otros ejemplos][urlEjemploSecurityConst].

## 4. Fuentes
Doc oficial Tomcat <http://tomcat.apache.org/tomcat-9.0-doc/ssl-howto.html>

Parámetros de configuración Tomcat <https://tomcat.apache.org/tomcat-9.0-doc/config/http.html>

Tutorial de instalación <https://www.digicert.com/es/instalar-certificado-ssl-tomcat.htm>

[//]: # (referencias citadas)
[urlTomcatSsl]: http://tomcat.apache.org/tomcat-9.0-doc/ssl-howto.html
[urlTomcatConf]: https://tomcat.apache.org/tomcat-9.0-doc/config/http.html
[urlTutoSsl]: https://github.com/newdigicash/apuntes/blob/master/security/certificado-ssl.md
[urlPem2P12]: https://github.com/newdigicash/apuntes/blob/master/java/util/convierte-pem-pkcs12.md
[urlTutoFw]: https://github.com/newdigicash/apuntes/blob/master/linux/firewall-linux.md
[urlTutoInstalaTomcat]: https://www.digitalocean.com/community/tutorials/install-tomcat-9-ubuntu-1804-es
[urlTutoTomcatUbuntu]: https://ubunlog.com/tomcat-9-instalacion-ubuntu-18-04/
[urlRedirectTomcat]: http://wiki.metawerx.net/wiki/ForcingSSLForSectionsOfYourWebsite
[urlTutoSsltomcat]: https://www.digicert.com/es/instalar-certificado-ssl-tomcat.htm
[urlTransportGuarantee]: http://wiki.metawerx.net/wiki/Web.xml.TransportGuarantee
[urlWebPattern]: https://docs.oracle.com/cd/E19798-01/821-1841/gjjcd/index.html
[urlEjemploSecurityConst]: http://wiki.metawerx.net/wiki/ForcingSSLForSectionsOfYourWebsite
