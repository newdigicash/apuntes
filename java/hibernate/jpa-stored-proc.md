# Funciones con StoredProcedureQuery de JPA
La implementación de la interfaz [StoredProcedureQuery en Hibernate][urlHibernate] permite 
ejecutar funciones en la base de datos con parámetros y recibir una respuesta.

En este [tutorial][urlEjemplo] hay un ejemplo con parámetros de entrada y salida. 
Además [aqui][urlEjmplos] hay [varios ejemplos][urlEjmplos].

## 1. Metadatos

### Autor
@newdigicash
### Versión
0.1

## 2. Observación

Aunque [StoredProcedureQuery en Hibernate][urlHibernate] parece que sirviera para 
procedimientos almacenados, de hecho, se **usa para funciones**. Ya que se ejecuta CALL para 
llamar a un SP y _query.execute()_ no lo hace.

## 3. Contenido 
El siguiente ejemplo sirve para ejecutar una [función en Postgres][urlMiFun] desde Java
~~~
	public void llamarSP(long id, String fecha, double valor) {
    	try {
    		StoredProcedureQuery query = entityManager.createStoredProcedureQuery("fn_ejemplo");
            query.registerStoredProcedureParameter(1, Long.class, ParameterMode.IN);
            query.registerStoredProcedureParameter(2, Date.class, ParameterMode.IN);
            query.registerStoredProcedureParameter(3, Double.class, ParameterMode.IN);
            query.setParameter(1, id);
            query.setParameter(2, new SimpleDateFormat("yyyy-MM-dd").parse(fecha), TemporalType.DATE );
            query.setParameter(3, valor);
            List<Producto> respuesta = query.getResultList();
            System.out.println("Registro ingresado: "+respuesta.size());
    	}catch(Exception e) {
            System.out.println(e.getMessage());
    	}
	}
~~~

Para ejecutar la función anterior:

	llamarSP(100, '2020-02-01', 12.345)

## 4. Fuentes
Doc oficial <https://docs.oracle.com/javaee/7/api/javax/persistence/StoredProcedureQuery.html>

Doc en hibernate <https://docs.jboss.org/hibernate/jpa/2.1/api/javax/persistence/StoredProcedureQuery.html>

[//]: # (aqui van las referencias citadas)
[urlMiFun]: https://github.com/newdigicash/apuntes/blob/master/database/postgres/funcion-postgres-11.md
[urlEjmplos]: https://www.programcreek.com/java-api-examples/?api=javax.persistence.StoredProcedureQuery
[urlEjemplo]: https://thoughts-on-java.org/call-stored-procedures-jpa-part-2/
[urlHibernate]: https://docs.jboss.org/hibernate/jpa/2.1/api/javax/persistence/StoredProcedureQuery.html
