# CRUD y consultas en MongoDb
## Crear una base de datos
use db1  (Solo se crea si contiene po lo menos una coleccion)

## Como crear una coleccion 
**use bd1** 
db.createCollection("Empleado")

## Mostrar las colecciones 
show collections

## Insertar un documento

```json
db.alumnos.insertOne(
 {
 nombre:'Soyla',
 apellido1:'Vaca',
 edad:32,
 Ciudad:'San Miguel de las piedras'
 })
```
## Insercion de un documento mas complejo con array

``` json
db.alumnos.insertOne(
 {
 nombre:'Joaquin',
 apellido:'Dorian',
 apellido2:'Guerrero',
 edad:15,
 aficiones:[
    'cerveza','hueva','Canabis'
 ]
 }
 )
 ```

 ## Insercion de documentos maa complejos con documentso anidado y ID

 ``` json
 db.alumnos.insertOne(
... {
 nombre:'Paps',
 apellido:'Destroyer',
 estudios:[
 'Primaria',
 'Secundaria',
 'Prepa'
 ],
  experiencia:{
 lenguaje:'SQL',
 exp:1
 }
 }


 db.alumnos.insertOne({
    _id:3,
    nombre:'Sergio',
    apellido:'Ramos',
    equipo:'Monterrey',
    aficiones:{
        futbol:true,
        bañarse:false
    }
})
 ```

## Insertar multiples documentos 

```json
db.alumnos.insertMany(
 [
 {
 _id:12,
 nombre:'Roberto',
 apellido:'Gomez',
 edad:'23',
 descripcion:'Es un comediante'
 },
 {
 nombre:'Luis',
 apellido:'Suarez',
 edad:43,
 habilidades:[
 'Correr','Dormir','Morder'
 ],
 direcciones:{
 calle:'Del infierno',
 numero: 666
},
esposas:[
    {nombre:'Marisol',
    edad:20,
    pension:350,
    hijos:['Joaquin','Briyet']
    },
    {
        nombre:'Dorien',
        edad:46,
        pension:6000,
        complaciente:true
    }
]
 }
 ]
)
```

# Practica
1. Bases de datos, colecciones e inserts
    . Nos conectamos con mongosh a MongoDb
    . Crear una base de datos denominada curso

    ```json
    use curso
    ```

    show dbs

    . crear una coleccion denominada "facturas" con el comando createCollection
    y comprobar que aoarece tanto la coleccion como la base de datso


# Practica 1
## Cargar Datos

[Libros.json](./data/libros.json)

## Busquedas. Condiciones de igualdad. Metodo find()
1. Selecionar todos los documentos de la coleccion libros

``` json
db.libros.find({})
```

2. Mostrar todos los documentos que sean de la editorial biblio
``` json
db.libros.find({editorial:'Biblio'})
```
3. Mostrar todos los documentos que le precio sea 25
```json
db.libros.find({precio:25})
```
4. Seleccionar todos los documentos donde titulos sea 'JSON para todos' 
```json
db.libros.find({titulo:'JSON para todos'})
```
## Operadoderes de comparacion 
![Operadores de Comperacion](./img/Operadores%20relacionales.png)

1. Mostrar todos los documentos donde el precio sea mayor a 25
```json
db.libros.find({precio:{$gte:25}})
```
2. Mostrar los documentos donde el precio sea 25
```json
db.libros.find({precio:{$gte $eq 25}})
```

3. Mostrar los documentos cuya cantidad sea menor a 5
```json
db.libros.find({precio:{$lt: 5}})
```

4. Mostrar los documentos que pertenezcan a la editorial Biblio o planeta
```json
db.libros.find({editorial:{$in: ['Biblio','Planeta']}})
```
5. Mostrar todos los libros que cuestente 20 o 25
```json 
db.libros.find({precio:{$in:[20,25]}})
```

6. Mostrar todos los documentos de libros que no cuesten 20 o 25
```json
 db.libros.find({precio:{$nin:[20,25]}})
```
7. Mostrar el primero documentos de libros que cueste 20 0 25
```json
db.libros.findOne({precio:{$in:[20,25]}})
```
## Operadores logicos
![Operadores logicos](./img/Operadores%20Logicos.png)

## Operador AND
Dos posibles opciones de AND
1. La simple, mediante condiciones sepaeradas por comas

***sintaxis***

db.coleccion.find({condicion1,condicion2}) -> con est asume que es una **and**

2. Usando el operador $and

***Sintaxis***
```json
db.coleccion.find({$and:[{condicion1},{condicion2}]})
```
### Ejercicios 
1. Mostrar todoos aquellos libros que cuesten mas de 25 y cuya cantidad sea inferiror a 15
```json
db.libros.find({ precio: { $gte: 25}, cantidad: {$lt: 15} } )
```

