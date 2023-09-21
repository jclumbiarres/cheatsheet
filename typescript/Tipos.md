# Tipos en TypeScript
---
En TypeScript, los tipos son una forma de especificar qué tipo de datos se espera que se almacenen en una variable o se pasen como argumento a una función. Los tipos básicos incluyen **number**, **string**, **boolean**, **null**, **undefined**, **object**, **symbol**, **unknown**, **any** y **void**. Además, TypeScript también tiene tipos más avanzados, como interface, enum, tuple, union, intersection y type alias, que permiten definir estructuras de datos más complejas. El uso de tipos en TypeScript ayuda a detectar errores en tiempo de compilación y mejora la legibilidad del código.

Un ejemplo básico:
```ts
let nombre: string;
nombre = "Joe Bananas"; // Todo correcto, es una string
nombre = 24; // ERROR
```
Type 'number' is not assignable to type 'string'.ts(2322)
   
   Sería el error que mostraría el LSP, avisando que no puedes asignar un tipo número a una variable de tipo string.

## Tipos básicos.
---
### String   
   Tipo de texto o cadena de cáracteres
   Ejemplo: 
```ts
let nombre: string;
nombre = "Joe Bananas"
// Inferido
let nombreReal = "Joe Bananas" // Tipo string inferido en la declaración de la variable.
console.log(typeof nombreReal) // resultado: string.
```

### Number
Tipo para números enteros y coma flotante (Recordad que TODO number en JavaScript/TypeScript es un float64!)
Ejemplo:
```ts
let edad: number;
edad = 39;
let edadReal = 39;
console.log(typeof edadReal); // resultado: number.
```

### Boolean
Tipo booleano (verdadero o falso, 0 o 1)
```ts
let verdad: boolean;
verdad = true;
let verdadVerdadera = false; 
console.log(typeof verdadVerdadera); // resultado: boolean
```

### Any
Cualquier tipo, aceptaría number,string,boolean lo que sea, no se recomienda su uso, ya que básicamente pasa por encima del sistema de tipos.
```ts
let cosas: any;
cosas = "joe"; // string
cosas = 34; // number
cosas = true // boolean
```
**NO** se recomienda usar.

### Unknown
Tipo desconocido, se recomienda usarlo si no se sabe el valor, es similar al any, pero con una restricción adicional: **NO** se puede hacer nada con un valor de tipo unknown hasta que se haya verificado su tipo. Esto significa que no se puede asignar un valor de tipo unknown a una variable de otro tipo sin antes verificar su tipo.
```ts
let valor: unknown;
valor = 5;
valor = "hola";

let longitud: number = valor.length; // Error: propiedad 'length' no existe en tipo 'unknown'

if (typeof valor === "string") {
  let longitud: number = valor.length; // OK
}
```

### Object
En TypeScript, el tipo object se refiere a cualquier valor que no sea de tipo primitivo (number,string,boolean,null,undefined,symbol). Esto incluye objetos literales, arreglos, funciones y cualquier otro objeto que se pueda crear con la sintaxis de JavaScript. Aunque object es un tipo amplio que incluye muchos tipos diferentes, no se puede acceder a las propiedades o métodos específicos de un objeto tipo object sin antes verificar su tipo
```ts
let persona: object = { nombre: "Juan", edad: 30 };
let arreglo: object = [1, 2, 3];
let funcion: object = function() { console.log("Hola"); };
///
let persona: object = { nombre: "Juan", edad: 30 };
console.log(persona.nombre); // Error: propiedad 'nombre' no existe en tipo 'object'

if (typeof persona === "object" && persona !== null) {
  console.log(persona.nombre); // OK
}
```

### Null

En TypeScript, el tipo null se refiere a un valor nulo o vacío. Se utiliza para indicar que una variable o propiedad no tiene un valor asignado intencionalmente. 
Un Ejemplo:
```ts
let valor: string | null = null;
let persona: { nombre: string, edad: number | null } = { nombre: "Juan", edad: null };
```
En este ejemplo, la variable valor y la propiedad edad de la variable persona se han asginado explícitamente como null. El tipo null se puede combinar con otros tipos utilizando la sintaxis de unión (|) para indicar que una variable o propiedad puede tener un valor de ese tipo o null.
Ejemplo:
```ts
let valor: string | null = "hola";
valor = null;

function obtenerNombre(): string | null {
  return null;
}
```
En el ejemplo, la función obtenerNombre devuelve un valor de tipo string o null. El uso del tipo null puede ayudar a evitar errores en tiempo de ejecución al indicar explícitamente que una variable o propiedad puede no tener un valor asignado.

### Undefined

En TypeScript, el tipo undefined se refiere a un valor que no está definido o no está asignado. Se utiliza para indicar que una variable o propiedad no tiene un valor asignado intencionalmente o que su valor es desconocido. 
Ejemplo:
```ts
let valor: string | undefined;
let persona: { nombre: string, edad?: number } = { nombre: "Juan" };
```
En este ejemplo, la variable valor y la propiedad edad de la variable persona no tienen un valor asignado intencionalmente, por lo que su valor es undefined. El tipo undefined se puede combinar con otros tipos utilizando la sintaxis de unión (|) para indicar que una variable o propiedad puede tener un valor de ese tipo o undefined.
Ejemplo:
```ts
let valor: string | undefined = "hola";
valor = undefined;

function obtenerEdad(): number | undefined {
  return undefined;
}
```
### Symbol

En TypeScript, el tipo symbol se refiere a un valor único e inmutable que se utiliza como clave de propiedad en objetos. Los símbolos se crean utilizando la función Symbol() y se pueden utilizar para crear propiedades de objeto que no se pueden acceder de otra manera.
```ts
let simbolo1 = Symbol();
let simbolo2 = Symbol("descripcion");

let objeto = {
  [simbolo1]: "valor1",
  [simbolo2]: "valor2"
};

console.log(objeto[simbolo1]); // "valor1"
console.log(objeto[simbolo2]); // "valor2"
```
En este ejemplo, se crean dos símbolos utilizando la función Symbol(). Luego, se utiliza la sintaxis de corchetes para crear propiedades de objeto utilizando los símbolos como claves. Los símbolos se pueden utilizar para crear propiedades de objeto que no se pueden acceder de otra manera, lo que los hace útiles para crear propiedades privadas o protegidas. El tipo symbol se utiliza principalmente en el contexto de objetos y no se utiliza comúnmente en otros tipos de datos.

### Void

El tipo void se refiere a la ausencia de un valor. Se utiliza para indicar que una función no devuelve ningún valor o que una variable no tiene un valor asignado.
Ejemplo:
```ts
function imprimirMensaje(): void {
  console.log("Hola");
}

let resultado: void = undefined;
```
En este ejemplo, la función imprimirMensaje no devuelve ningún valor, por lo que su tipo de retorno es void. La variable resultado se ha asignado explícitamente como undefined, que es el valor que se utiliza para indicar que una variable no tiene un valor asignado. El tipo void se utiliza principalmente en el contexto de funciones y no se utiliza comúnmente en otros tipos de datos.
