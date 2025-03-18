# Modelo de datos en MongoDb

- Modelos
    - Embebidos
    - Referenciados
- Modelos embebidos

**Uno a uno**

```json
db.departamentos.insertMany(
    [
        {
        _id:1,
        nombre:"Tecnologias de la informacion",
        responsable:{
            nombre:'Monico',
            apellidos:'Martin Perez'
        },
        descripcion:'ejemplo de departamento'
        },
        {
        _id:2,
        nombre:"Contabilidad",
        responsable:{
            nombre:'Raul',
            apellidos:'Lopez Hernandez'
        },
        descripcion:'ejemplo de departamento'  
        }
    ]
)
```

- Modelos referenciados
**Uno a uno**

```json
db.localidades.insertMany(
    [
        {
            _id:'BA',
            ciudad:'Buenos Aires',
            pais:'Argentina',
            poblacion:'16 millones',
            turismo:['edificios','tango','gastronomia','museos','parques'],
            direccion:'Avenida Tortolos',
            cod_departamento:1
        },
        {
            _id:'SA',
            ciudad:'Santiago',
            pais:'Chile',
            poblacion:'20 millones',
            turismo:['iglesias','vino','gastronomia','museos'],
            direccion:'Avenida Tortolos',
            cod_departamento:2
        }
    ]
)
```

```json
db.departamentos.aggregate(
    [
        {
            $lookup:{
                from:'localidades',
                localField:'_id',
                foreignField:'cod_departamento',
                as:'localidades'
            }
        }
    ]
)
```

```json
db.categorias.insertMany(
    [
        {
            _id:2,
            nombre:'Hogar'

        },
        {
            _id:2,
            nombre:'Tecnologia'
        }
    ]
)
```
```json
db.producto.insertMany(
    [
        {
            _id:'1',
            nombre:'Papel de baño',
            precio:20,
            existencia:2,
            categoria:'cod_categoria'
        },
        {
            _id:2,
            nombre:'Laptop de la nasa',
            precio:20000,
            existencia:4,
            categoria:'cod_categoria'
        }
    ]
)
```