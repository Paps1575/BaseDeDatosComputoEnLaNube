# Consultas 

1. Cargar el archibvo empledos.json
- En local:
    Comando:
      mongoimport --db curso --collection empleados --file empleados.json

- Docker:
    comando:
      mongoimport --port 80 --db curso --collection empleados --file empleados.json
      

      
```json
Nota: Si no existe el campo lo genera
      db.libros.updateOne({_id:2},{$set:{precio:56,existenci:10}})
```
2. Utilizar la base de datos curso

```json
use curso
```
3. Buscar todos los empleados que trabajen en Google
```json
db.empleados.find({'empresa':'Google'})
```
4. Empleados que vivan en Peru
```json
 db.empleados.find({pais:'Peru'})
```
5. Empleados que ganen mas de 8000 dolares
```json
db.empleados.find({salario:{$gt:8000}})
```
6. Empleados con ventas inferiores a 10,000
```json
db.empleados.find({salario:{$lt:10000}})
```
7. Realizar la consulta anterior pero devolviendo una sola fila 
```json
db.empleados.findOne({salario:{$lt:10000}})
```
8. Empleados que trabajan en Google o en Yahoo! con el operador in
```json
db.empleados.find({$or:[
    {empresa:'Google'},
    {empresa:'Yahoo'}
]})
```
9. Empleados de amazon que ganen mas de 9000 dolares 
```json
db.empleados.find({$and:[
    {empresa:'Amazon'},
    {salario:{$gt:8000}}
]})
```
10. Empleados que trabajen en Google o Yahoo con el operador $or
```json
db.empleados.find({
    $or:[{empresa:'Amazon'},{empresa:{$eq:'Yahoo'}}]
})
```
11. Empleados que trabajen en YAhoo! que ganen mas de 6000 o empleados que trabajen en 
Google que tenga ventas inferiores a 20,000
```json
db.empleados.find({$or:[
    {$and:[{empresa:'Yahoo'},{salario:{$gt:6000}}]},
    {$and:[{empresa:'Google'},{ventas:{$lt:20000}}]}
]})
```
11. Visualizar el nombre, apellidos y el pais de cada empleado
```json
db.empleados.find({},{nombre:1,apellidos:1,pais:1},_id:0)
```
12. Operador $inc y $mul
- Actualizar con un incremento de 5 todos los documentos
```json
```
```json
db.libros.updateMany(
    {},
    {$inc:{precio:5}}
)
```
- Actualizar con una multiplicacion de 2 todos los documentos que sean cantidad mayores a 20
```json
db.libros.updateMany({cantidad:{$gt:20}},{$mul:{cantidad:2}})
```
- Actualizar todos los documentos donde el precio sea mayor a 20 y se multiplique por 2 la cantidad y el precio
```json
db.libros.updateMany({precio:{$gt:20}},{$mul:{cantidad:2,precio:2})
```
3. Remplazar Documentos (replaceOne)
```json
db.libros.replaceOne({_id:2},{
    titulo:'De la tierra a la luna',
    autor:'Julio Verne',
    precio:500
})
```
# Borrar Documentos
1. deleteOne -> Elmina un solo documento
2. deleteMany -> Elimina multiples documentos
1. Eliminar el documento con id 2
```json
db.libros.deleteOne({_id:2})
```
2. Eliminar los documentso donde la cantidad sea mayor o igual a 150
```json
 db.libros.deleteMany({cantidad:{$gte:100}})
 ```

 # Expresiones regulares
 1. Buscar los libros que contenga el titulo la letra t 
 ```json
 db.libros.find({titulo:/t/})
 ```
 2. Buscar los libros que en el titulo contenga la palabra json
```json
 db.libros.find({titulo:/JSON/})
```
3. Buscar todos los documentos que en tiltulo terminen en tos
```json
db.libros.find({titulo:/tos$/})
```
4. Todos los documentos que en el titulo comienzen con J
```json
db.libros.find({titulo:/^J/})
```
# Operaedor $regex

[Operador Regex](https://www.mongodb.com/docs/manual/reference/operator/query/regex/)

1. Seleccionar los libros que contengan la palabra para en titulo
```json
db.libros.find({titulo{$regex: 'para'}})

 json
db.libros.find({titulo:{$regex:'JSON'}})

 json
db.libros.find({titulo:{$regex:/JSON/}})

```
- Distinguir entre MAYUSCULAS y minusculas
 ```json
db.libros.find({titulo:{$regex:/JSON/}})-> No distingue entre MAYUSCULAS y minusculas

 json
db.libros.find({titulo:{$regex:/JSON/, $options:"i"}})
```

2. Seleccionar todos los libros que comiencen con J o j

 ```json
db.getcollection('libros'),find({
  titulo: {$regex: RegExp(^)}
})

db.libros.find({titulo:{$regex: 'es$', $options: 'i'}})
```
# Metodo sort (Ordenar Documentos)

1. Ordenar los libros de manera accendente por el precio
 ```json
db.libros.find({}, {titulo:1, precio:1, _id:0}).sort({precio:1})
```
2. Ordenar los libros de manera descendente por el precio
``` json
db.libros.find({}, {titulo:1, precio:1, _id:0}).sort({precio:-1})
```
3. Ordenar los libros de manera asendente por la editorial y de manera descente por el precio
mostrando el titulo el precio y la editorial
 ```json
db.libros.find({}, {titulo:1, precio:1,editorial:1, _id:0}).sort({editorial:-1, precio:-1})
```
# Otros Metodos Skip, limit, size
 ```json
db.libros.find({}, {titulo:1, precio:1, _id:0, editorial:1}).size()
```
 ```json
db.libros.find({titulo:{$regex:/Java/}})
```
 ```json
db.libros.find({titulo:{$regex:/Java/}}).size()
```
1. Buscar todos los libros pero mostrando los dos libros
 ```json
db.libros.find({},{titulo:1, editorial:1,precio:1, _id:0}).limit(2)
```
2. Mostrar los tres ultimos libros
 ```json
db.libros.find({},{titulo:1, editorial:1,precio:1, _id:0}).sort({precio:-1}).limit(3)
```
3. Seleccionar todos los libros ordenador por titulo de forma decendente saltando los primeros dos docuemtnos y que muestre el tamaño
 ```json
db.libros.find({}).sort({titulo:-1}).skip(2).size()
```

``` json
db.libros.find({}).sort({titulo:-1}).skip(2).size()
```

# Borrar Colecciones y base de Datos
 ```json
use db5

 db.createCollection ('Ejemplo')

show collections

db.ejemplo.insertOne({ nombre: 'Chapuin' } )

db.ejemplo.drop()
```
