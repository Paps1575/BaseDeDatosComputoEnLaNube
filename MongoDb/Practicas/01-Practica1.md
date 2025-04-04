# Practica 1: Base de datos, colecciones e inserts
| Codigo   | Valor   |
|-------------|-------------|
| Cod_Factura | 10 |
| Ciente | Frutas Ramirez |
| Total | 223 |
| Codigo   | Valor   |
|-------------|-------------|
| Cod_Factura | 10 |
| Ciente | Frutas Ramirez |
| Total | 223 |

1. Conectarnos con mongosh a mongodb
1. crear una base de datos llamada curso 
1. Comprobar que la base no existe 
1. Crear una coleccion que se llame facturas y mostarla 

``` json
db.createCollection('Facturas')
show collections
```

5. Insertar un documento con los siguientes datos: 


``` json
 db.facturas.insertOne(
 {
 cod_factura:10,
 Cliente:'Frutas Ramirez',
 total:123,
 unidades:1500
 }
 )
```
6. Crear una nueva coleccion pero usando directamente el insertOne. insertar un documento en la coleccion productos con los siguienres datos:

| Codigo   | Valor   |
|-------------|-------------|
| Cod_producto | 1 |
| nombre | Tornillo x 1 |
| precio  |

7. Crear un nuevo documento que contenga un array con los siguientes datos

| Codigo   | Valor   |
|-------------|-------------|
| Cod_producto | 2 |
| Nombre | Martillo |
| precio | 20 |
| Fabricantes | fab1, fab2. fab3, fab4 |

``` json 
db.producto.insertOne(
 {
 cod_producto:2,
 Nombre:'Martillo',
 precio:20,
 Fabricantes:[ 'fab1','fab2','fab3','fab3']
 }
 )
```
8. Borrar la colecion Facturas y comprobar que se borro 
``` json
 db.facturas.drop()
 show collections
```
9. Insertar un documento en una coleccion denominada **fabricantes** para probar los sub documentos y la clave _id personalizada

| Codigo   | Valor   |
|-------------|-------------|
| id | 1 |
| Nombre | fab1 |
| Localidad | ciudad: buenos aires, pais:argentina,Calle:Calle Pez 27, cod_postal:2900 |

``` json
 db.fabricantes.insertOne(
 {
 _id:1,
 Nombre:'fab1',
 Localidad:{
 ciudad:'Buenos Aires',
 Pais:'Argentina',
 Calle:'Calle pez 27',
 cod_postal:2900
 }
 }
 )
```
10. Realizar una insercion de varios documentos en la coleccion productos

| Codigo   | Valor   |
|-------------|-------------|
| Cod_producto | 3 |
| Nombre | Alicates|
| precio | 10 |
| unidades | 25 |
| Fabricantes | fab1, fab2. fab3, fab4 |

| Codigo   | Valor   |
|-------------|-------------|
| Cod_producto | 4 |
| Nombre | Arandela|
| precio | 1 |
| unidades | 500 |
| Fabricantes | fab2. fab3, fab4 |

```json
 db.libros.insertMany([{
    _id: 1,  
    titulo: 'El aleph',
    autor: 'Borges',
    editorial: 'Planeta',
    precio: 20,
    cantidad: 50 
  }
,
  {
    _id: 2,  
    titulo: 'Martin Fierro',
    autor: 'Jose Hernandez',
    editorial: 'Siglo XXI',
    precio: 50,
    cantidad: 12
  }
,
  {
    _id: 3,  
    titulo: 'Aprenda PHP',
    autor: 'Mario Molina',
    editorial: 'Siglo XXI',
    precio: 50,
    cantidad: 20
  }
,
  {
    _id: 4,  
    titulo: 'Java en 10 minutos',
    editorial: 'Siglo XXI',
    precio: 45,
    cantidad: 1 
  }
  ,{
    _id: 5,  
    titulo: 'Python para torpes',
    editorial: 'Planeta',
    precio: 25,
    cantidad: 14
  } 
  ,
  {
    _id: 6,  
    titulo: 'Don quijote',
    editorial: 'Biblio',
    precio: 25,
    cantidad: 14
  }
  ,
  {
    _id: 7,  
    titulo: 'MongoDB para expertos',
    editorial: 'Biblio',
    precio: 34,
    cantidad: 29
  }
  ,
  {
    _id: 8,  
    titulo: 'MongoDB para novatos',
    editorial: 'Biblio',
    precio: 15,
    cantidad: 12
  }
  ,
  {
    _id: 9 ,
    titulo: 'JSON para todos',
    editorial: 'Planeta',
    precio: 25,
    cantidad: 15
  }
 ]
)
```