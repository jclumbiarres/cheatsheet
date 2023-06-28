# Embedding
---

En una base de datos relacional, la información se guarda individualmente en su tabla asignada, luego se enlaza mediante los foreign keys. MongoDB soporta referencias de un documento a otro, incluído joins de varios documentos, pero es un error, ya que estamos usando una base de datos NoSQL basada en documentos.

Por ejemplo, la siguiente estructura tiene un usuario y una o varias direcciones. Una forma de estructurarlo sería usar referencias para relacionar ambas entidades.

```
> db.user.findOne()
{
    _id: 111111,
    email: “email@example.com”,
    name: {given: “Jane”, family: “Han”},
}

> db.address.find({user_id: 111111})
{
    _id: 121212,
    street: “111 Elm Street”,
    city: “Springfield”,
    state: “Ohio”,
    country: “US”,
    zip: “00000”
}
```

Normalmente esa información la necesitas al mismo tiempo que la de usuario, en una base de datos basada en documentos es mucho mas simple integrar (embed) la dirección directamente en el documento del usuario:

```
> db.user.findOne({_id: 111111})
    
{    
    _id: 111111,    
    email: “email@example.com”,    
    name: {given: “Jane”, family: “Han”},    
    address: {    
    street: “111 Elm Street”,    
    city: “Springfield”,    
    state: “Ohio”,    
    country: “US”,    
    zip: “00000”,    
    }    
}
```

Así solo con una llamada (query) tienes toda la información, puedes acceder al subdocumento directamente.

Con un multiples direcciones sería de la siguiente forma:

```
> db.user.findOne({_id: 111111})
    
{    
    _id: 111111,    
    email: “email@example.com”,    
    name: {given: “Jane”, family: “Han”},    
    addresses: [    
    {    
    label: “Home”,    
    street: “111 Elm Street”,    
    city: “Springfield”,    
    state: “Ohio”,    
    country: “US”,    
    zip: “00000”,    
    },    
    {label: “Work”, ...}    
    ]    
}
```

Incluso puedes actualizar individualmente las direcciones usando el operador posicional:

```
> db.user.update(    
    {_id: 111111,    
    “addresses.label”: “Home”},    
    {$set: {“addresses.$.street”: “112 Elm Street”}}    
    )
```

Como se puede ver hay que entrecomillar cualquier llamada (query) o acutalización (update) para que la sintaxis sea correcta. La parte de la llamada (query) que se va a actualizar necesita incluir una array de lo que se está actualizando, la actualización se aplicará al primer elemento que coincida en la array. En este caso la dirección "Home" se actualizaría con address a "112 Elm Street".

---

## Cuando deberías usar embed o referencias.

Hacer documentos embeddeds es una forma eficiente de guardar datos relacionados, especialmente cuando siempre se acceden juntos. En general los esquemas (schema) que uses en MongoDB al contrario que MySQL se debería usar el embedding por defecto, y luego usar referencias en el lado de la aplicación o del lado de MongoDB solo cuando sean realmente necesarios. Cuanto más a menudo una carga de trabajo determinada pueda recuperar un solo documento y tener todos los datos necesarios, mejor rendimiento tendrá la aplicación.

Hay varios patrones de embedding.

## Embedded Document Pattern

El patrón de diseño general y preferible es integrar cualquier substructura compleja en los documentos que serán usados. La regla es:

*Lo que siempre llames junto, guardalo junto*

## Embedded Subset Pattern

En casos hibridos el subset pattern es útil cuando tienes una colección separada con una lista larga de items relacionados, pero quieres poder llamar a algunos de esos items de forma facil para mostrarlo al usuario. Un ejemplo:

```
> db.movie.findOne()
    
{    
    _id: 333333,    
    title: “The Big Lebowski”
}
        
> db.review.find({movie_id: 333333})
    
{    
    _id: 454545,    
    movie_id: 333333,    
    stars: 3    
    text: “it was OK”    
}    

{    
    _id: 565656,    
    movie_id: 333333,    
    stars:5,    
    text: “the best”    
}    
...
```

Ahora imagina que tienes miles de reviews, pero solo quieres mostrar las dos mas recientes en una película. En este caso, es mejor guardar ese subset como una lista en el documento de la película.

```
> db.movie.findOne({_id: 333333})
    
{    
    _id: 333333,    
    title: “The Big Lebowski”,    
    recent_reviews: [    
    {_id: 454545, stars: 3, text: “it was OK”},    
    {_id: 565656, stars: 5, text: “the best”}    
    ]    
}
```

*Si accedes regularmente un subset de items relacionados, integra ese subset*

## Patrón de referencia extendida.

Otro caso hibrido es conocido como Extended Reference Pattern. Es algo similar a un patrón de subset, en el cual se optimiza para una cantidad pequeña de información accedida regularmente. En este en vez de usar una lista , usa una referencia de un documento a otro en la misma colección, tambien guarda otros campos de otro documento para su fácil acceso.

Ejemplo:

```
> db.movie.findOne({_id: 444444})
    
{    
    _id: 444444,    
    title: “One Flew Over the Cuckoo's Nest”,    
    studio_id: 999999,    
    studio_name: “Fantasy Films”    
}
```

Como se puede ver, se guarda el studio_id para usarlo como referencia, igualmente el nombre del estudio se ha copiado al documento para que sea mas facil para mostrarlo. Si estas integrando informacion de documentos que cambian regularmente, necesitas recordar de actualizar los documentos que has copiado la información que ha cambiado.

*Si accedes a pocos campos de un documento referenciado, integra esos campos*
