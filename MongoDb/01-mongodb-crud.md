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

