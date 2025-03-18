# Agregaciones en MongoDB (Framework)

## Metodos para realizar agregaciones simples
- distinct():Devuelve valores no duplicados
- countDocuments():Cuenta los docuemnros em una coleccion
- estimatedDocuments():Cuenta de manera aproximada dirante un periodo de tiempo

## Una Aggregation pipeline conta de una o mas etapas(stage) que procesan documentos

1. Cada etapa realiza una operacion en los documentos de entrada. Por ejemplo, una fase de filtrar documentos, agrupar docuemntos y
calcula valores
2. Los documentos que se generen en na fase pasan a la siguiente fase.
3. Puede devolver resultados para grupos de documentos como totales, maximo, minimo, etc

### Se utilia la clausula "aggregate"

- Existe una serie de operadores qjue se pueden utilizar para realiar operaciones. Se tiene distintos:
etapa de comparacion, booleanos, aritmeticos, de cadena etc.

1. contar los documentos de la coleccion libros 
```json
db.libros.countDocuments({})
```

2. Contar los documentos de la editorial Terra
```json
db.libros.countDocuments({editorial:{$eq:'Terra'}})
```
3. Seleccionar o mostar todos los libros mostrando solo la editorial
```json
db.libros.find({},{_id:0,editorial:1})
```
4. Mostrar todos los libros distintos a editorial
```json
db.libros.distinct('editorial')
```
[Documentacion de Agregacines](https://www.mongodb.com/docs/manual/aggregation/)

## $match Una pipeline basica
## Tienen funciones de etapa
```json
db.libros.aggregate(
[
    { 
  $match:{editorial:"Terra"}
  }
]
)
```

## $project. Incluir y renombrar campos
```json
db.libros.aggregate(
[
  {
    $match:{editorial:'Terra'}
  },
  {
    $project:{
      _id:0,
      titulo:1,
      precio:1,
      NombreEditorial:"$editorial",
      editorial:1
    }
  }
]
)
```

## $sort. ordenaciones
```json
[
  {
    $match:
      /**
       * query: The query in MQL.
       */
      {
        editorial: "Terra"
      }
  },
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
      {
        _id: 0,
        titulo: 1,
        precio: 1,
        cantidad: 1,
        "Nombre de la editorial": "$editorial",
        "Total de ganancias": {
          $multiply: ["$precio", "$cantidad"]
        }
      }
  },
  {
    $sort:
      /**
       * Provide any number of field/order pairs.
       */
      {
        precio: 1
      }
  }
]
```
## #group. Agrupaciones
[Agrupaciones](https://www.mongodb.com/docs/manual/reference/operator/aggregation/group/)

- Cuantos documentos hay por cada una de las editoriales

```json
[
  {
    $group:
      {
        _id: "$editorial",
        "Numero Documento": {
          $count: {}
        }
      }
  }
]
```

-- Cuantos documentos hay por cada una de las editoriales por numero de documentos de manera descendente





-- Utilizando Mongo Atlas base de datos sample_airbnb
-- Agrupar por tipo de propiedad, mostrando el numero de propiedades y 
-- el numero de sus precios
```json
{
  _id: '$property_type',
  Numero: {
    $count: {}
  },
  Media:
    {
      $avg:'price'
    }
}
```

- Operadores $set
```json
[
  {
    $group: {
      _id: "$property_type",
      Numero: {
        $count: {}
      },
      Media: {
        $avg: "$price"
      }
    }
  },
  {
    $set:
      /**
       * field: The field name
       * expression: The expression.
       */
      {
        Media_Total: {
          $trunc: "Media"
        }
      }
  }
]
```
```json
[
  {
    $group: {
      _id: "$property_type",
      Numero: {
        $count: {}
      },
      Media: {
        $avg: "$price"
      }
    }
  },
  {
    $set:
      /**
       * field: The field name
       * expression: The expression.
       */
      {
        Media_Total: {
          $trunc: "Media"
        }
      }
  },
  {
    $unset:
      /**
       * Provide the field name to exclude.
       * To exclude multiple fields, pass the field names in an array.
       */
      ["Media", "Media_Total"]
  }
]
```

-- Quitando solo un campo
```json
[
  {
    $group: {
      _id: "$property_type",
      Numero: {
        $count: {}
      },
      Media: {
        $avg: "$price"
      }
    }
  },
  {
    $set:
      /**
       * field: The field name
       * expression: The expression.
       */
      {
        Media_Total: {
          $trunc: "Media"
        }
      }
  },
  {
    $unset:
      /**
       * Provide the field name to exclude.
       * To exclude multiple fields, pass the field names in an array.
       */
      "Media"
  }
]
```
-- Creando nuevas colecciones utilizadno el operador $out
-- Nota: Debe ser el ultimo en nuestra agregacion 

- Ejemplosn con operadores de comparacion y logicos
```json
[
  {
    $project: {
      _id: 0,
      price: 1,
      room_type: 1,
      caro: {
        $gte: ["$price", 300]
      },
      medio: {
        $and: [
          {
            $gte: ["$price", 100]
          },
          {
            $lte: ["$price", 300]
          }
        ]
      },
      economico: {
        $lte: ["$price", 100]
      }
    }
  }
]
```
- $cond. Devuelve valores segun una condicion(es parecido a un operador ternario de un lenguaje de programacion)
```json
[
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
      {
        _id: 0,
        price: 1,
        name: 1,
        room_type: 1,
        caro: {
          $cond: [
            {
              $gt: ["$price", 300]
            },
            "Si",
            "NOOOOO."
          ]
        },
        medio: {
          $cond: [
            {
              $and: [
                {
                  $gte: ["$price", 100]
                },
                {
                  $lte: ["$price", 300]
                }
              ]
            },
            "Si",
            "No"
          ]
        },
        barato: {
          $cond: [
            {
              $lt: ["$price", 100]
            },
            "Si",
            "No"
          ]
        }
      }
  }
]
```
- Views
```json
db.createView("ganacias_libros",
  [
  {
    $match:
      /**
       * query: The query in MQL.
       */
      {
        editorial: "Biblio"
      }
  },
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
      {
        _id: 0,
        titulo: 1,
        precio: 1,
        cantidad: 1,
        "Nombre de editorial": "$editorial",
        "Total de ganancias": {
          $multiply: ["$precio", "$cantidad"]
        }
      }
  },
  {
    $sort:
      /**
       * Provide any number of field/order pairs.
       */
      {
        "Total de ganancias": -1
      }
  }
]
)
```
