# Busquedas en Arrays y documentos anidados

##  Buscar dentro de un documento anidado

```json
db.Ciudades.find({"alcalde.nombre":"cristal"})
```
1. Seleccionar las ciudades donde los alcaldes tengan mas de 70 años
```json
db.Ciudades.find({"alcalde.edad":{$gt:70}})
```

2. Realizar la consulta anterior pero usando una proyeccion
```json
db.Ciudades.find({"alcalde.edad":{$gt:70}},{_id:0,"alcalde.edad":1})
```
3. Seleccionar las ciudades donde el alcalde tenga 76 años 0 41

```json
db.Ciudades.find({$or[
   { {"alcalde.edad":70},{_id:0,"alcalde.edad":1}},
   { {"alcalde.edad":41},{_id:0,"alcalde.edad":1}}
]})
```

```json
db.ciudades.find({
   $or:[
      {"alcalde.edad"},
      {}
   ]

})
```

## Consultas con Arrays
1. Crear una coleccion cazas
```json
db.createCollection("Cazas")
```
2. Cargar los documentos de la coleccion cazas, de la data cazas
## EJemplos de consulta con Arrays
1. Buscar los documentos que tenga usa como paises
```json
db.cazas.find({paises:"usa"})
```
2. Buscar los docuemntos donde cualquier elemnto del array dimensiones sean mayores a 5
```json
db.Cazas.find({dimensiones:{$gt:5}})
```
3. Buscar los documentos donde las dimensiones del avion por ancho que es la posicion 1, sea superior a 14
```json
db.Cazas.find({
   "dimensiones.1":{$gt:14}
},
{
   _id:0,modelo:1
}
)
```
### Arrays. Operador $all
1. Buscar todos los aviones que estan sirviendo a egipto y en israel
```json
db.Cazas.find({paises:{$all:['egipto','israel']}},{_id:0,modelo:1,paises:1})
```
2. Seleccionar los Cazas que sirven in Inglaterra y españa
```json
db.Cazas.find({$and:[
   {paises:'inglaterra'},
   {paises:'españa'}
]})

db.Cazas.find(
{
   paises:{$all:['inglaterra','españa']}
}
)
```

### Arrays operador $elemenMatch -> Permite hacer busquedas dentro de arrays, estee busca la lista de condiciones que estan dnertro del operador
1. buscar que aviones tienen uso dentro de inglaterra

```json
db.Cazas.find({paises:{$elementMatch:{$eq:'inglaterra'}}})
```
2. Buscar los aviones donde las dimensiones sean mayores a 20 o mayores a 15
```json
db.Cazas.find({dimensiones:{$elemMatch:{$lt:20,$gt:15}})
```
### Buscar Arrays con documentos anidados
1. utilizar la coleccion ciudades
2. Buscar los consejeros de nombre Jeri Flower de edad 78

```json
db.ciudades.find({identificador:1,nombre:"Jeri Flowers",edad:78})
```
3. Buscar en consejeros donde tengan una edad mayor a 70
```json
db.Ciudades.find({"consejeros.edad":{$gt:70}})
```
4. Buscar en consejeros en la posicion 2 que esten entre 70 y 80 años

```json
`db.Ciudades.find({
   'consejeros.2.edad':{$gt:70,$lt:80}
},
{
   _id:0,"consejeros.nombre":1,"consejeros.edad":1
})`
```

5. Buscar los consejeros de la posición 0 que sean mayores a 70 años

json
db.ciudades.find({
  "consejeros.0.edad":{$gt:70}  
}, {_id:0, "consejeros.nombre":1, "consejeros.edad":1})


6. Buscar consejeros que sean mayores a 70

json
db.ciudades.find({
  "consejeros.edad":{$gt:70}
}, {_id:0, "consejeros.nombre":1, "consejeros.edad":1})
