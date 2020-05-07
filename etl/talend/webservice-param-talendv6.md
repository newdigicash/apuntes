# Ejecutar un job de Talend DI 6.0 con parámetros 
Indicaciones de cómo ejecutar un job de Talend con parámetros
## 1. Metadatos

### Autor
@newdigicash

### Versión
0.2

## 2. Observación

Falta revisar si Talend DI actual permite exportar como war y 
ejecutarlo en tomcat porque en talend 6.0 si se puede.

## 3. Contenido 
El [componente tJava][linkTjava] permite agregar código nativo. 
Se puede usar para [generar la fecha del sistema][linkGenerador] y asignar 
a las variables de contexto.

Una vez probado el Job, hay que exportar como web service. 
Todos los subjobs deben estar interconectados.

Para probar el web service pasando parámetros por la url 
~~~
http://localhost:8080/Pruebas_0.1/services/Pruebas?method=runJob&arg1=--context_param%20desde=2019-01-01&arg2=--context_param%20hasta=2019-02-01
~~~

## 4. Fuentes
Tutoriales de Talend <https://community.talend.com/t5/custom/page/page-id/Tutorials>

Doc de Tjava <https://help.talend.com/reader/wDRBNUuxk629sNcI0dNYaA/sSh_6LAVLHtLJc5jL6e21Q>

[//]: # (referencias citadas)
[linkTalend]: https://community.talend.com/t5/custom/page/page-id/Tutorials
[linkGenerador]: https://github.com/newdigicash/apuntes/blob/master/etl/talend/GeneraFecha.java
[linkTjava]: https://help.talend.com/reader/wDRBNUuxk629sNcI0dNYaA/sSh_6LAVLHtLJc5jL6e21Q
