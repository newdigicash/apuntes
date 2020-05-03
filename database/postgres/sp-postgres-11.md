# Ejemplo de SP en Postgres 11
Un [procedimiento almacenado][defSp] recibe parámetros (tipos de datos primarios) y aplica la lógica indicada. 
A diferencia de una [función][defFuncion], un [procedimiento almacenado][defSp] no retorna valores.

Hay un [tutorial][tutoBasico] básico [aquí][tutoBasico] para crear y ejecutar un [SP][defSp] simple.

## 1. Metadatos

### Autor
@newdigicash
### Versión
0.1

## 2. Observación

__IMPORTANTE__: Los [SP][defSp] [aparecen en Postgres 11][posNoticia] en adente, 
además tienen [soporte para transacciones][posNoticia]. 
He aquí un [ejemplo][tutoBasico].
## 3. Contenido 
El siguiente ejemplo muestra cómo pasar un entero largo, una fecha y un decimal, 
y guardar en una tabla.
~~~
CREATE OR REPLACE PROCEDURE sp_ejemplo(
	param_id numeric,
	param_fecha date,
	param_valor numeric
)

LANGUAGE plpgsql 
AS $$
declare
  salida numeric(18,2) := 0;
begin
	salida := param_id * param_valor;
	INSERT INTO producto VALUES (param_id, 'SP Ejemplo', cast(param_fecha as text), param_valor, 'A');
	return ;
END
$$;
~~~
Si es necesario hay que redondear el parámetro decimal.

## 4. Fuentes
Doc oficial <https://www.postgresql.org/docs/11/sql-createprocedure.html>

Tutorial de procedimiento <https://severalnines.com/database-blog/overview-new-stored-procedures-postgresql-11>

[//]: # (referencias citadas)
[defFuncion]: https://www.postgresql.org/docs/11/sql-createprocedure.html
[defSp]: https://www.postgresql.org/docs/11/sql-createprocedure.html
[posNoticia]: https://www.postgresql.org/about/news/1894/
[tutoBasico]: https://severalnines.com/database-blog/overview-new-stored-procedures-postgresql-11