***Forma simple***
```json
db.libros.find({ $and:[ {precio: { $gte: 25}}, {cantidad: {$lt: 15}}] } )
```
2. Mostar todos aquelllos libros que cuesten mas de 25 y cuya cantidad sea inferior a 15 y id igual a 14
```json
db.libros.find({ precio: { $gte: 25}, cantidad: {$lt: 15}, _id:4 } )
```
### Operador OR 
#### Mostrar todos aquellos libros que cuesten mas de 25 o cuya cantidad sea inferior a 15

```json
db.libros.find({$or:[{precio:{$gte:25}},{cantidad:{$lt:15}}]})
```


### AND y OR combinadas

1. Mostar los libros de la editorial BIblio o recio mayor a 40 o Libros de la editorial planeta
con precio mayor a 30 

```json
db.libros.find({
    $or:[
        {$and:[{editorial:'Biblio'},{precio:{$gt:30}}]},
        {$and:[{editorial:{$eq:'Planeta'}},{precio:{$gt:20}}]}
    ]
})
```
##### De la otra forma 
```json
db.libros.find({
    $or:[
        {editorial:'Biblio'},{precio:{$gt:30}},
        {editorial:{$eq:'Planeta'}},{precio:{$gt:20}}
    ]
})
```

## Proyeccion de columnas

*** Sintaxis  ***
db.coleccion.find(filtro,columnas)

db.libros.find({},{titulo:1})

1. Seleccionar todos los documentos, mostrando el titulo y la editorial

```json
db.libros.find({},{titulo:1,editorial:1})
db.libros.find({},{titulo:1,editorial:1,_id:0})
```

2. Seleccionar todos los documentos de la editorial Planeta, 
mostrando solamente el titu;o y la editorial

```json
db.libros.find({editorial:'Planeta'},{_id:0,titulo:1,editorial:1})
```

## Operador exists (Permite saber si un campo se enecuentra o no en un documento)


```json
db.libros.find({
    editorial:{$exists:true}
})
```

```json
db.libros.insertOne({
    _id:10,
    titulo:'Mongo en entornos graficos',
    editorial:'Terra',
    precio:25
})
```
1. Mostrar todos los documentos que no contengan el campo cantidad

```json
db.libros.find(
{
    cantidad:{$exists:false}
}
)
```

## Operadoe Tyoe (Permite preguntar si un determinado campo corresponde con un tipo)

[Operador type](https://www.mongodb.com/docs/manual/reference/operator/query/type/#mongodb-query-op.-type)


1. Mostar todos los documentos donde el precio sean doubles

```json
db.libros.find({precio:{$type:16}})
```

```json
db.libros.insertOne({
    _id:11,
    titulo:'IA',
    editorial:'Terra',
    precio:125.4,
    cantidad:20
})
```

```json
db.libros.insertMany([
 {
    _id: 12,
    titulo: 'IA',
    editorial: 'Terra',
    precio: 125, 
	cantidad: 20
  },
  {
    _id: 13,
    titulo: 'Python para todos',
    editorial: 2001,
    precio: 200, 
	cantidad: 30
  }]
  )
```

1. Seleccionar los documentos donde la editorial sea de tipo entero
```json
db.libros.find({editorial:{$type:16}})
db.libros.find({editorial:{$type:'int'}})

```

2. Seleccionar todos los documentos donde la editorial sea string
```json
db.libros.find({editorial:{$type:2}})
db.libros.find({editorial:{$type:'string'}})
```

## Practica de consultas
1. Instalar las tools de MongoDb
[DatabaseTool](https://www.mongodb.com/try/download/database-tools)

2. cargar el json empleados (Debemos estar ubicados en la carpeta
 donde se enecuentra el json empleados)

 - En local:
    comando:
        mongoimport --db curso --collection empleados --file empleados.json

# Modificando Documents
## Comnados importantes
1. updateOne -> Modificar un solo documento
2. updateMany -> Modificar multiples documentos
3. replaceOne -> Sustituir el contenido completo de un documento

Tiene el siguiente formato

```json
db.collection.updateOne(
{filtro},
{operador}
)
```
[Operadores update](https://www.mongodb.com/docs/manual/reference/operator/update/)

### Operador set
1. Modificar un documento
```json
 db.libros.updateOne({titulo:'Python para todos'},{$set:{titulo:'Java para todos'}})
```
2. Actualizar el precio 100 y la cantidad a 50 para el _id:10

```json
db.libros.updateOne({_id:10},{$set:{precio:100, cantidad:50}})
```

### MOdificar multiples documentos
-- Modificar todos los documentos donde el precio sea mayor a 100 a un precio de 150
```json
db.libros.updateMany(
{precio:{$gt:100}},
{$set:{precio:150}}
)
```

2. Operador $inc y $mul