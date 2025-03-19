# Agregaciones

1. Para hacer esta práctica vamos a cargar unos datos ficticios de empresas.
2. Tienes un fichero denominado “productos.json”
3. Debes poner el resultado de las consultas en cada apartado

- Cuenta los productos de tipo “medio”, usando un método básico
```json
db.productos.count({ tipo: "medio" })
```
- Indicar con un distinct, las empresas (fabricantes) que hay en la colección
```json
db.productos.distinct("fabricante");
```
- Usando aggregate, visualizar los productos que tengan más de 80 unidades
```json
db.productos.aggregate(
[
  {
    $match:
      /**
       * query: The query in MQL.
       */
      {
        unidades: {
          $gt: 80
        }
      }
  }
]
)
```
- Con $project visualizar solo el nombre, unidades y precio de los productos que tengan menos de 10 unidades
```json
db.productos.aggregate(
[
  {
    $match:
      /**
       * query: The query in MQL.
       */
      {
        unidades: {
          $lt: 10
        }
      }
  },
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
      {
        nombre: 1,
        unidades: 1,
        precio: 1
      }
  }
])
```
- Con $project ponemos el fabricante pero le cambiamos el nombre por “empresa”. Usamos el mismo comando anterior
```json
db.productos.aggregate(
[
  {
    $match:
      /**
       * query: The query in MQL.
       */
      {
        unidades: {
          $lt: 10
        }
      }
  },
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
      {
        empresa: "$fabricante",
        unidades: 1,
        precio: 1
      }
  }
]
)

```
- Añadir a la consulta anterior un campo calculado que se llame total y que multiplique precio por unidades.
```json
db.productos.aggregate(
[
  {
    $match:
      /**
       * query: The query in MQL.
       */
      {
        unidades: {
          $lt: 10
        }
      }
  },
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
      {
        empresa: "$fabricante",
        unidades: 1,
        precio: 1,
        total: {
          $multiply: ["$precio", "$unidades"]
        }
      }
  }
]
)

```
- Hacer que el nombre salga en mayúsculas con el operador $toUpper
```json
db.productos.aggregate(
[
  {
    $match:
      /**
       * query: The query in MQL.
       */
      {
        unidades: {
          $lt: 10
        }
      }
  },
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
      {
        empresa: "$fabricante",
        nombre: {
          $toUpper: "$nombre"
        },
        unidades: 1,
        precio: 1,
        total: {
          $multiply: ["$precio", "$unidades"]
        }
      }
  }
]
)

```
- Añadir un campo calculado que ponga el nombre del producto y el tipo concatenado con el operador $concat. Le llamamos al campo “completo”
```json
db.productos.aggregate(
[
  {
    $match:
      /**
       * query: The query in MQL.
       */
      {
        unidades: {
          $lt: 10
        }
      }
  },
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
      {
        empresa: "$fabricante",
        nombre: {
          $toUpper: "$nombre"
        },
        tipo: 1,
        completo: {
          $concat: ["$nombre", " ", "$tipo"]
        },
        unidades: 1,
        precio: 1,
        total: {
          $multiply: ["$precio", "$unidades"]
        }
      }
  }
]
)

```
- Ordena el resultado por el campo “total”
```json
db.productos.aggregate(
[
  {
    $match:
      /**
       * query: The query in MQL.
       */
      {
        unidades: {
          $lt: 10
        }
      }
  },
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
      {
        empresa: "$fabricante",
        nombre: {
          $toUpper: "$nombre"
        },
        tipo: 1,
        completo: {
          $concat: ["$nombre", " ", "$tipo"]
        },
        unidades: 1,
        precio: 1,
        total: {
          $multiply: ["$precio", "$unidades"]
        }
      }
  },
  {
    $sort:
      /**
       * Provide any number of field/order pairs.
       */
      {
        total: 1
      }
  }
]
)

```
- Haciendo una nueva consulta, averiguar el numero de productos por tipo de producto
```json
db.productos.aggregate(
[
  {
    $group:
      /**
       * _id: The id of the group.
       * fieldN: The first field name.
       */
      {
        _id: "$tipo",
        count: {
          $sum: 1
        }
      }
  }
]
)
```
- Añadir el valor mayor y el menor
```json
db.productos.aggregate(
[
  {
    $group:
      /**
       * _id: The id of the group.
       * fieldN: The first field name.
       */
      {
        _id: "$tipo",
        count: {
          $sum: 1
        },
        maxPrecio: {
          $max: "$precio"
        },
        minPrecio: {
          $min: "$precio"
        }
      }
  }
]
)
```
- Añade el total de unidades por cada tipo
```json
db.productos.aggregate(
[
  {
    $group:
      /**
       * _id: The id of the group.
       * fieldN: The first field name.
       */
      {
        _id: "$tipo",
        count: {
          $sum: 1
        },
        maxPrecio: {
          $max: "$precio"
        },
        minPrecio: {
          $min: "$precio"
        },
        totalUnidades: {
          $sum: "$unidades"
        }
      }
  }
]
)
```
- Con el operador $set y el operador “$substr” visualiza todos los datos del producto "Small Metal Tuna" y los primeros 5 caracteres del nombre.
```json
db.productos.aggregate(
[
  {
    $match:
      /**
       * _id: The id of the group.
       * fieldN: The first field name.
       */
      {
        nombre: "Small Metal Tuna"
      }
  },
  {
    $set:
      /**
       * field: The field name
       * expression: The expression.
       */
      {
        primerosCinco: {
          $substr: ["$nombre", 0, 5]
        }
      }
  }
]
)
```
- Creamos una salida que tenga el nombre del articulo y el total (precio por unidades) y lo guardamos en una colección denominada productos2
```json
db.productos.aggregate(
[
    { $project: { nombre: 1, total: { $multiply: ["$precio", "$unidades"] } } },
    { $out: "productos2" }
]
)
```
- Comprobamos que se ha creado
```json
show collections
```
- Hacemos un find para comprobar el resultado
```json
db.productos2.find({})
```
- Usando $cond y $project vamos a visualizar el nombre del producto, el precio y un campo llamado valoración que ponga “barato” si el precio es menor de 250 y caro si es mayor o igual
```json
db.productos.aggregate(
[
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
      {
        nombre: 1,
        precio: 1,
        valoración: {
          $cond: [
            {
              $lt: ["$precio", 250]
            },
            "barato",
            "caro"
          ]
        }
      }
  }
]
)
```