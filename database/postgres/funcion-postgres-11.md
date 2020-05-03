# Ejemplo de función en Postgres 11
Una [función][defFuncion] recibe parámetros (tipos de datos primarios), aplica la lógica indicada y devuelve un valor.

Hay un [tutorial][tutoBasico] básico [aquí][tutoBasico] para crear, ejecutar y borrar una función simple.

## 1. Metadatos

### Autor
@newdigicash
### Versión
0.1

## 2. Observación

__IMPORTANTE__: El tipo de dato ***double precision*** es incompatible con postgres 9. 
Hay que usar ***bigdecimal*** en su lugar.

## 3. Contenido 
El siguiente ejemplo muestra cómo pasar un entero largo, una fecha y un decimal, 
guardar en una tabla y devolver el registro guardado.
~~~
CREATE OR REPLACE FUNCTION fn_ejemplo(
	param_id numeric,
	param_fecha date,
	param_valor double precision
)
RETURNS TABLE (
	s_codigo int,
	s_nombre character varying (20),
	s_fecha character varying (50),
	s_valor decimal(18,2),
	s_estado char(1)
)
LANGUAGE plpgsql 
AS $$
declare
  val_numerico numeric(18,2) := 0;
begin
	val_numerico = cast(param_valor as numeric(18,2)); -- redondea a dos decimales
	INSERT INTO producto VALUES (param_id, 'FUN Ejemplo', cast(param_fecha as text), val_numerico, 'A');
	RETURN QUERY (SELECT * from producto where pr_codigo = param_id);
END
$$;
~~~
Si es necesario hay que redondear el parámetro decimal.

## 4. Fuentes
Doc oficial <https://www.postgresql.org/docs/11/sql-createfunction.html>

Tutorial de función <https://www.postgresqltutorial.com/postgresql-create-function/>

[//]: # (referencias citadas)
[defFuncion]: https://www.postgresql.org/docs/11/sql-createfunction.html
[tutoBasico]: https://todopostgresql.com/manejando-funciones-en-postgresql/
