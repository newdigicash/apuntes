# Certificado SSL en Tomcat
[Tomcat][urlTomcatSsl] permite la configuración de un [certificado SSL][urlTutoSsl]. 

En la [documentación oficial][urlTomcatSsl] hay indicaciones para preparar los certificados y 
hacer la configuración de la keystore [PKCS12 o JKS][urlTomcatSsl]. 
Tomcat usa un [_Connector_][urlTomcatConf] para configurar y tiene [varios parámetros][urlTomcatConf].

Hay un [tutorial][urlTutoSsltomcat] que explica la [configuración del certificado][urlTutoSsltomcat].

## 1. Metadatos

### Autor
@newdigicash
### Versión
0.4

## 2. Observaciones

- En la configuración con certificados individuales, la llave privada no debe estar encriptada 
para *tomcat 9* caso contrario aparece un error con AES cipher. No obstante, en *tomcat 8* no 
ocurre este problema.
- En caso de falla al arrancar Tomcat luego de la configuración, hay que revisar 
la causa en el log. El log puede estar en *cat /opt/tomcat/latest/logs/catalina.out* 
o similares.
- Para [instalar Tomcat 9 en Ubuntu][urlTutoInstalaTomcat] hay un [tutorial][urlTutoInstalaTomcat] 
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
	<SSLHostConfig>
		<Certificate 
			certificateKeystoreFile="/ruta/mikeystore.p12" 
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
o el puerto que corresponda según el Conector.

#### 3.3.2 Redirección a protocolo seguro

**Paso 1**. Buscar el archivo de configuración en la carpeta *conf*. 
El archivo predeterminado es *web.xml*.

~~~
sudo ls -l /opt/tomcat/latest/conf
sudo vim /opt/tomcat/latest/conf/web.xml
~~~

**Paso 2**. Agregar la [restricción de seguridad][urlEjemploSecurityConst] 
en *web.xml* para hacer la redirección temporal (estado 302) a 
protocolo seguro.

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

El [parámetro _url-pattern_][urlWebPattern] indica la url a ser protegida. 

El [parámetro _transport-guarantee_][urlTransportGuarantee] establece 
el nivel de protección a los datos. [*CONFIDENTIAL*][urlTransportGuarantee] 
garantiza que los datos no cambian por eso usa SSL y el *redirectPort* 
del _Connector_.

Hay otras [formas de configurar la restricción de seguridad][urlEjemploSecurityConst], 
he [aquí otros ejemplos][urlEjemploSecurityConst].

**Paso 3**. Buscar el archivo de configuración *server.xml*

~~~
sudo ls -l /opt/tomcat/latest/conf
sudo vim /opt/tomcat/latest/conf/server.xml
~~~

**Paso 4**. Agregar la [redirección permanente (estado 301)][urlEjemploSecurityConst]. 
El parámetro *transportGuaranteeRedirectStatus* establece una redirección con otro valor. 
El valor predeterminado es 302..

~~~
<Realm className="org.apache.catalina.realm.LockOutRealm" transportGuaranteeRedirectStatus="301">
	<Realm ... />
</Realm>
~~~

### 3.4 Configuración alterna de Conector

#### 3.4.1 Con certificados individuales
El parámetro [*certificateFile*][urlTomcatConf] es para el certificado primario de dominio, 
[*certificateKeyFile*][urlTomcatConf] es para la llave privada, y 
[*certificateChainFile*][urlTomcatConf] para el certificado chain (intermediate \+ root).

~~~
<Connector port="443" 
	protocol="org.apache.coyote.http11.Http11NioProtocol" 
	maxThreads="150" SSLEnabled="true" >
	<SSLHostConfig>
		<Certificate 
			certificateKeyFile="/ruta/private.key" 
			certificateFile="/ruta/certificate.crt"
			certificateChainFile="/ruta/ca_bundle.crt"
			type="UNDEFINED" />
	</SSLHostConfig>
</Connector>
~~~

Si la llave privada está encriptada entonces se debe agregar 
el parámetro *certificateKeyPassword* a *Certificate*.

#### 3.4.2 Soporte para [HTTP/2][urlIntroHttp2]

Acepta peticiones en HTTP/1.1 y [responde en HTTP/2][urlTomcatHttp2].

~~~
<Conector 
	protocol="org.apache.coyote.http11.Http11NioProtocol" 
	port="443" maxThreads="150" 
	scheme="https" secure="true" SSLEnabled="true" 
	clientAuth="false" >
	<UpgradeProtocol className="org.apache.coyote.http2.Http2Protocol" />
	<SSLHostConfig>
		<Certificate 
			certificateKeystoreFile="/ruta/mikeystore.p12" 
			certificateKeystorePassword="mipassphrase" />
	</SSLHostConfig>
</Conector>
~~~
[HTTP/2][urlIntroHttp2] funciona bien con *OpenJDK 11*, aunque parece que 
[Java 8u252 ya tiene soporte][urlNoticiaJava] para [ALPN][urlWikiAlpn].

### 3.5 Redirección alterna usando [_RewriteValve_][urlTomcatRewrite]

*Paso 1*. Buscar el archivo *server.xml* de configuración.

~~~
sudo ls -l /opt/tomcat/latest/conf
sudo vim /opt/tomcat/latest/conf/server.xml
~~~

*Paso 2*. Agregar el elemento _Valve_ a _Host_ en *server.xml*.

~~~
<Host>
	<Valve className="org.apache.catalina.valves.rewrite.RewriteValve" />
	...
</Host>
~~~

*Paso 3*. Crear un archivo de configuración nuevo.

~~~
sudo vim /opt/tomcat/latest/conf/Catalina/localhost/rewrite.config
~~~

*Paso 4*. Crear las condiciones y reglas para redireccionar cuando 
no sea protocolo seguro..

~~~
RewriteCond %{HTTPS} off 
RewriteRule ^(.*)$ https://dominio.com:puerto/$1 [R=301,L]
~~~

En la [documentación de tomcat][urlTomcatRewrite] hay detalles sobre 
la [configuración con _rewrite_][urlTomcatRewrite].


## 4. Fuentes
Doc oficial Tomcat <http://tomcat.apache.org/tomcat-9.0-doc/ssl-howto.html>

Parámetros de configuración Tomcat <https://tomcat.apache.org/tomcat-9.0-doc/config/http.html>

Tutorial de instalación <https://www.digicert.com/es/instalar-certificado-ssl-tomcat.htm>

Introduccion a HTTP/2 <https://developers.google.com/web/fundamentals/performance/http2?hl=es>

Wiki de ALPN <https://en.wikipedia.org/wiki/Application-Layer_Protocol_Negotiation>

Doc sobre Rewrite <https://tomcat.apache.org/tomcat-8.0-doc/rewrite.html>

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
[urlTomcatHttp2]: https://tomcat.apache.org/tomcat-9.0-doc/config/http.html#HTTP/2_Support
[urlIntroHttp2]: https://developers.google.com/web/fundamentals/performance/http2?hl=es
[urlNoticiaJava]: https://webtide.com/jetty-alpn-java-8u252
[urlWikiAlpn]: https://en.wikipedia.org/wiki/Application-Layer_Protocol_Negotiation
[urlTomcatRewrite]: https://tomcat.apache.org/tomcat-8.0-doc/rewrite.html
