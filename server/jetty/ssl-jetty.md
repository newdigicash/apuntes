# Certificado SSL en Jetty 9
[Jetty][urlJetty] permite la configuración de un [certificado SSL][urlJetty]. 

En la [documentación oficial][urlJettySsl] hay detalles para generar un certificado, 
para armar un keystore con certificados, la estructura del keystore 
y habla de _SslContextFactory_, encargado de la configuración SSL en el server.

Hay un [tutorial][urlTutoJetty] que explica la [configuración del certificado][urlTutoJetty] 

Las [indicaciones para hacer la redirección][urlRedirectSSL] hacia el protocolo seguro 
están [aquí][urlRedirectSSL].

## 1. Metadatos

### Autor
@newdigicash
### Versión
0.1

## 2. Observaciones
+ En caso de falla al arrancar Jetty luego de la configuración, hay que revisar 
la causa en el log *cat /var/log/jetty9/out.log* o en *cat /var/log/jetty9/\*.debug.log*.
+ Los instrucciones de configuración fueron probadas en *Jetty 9.4.28*. Para 
[versiones anteriores, aquí están las indicaciones][urlTutoConfigSsl].

## 3. Contenido

### 3.1 Pre requisitos
Hay que [generar un keystore en formato PKCS12][urlPem2P12] a partir de 
los certificados emitidos por una CA. El archivo necesario para esta 
configuración es:
+ almacén de llaves. Ejemplo *mikeystore.p12*

### 3.2 Instalación y configuración

#### 3.2.1 Ubuntu

**Paso 1**. [Generar un keystore con keytool][urlPem2P12]. El archivo final 
puede tener extension *.p12* o *.jks*.

A partir de *mikeystore.p12* generado con openssl, keytool lo importa obteniendo 
*mikeystore.jks* con _mipassphrase_. [Aquí están más detalles][urlPem2P12].

~~~
sudo keytool -importkeystore -srckeystore mikeystore.p12 -srcstoretype PKCS12 -alias dominio.com -deststoretype PKCS12 -destkeystore mikeystore.jks
~~~

**Paso 2**. Buscar la libreria _jetty-util-\*.jar_ y proteger _mipassphrase_. 
Esto genera una cadena de caracteres empezando con _OBF:\*_.

~~~
sudo find / -name jetty-util-*.jar
java -cp /usr/share/jetty9/lib/jetty-util-9.4.*.jar org.eclipse.jetty.util.security.Password mipassphrase
~~~

**Paso 3**. Cambiar los paths en _jetty-ssl-context.xml_ para indicar las rutas absolutas.

~~~
ls -l /etc/jetty9
sudo vim /etc/jetty9/jetty-ssl-context.xml
~~~

La ruta de *keyStorePath* y *TrustStorePath* predeterminada es *jetty.base/etc/keystore*. 
Para indicar otra carpeta, donde está el keystore, es necesario quitar la *jetty.base*. 
Tal como se indica a continuación.
~~~
<Configure id="sslContextFactory" ..>
	...
	<Set name="KeyStorePath"><Property name="jetty.sslContext.keyStorePath" deprecated="jetty.keystore" default="etc/keystore"/></Set>
	<Set name="TrustStorePath"><Property name="jetty.sslContext.trustStorePath" deprecated="jetty.truststore" default="etc/keystore"/></Set>
	...
</Configure>
~~~

**Paso 4**. Crear o modificar el archivo *ssl.ini* y agregar la configuración 
~~~
sudo vim /etc/jetty9/start.d/ssl.ini
~~~

Activar el módulo *https* e indicar los paths, las passwords y el puerto para ssl 
en *ssl.ini*.

~~~
--module=https
# config para ssl
jetty.httpConfig.securePort=8443
jetty.ssl.port=8443
jetty.sslContext.keyStorePath=/ruta/mikeystore.jks
jetty.sslContext.trustStorePath=/ruta/mikeystore.jks
jetty.sslContext.keyStorePassword=OBF:19iy19j019j219j419j619j8
jetty.sslContext.keyManagerPassword=OBF:19iy19j019j219j419j619j8
#jetty.sslContext.trustStorePassword=OBF:19iy19j019j219j419j619j8
jetty.sslContext.keyStoreType=PKCS12
~~~

Las variables usadas en la configuración provienen de los archivos.
~~~
sudo cat /etc/jetty9/jetty-ssl-context.xml
sudo cat /etc/jetty9/jetty-ssl.xml
sudo cat /etc/jetty9/jetty-https.xml
~~~

**Paso 5**. Reiniciar jetty

~~~
sudo service jetty9 restart
~~~

### 3.3 Post instalación

#### 3.3.1 Habilitar el puerto
[Habilitar el puerto 8443 en el firewall][urlTutoFw] y/o router del server 
o el que corresponda según la configuración.

#### 3.3.2 Redirección a protocolo seguro

**Paso 1**. Buscar el archivo de configuración *webdefault.xml*.
~~~
sudo ls -l /etc/jetty9
sudo vim /etc/jetty9/webdefault.xml
~~~

**Paso 2**. Agregar la restricción de seguridad en *webdefault.xml* 
para hacer la [redirección temporal (estado 302)][urlRedirectSSL] a protocolo seguro.
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

Hay [otras formas de configurar la redirección usando _Rewrite_][urlJettyRewrite].

## 4. Fuentes
Jetty <https://www.eclipse.org/jetty/>

Configuración SSL <https://www.eclipse.org/jetty/documentation/current/configuring-ssl.html>

Tutorial de configuración <http://whitehorseplanet.org/gate/topics/documentation/public/jetty_ssl_configuration.html>

Jetty Rewrite <https://www.eclipse.org/jetty/documentation/current/rewrite-handler.html>

[//]: # (referencias citadas)
[urlJetty]: https://www.eclipse.org/jetty
[urlJettySsl]: https://www.eclipse.org/jetty/documentation/current/configuring-ssl.html
[urlTutoSsl]: https://github.com/newdigicash/apuntes/blob/master/security/certificado-ssl.md
[urlPem2P12]: https://github.com/newdigicash/apuntes/blob/master/java/util/convierte-pem-pkcs12.md
[urlTutoJetty]: http://whitehorseplanet.org/gate/topics/documentation/public/jetty_ssl_configuration.html
[urlJettyRewrite]: https://www.eclipse.org/jetty/documentation/current/rewrite-handler.html
[urlTutoConfigSsl]: https://wiki.eclipse.org/Jetty/Howto/Configure_SSL
[urlTutoFw]: https://github.com/newdigicash/apuntes/blob/master/linux/firewall-linux.md
[urlRedirectSSL]:  https://wiki.eclipse.org/Jetty/Howto/Configure_SSL
