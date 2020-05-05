# Comandos de MongoDB
Resumen de comandos más usados de [mongodb][urlMongo].

Para empezar a usar [mongodb][urlMongo], hay un [tutorial breve][urlTutoMongo] con ejemplos sencillos de entender.
## 1. Metadatos

### Autor
@newdigicash
### Versión
0.1

## 2. Observación

En el ejemplo se busca productos.valor=15 o productos.valor\>=20 y no funciona. 
La condicion AND \+ OR hay que revisar.

## 3. Contenido 
### 3.1 Comandos

Cmd | Descripción
:-- | :--
`use mibase` | crea una base nueva o selecciona *mibase*
`db` | para ver la base seleccionada
`show dbs` | lista bases creadas
`db.createCollection("mitabla")` | colección implícita de *mitabla*
`show collections` | muestra todas las coleciones
`db.usuarios.insert({..})` | crea una colección explícita _usuarios_ y agrega
`db.productos.insert({...})` | agrega un registro nuevo en _productos_
`db.p.update({clave},{ $set: {valor} })` | actualiza *valor* dado la _clave_ en *p*
`db.productos.deleteOne({clave})` | elimina un _producto_ con *clave* dada
`db.productos.find({"condicion":valor})` | match exacto con *condicion* en _productos_
`db.productos.find().pretty()` | muestra todos los datos de *productos*
`db.productos.find().limit(num)` | obtiene *num* registros desde *productos*
`db.productos.find().sort({campo: 1})` | ordena por *campo*. 1 ascendente -1 descendente
`db.productos.drop()` | elimina colección *productos*
`db.dropDatabase()` | elimina la base

### 3.2 Ejemplos de Uso
Crear colección explícita e insertar registro
~~~
db.usuarios.insert({ "cedula": "123456789","pais": "ecuador"})
~~~

Insertar
~~~
db.productos.insert({"id": "1", "nombre":"camiseta L", "valor": 18.5, "stock":2})
db.productos.insert({"id": "2", "nombre":"camiseta M", "valor": 15.0, "stock":5})
~~~

Actualizar
~~~
db.productos.update({"id":"1"},{ $set: {"valor": 20.45} }) 
~~~

Borrar
~~~
db.productos.deleteOne({ "id":"1"})
~~~

Buscar y filtrar
~~~
db.productos.find({"valor":15}) #match exacto
db.productos.find({"valor": {$lt: 16} }) #match con menor que
db.productos.find({"valor": {$gte: 15}, "valor": {$lte: 21} }) #match AND  15<=valor<=21
db.productos.find({ $or: [{"valor": 15}, {"valor": 20}] }) #match OR valor=15 o valor=20
db.productos.find({ "valor": {$gte: 20}, $or: [{"valor": 15}] }) #match AND + OR
db.productos.find().limit(1)
db.productos.find().sort({valor: 1}) #ordena ascendentemente
~~~

## 4. Fuentes
Doc oficial <https://docs.mongodb.com/manual/contents/>

MongoDB en 20 mins <https://www.youtube.com/watch?v=c8n6JsQuX2A>

[//]: # (referencias citadas)
[urlMongo]: https://www.mongodb.com/
[urlTutoMongo]: https://www.youtube.com/watch?v=c8n6JsQuX2A
