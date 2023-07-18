Los genéricos son una característica de TypeScript que te permiten crear funciones y clases que pueden trabajar con diferentes tipos de datos. Esto puede ser muy útil para escribir código más general y reutilizable.

Por ejemplo, supongamos que quieres crear una función que pueda ordenar una lista de números. Sin genéricos, tendrías que escribir dos funciones diferentes: una para ordenar números y otra para ordenar cadenas. Con genéricos, puedes escribir una sola función que puede ordenar cualquier tipo de datos.

Aquí hay un ejemplo de una función genérica para ordenar una lista de datos:

```ts
function sort<T>(list: T[]): T[] {
  // Ordena la lista
  return list.sort();
}
```
Esta función puede ser llamada con cualquier tipo de datos. Por ejemplo, puedes llamarla para ordenar una lista de números, una lista de cadenas o una lista de objetos.

Los genéricos también se pueden usar para crear clases genéricas. Por ejemplo, supongamos que quieres crear una clase que pueda almacenar cualquier tipo de datos. Puedes usar genéricos para crear una clase como esta:

```ts
class Stash<T> {
  private data: T;

  constructor(public data: T) {}
}
```

Esta clase puede ser instanciada con cualquier tipo de datos. Por ejemplo, puedes crear una instancia de esta clase para almacenar un número, una cadena o un objeto.

Los genéricos son una característica poderosa de TypeScript que te permiten escribir código más general y reutilizable.
Aquí hay algunos ejemplos más de cómo se pueden usar genéricos en TypeScript:

    Para crear funciones que puedan trabajar con diferentes tipos de datos.
    Para crear clases que puedan almacenar cualquier tipo de datos.
    Para crear interfaces que puedan ser implementadas por diferentes tipos de clases.
    Para crear uniones que puedan contener diferentes tipos de datos.


   Por ejemplo, la siguiente función puede tomar cualquier tipo de colección y devolver el valor máximo de esa colección:
```ts
function max<T>(collection: T[]): T {
  let maxValue: T = collection[0];
  for (let i = 1; i < collection.length; i++) {
    if (collection[i] > maxValue) {
      maxValue = collection[i];
    }
  }
  return maxValue;
}
```