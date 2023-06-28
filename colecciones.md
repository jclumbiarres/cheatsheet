# Colecciones MongoDB


MongoDB guarda los datos como "documentos" (especificamente BSON) los cuales son guardados dentro de una colección, una base de datos puede guardar una o más conexiones.

## Bases de datos

En MongoDB una base de datos puede guardar una o mas colecciones.
Para acceder a una base de datos desde la shell de mongo (mongosh)
```mongo
use BaseDeDatos
```


## Inserción en colecciones.

Una vez creada la base de datos podrás insertar información en las colecciones.
ejemplo:
```
use BaseDeDatos
db.MiColeccion.insertOne(
    {nombre: "Joe Bananas"}
)
```
Si la colección no existe MongoDB la creará por tí, recuerda que es una colección schemeless (no hay un esquema definido).

En caso de querer crearla manualmente y tener un schema se puede hacer a mano (recomendable cuando sabes lo que quieres en la tabla)
Ejemplo sacado de la documentación de MongoDB:
```
db.createCollection( <name>,
   {
     capped: <boolean>,
     timeseries: {                  // Desde MongoDB 5.0
        timeField: <string>,        // Necesario para time series.
        metaField: <string>,
        granularity: <string>
     },
     expireAfterSeconds: <number>,
     clusteredIndex: <document>,    // Desde MongoDB 5.3
     changeStreamPreAndPostImages: <document>,  // Desde MongoDB 6.0
     size: <number>,
     max: <number>,
     storageEngine: <document>,
     validator: <document>,
     validationLevel: <string>,
     validationAction: <string>,
     indexOptionDefaults: <document>,
     viewOn: <string>,
     pipeline: <pipeline>,
     collation: <document>,
     writeConcern: <document>
   }
)
```

Estas son las opciones que hay dentro de una colección de MongoDB. En la 
siguiente tabla hablaremos de los parametros y tipos con una descripción de lo que hace.

| Parametro | Tipo | Descripción |
|---|---|---|
| name | string | El nombre de la colección que vayas a crear |
| options | document | Opcional. Opciones de configuración para crear: capped collection, clustered collection y view |

Ahora veremos unas pocas opciones que pueden ser útiles en ciertos casos.

## Crear una colección capped

Una colección capped como su nombre indica está limitada. Si intentas escribir mas información de lo que acepta la colección dará un error de threshold limit excedeed.

```
db.createCollection("log", { capped : true, size : 5242880, max : 5000 } )
```
Este comando crearía una colección llamada log con un máximo de 5 megabytes y un máximo de 5000 documentos

## Crear una colección con fecha de caducidad.

Para crear una time series collection (caducidad) de ejemplo que guarde la información del tiempo y pasado 24 horas se borraría la información.

```
db.createCollection(
    "weather24h",
    {
       timeseries: {
          timeField: "timestamp",
          metaField: "data",
          granularity: "hours"
       },
       expireAfterSeconds: 86400
    }
)
```

* metaField indica el nombre del campo que contiene la metadata de cada documento. metaField sirve como un un título o tag que permite las colecciones time series tener un identificador único. Este campo nunca, o muy raramente se cambiaría.

* granularity granularidad indica el tiempo entre documentos con un metaField existente, por defecto está en segundos lo que indica una alta frecuencia de datos, se relaciona con el time series identificado en el metaField.

Por último puedes añadir una caducidad para eliminar la información después de X tiempo pasado.

* expireAfterSeconds es un campo que indica cuantos segundos pasados se borre la información

```mongo
db.createCollection("dowJonesTickerData", 
{ timeseries: { 
               timeField: "date", 
               metaField: "symbol",
               granularity: "minutes" } })
```