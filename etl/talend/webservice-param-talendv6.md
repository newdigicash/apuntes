# Ejecutar un job de Talend 6.0 DI con parámetros 
Indicaciones de cómo ejecutar un job de Talend con parámetros
## 1. Metadatos

### Autor
@newdigicash

### Versión
0.1

## 2. Observación

Este archivo está sujeto a cambios

## 3. Contenido 
El componente tJava permite agregar código nativo. 
Se puede usar para [generar la fecha de sistema][linkGenerador] y asignar a las variables de contexto.

Una vez probador el Job, hay que exportar como web service. 
Todos los subjobs deben estar interconectados.

Para probar el web service pasando parámetros por la url 
~~~
http://localhost:8080/Pruebas_0.1/services/Pruebas?method=runJob&arg1=--context_param%20desde=2019-01-01&arg2=--context_param%20hasta=2019-02-01
~~~

## 4. Fuentes
Tutorial de Talend <https://community.talend.com/t5/custom/page/page-id/Tutorials>

[//]: # (referencias citadas)
[linkTalend]: https://community.talend.com/t5/custom/page/page-id/Tutorials
[linkGenerador]: https://github.com/newdigicash/apuntes/etl/talend/GeneraFecha.java
