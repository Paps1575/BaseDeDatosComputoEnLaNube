# Consultas 

1. Cargar el archibvo empledos.json
- En local:
    Comando:
      mongoimport --db curso --collection empleados --file empleados.json

- Docker:
    comando:
      mongoimport --port 80 --db curso --collection empleados --file empleados.json

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
