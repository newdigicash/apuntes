# Codificación UFT-8 en Tomcat
La [codificación de caracteres UFT8][tutoCarEspecial] permite trabajar con caracteres especiales 
como tildes, eñes, diéresis.

Estos caracteres son necesarios para mostrar información correctamente desde la DB, o 
para agregar información, desde un formulario, y persistir en la DB.

Hay un [tutorial][tutoUft8] con indicaciones para 
[agregar UFT8 en Tomcat usando un filtro personalizado][tutoUft8] y [otro tutorial][tutoUftT6] 
que emplea el [filtro predefinido de Tomcat][tutoUftT6].

## 1. Metadatos

### Autor
@newdigicash

### Versión
0.1

## 2. Observación

Esta configuración ha sido probada en Tomcat 9

## 3. Contenido 
Para agregar la configuración UFT8 a Tomcat, hay que seguir estos pasos.

**Paso 1**. Buscar y editar el archivo *web.xml*
~~~
sudo ls -l /opt/tomcat/latest/conf
sudo vim /opt/tomcat/latest/conf/web.xml
~~~

Descomentar el filtro de codificación en *web.xml*. Si no existe hay que agregar.
~~~
	<filter>
		<filter-name>setCharacterEncodingFilter</filter-name>
		<filter-class>org.apache.catalina.filters.SetCharacterEncodingFilter</filter-class>
		<init-param>
			<param-name>encoding</param-name>
			<param-value>UTF-8</param-value>
		</init-param>
		<async-supported>true</async-supported>
	</filter>
~~~

Descomentar el mapeo en *web.xml*. Si no existe, agregar. 
~~~
	<filter-mapping>
		<filter-name>setCharacterEncodingFilter</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>
~~~

**Paso 2**. Buscar y editar el archivo *server.xml*
~~~
sudo ls -l /opt/tomcat/latest/conf
sudo vim /opt/tomcat/latest/conf/server.xml
~~~

Agregar la codificación _URIEncoding_ a cada _Connector_ en *server.xml*
~~~
<Connector URIEncoding="UTF-8" port="8080" ... />
~~~

**Paso 3**. Reiniciar Tomcat
~~~
sudo systemctl restart tomcat
~~~

O manualmente
~~~
sudo /opt/tomcat/latest/bin/shutdown.sh
sudo /opt/tomcat/latest/bin/startup.sh
~~~

## 4. Fuentes
Codificación UFT8 <https://es.wikipedia.org/wiki/UTF-8>

Caracteres especiales con UFT8 <http://blog.fidelizador.com/2019/12/17/utf8-n-tildes-y-caracteres-especiales/>

Tutorial para tomcat 6 <https://paucls.wordpress.com/2013/01/18/solucionando-problema-encoding-utf-8-en-java-webapp-sobre-tomcat-6/>

Tutorial UFT8 <https://www.baeldung.com/tomcat-utf-8>

[//]: # (aqui van las referencias citadas)
[tutoUft8]: https://www.baeldung.com/tomcat-utf-8
[tutoUftT6]: https://paucls.wordpress.com/2013/01/18/solucionando-problema-encoding-utf-8-en-java-webapp-sobre-tomcat-6
[tutoCarEspecial]: http://blog.fidelizador.com/2019/12/17/utf8-n-tildes-y-caracteres-especiales
