# TypeScript (JavaScript con sintaxis para tipos)

![Manintained](https://img.shields.io/badge/Maintained%3F-yes-green.svg)
![GitHub last commit (master)](https://img.shields.io/github/last-commit/dgrobinson0/TypeScript)
![Starts](https://img.shields.io/github/stars/dgrobinson0/TypeScript.svg)

<img src="/images/typescript-logo.png" />

# ¿Qué es TypeScript?
TypeScript (creado por Microsoft en 2012) es un lenguaje de programación fuertemente tipado que se basa en JavaScript.

<img src="/images/typescript.png" />

> [!NOTE]
> Mientras que JavaScript es un lenguaje de programación con tipado débil y dinámico, TypeScript es un lenguaje de programación con tipado fuerte y estático.
>
> Un lenguaje con tipado débil y dinámico permite cambios y conversiones automáticas de tipo en tiempo de ejecución, priorizando flexibilidad, mientras que uno con tipado fuerte y estático exige consistencia de tipos desde la compilación, priorizando seguridad y detección temprana de errores.
>
> ```
> // tipado débil y dinámico
> // débil porque no es necesario declarar el tipo de variable (string ,number, etc).
> // dinámico porque podemos cambiar los tipos de una variable, por ejemplo de string a number.
> let a = 'hola'   //string
> a = 2            //number
> ```

> [!IMPORTANT]
> TypeScript no funciona en tiempo de ejecución. Al compilar código TypeScript este se convierte en JavaScript para ejecutarse en el navegador ya que el navegador no entiende TypeScript.
> 
> TypeScript no te ahorra código comparado con JavaScript, pero añade seguridad y rebustez.

# Tipos de datos en TypeScript

Tipos primitivos (básicos)
```
// number: Para números enteros y decimales.   
let edad: number = 25;
``` 
```
// string: Para cadenas de texto.
let nombre: string = "Ana";
```
```
// boolean: Valores lógicos true o false.   
let activo: boolean = true;
```
```
// undefined: Valor por defecto de variables no inicializadas.
// null: Representa la ausencia intencionada de valor.
```
```
// symbol: Valores únicos e inmutables (introducidos en ES6).   
let id: symbol = Symbol("id");
```
```
// bigint: Para números enteros muy grandes (ES2020+).   
let granNumero: bigint = 123n;
```

Tipos compuestos
```
// object: Cualquier valor que no sea primitivo (objetos, arrays, funciones, etc.).   
let persona: object = { nombre: "Luis" };
```
```
// array: Listas de elementos del mismo tipo.   
let numeros: number[] = [1, 2, 3];
// o
let letras: Array<string> = ["a", "b"];
```
```
// tuple (tupla): Array con número fijo de elementos y tipos específicos.   
let coordenada: [number, number] = [10, 20];
```

Tipos especiales 
```
// any: Desactiva la verificación de tipos (se debería evítar en código seguro).   
let valor: any = "hola";
valor = 42;
```
```
// unknown: Tipo seguro para valores de tipo desconocido (requiere verificación antes de usar).
let dato: unknown = "hola";
// dato.toUpperCase(); ❌ error: no se puede usar sin verificar
```
```
// void: Usado principalmente en funciones que no devuelven valor.   
function saludar(): void {
  console.log("Hola");
}
```

Tipos definidos por el usuario 
```
// enum: Conjunto de constantes con nombre.   
enum Color { Rojo, Verde, Azul }
let c: Color = Color.Verde;
```
```
// type: Alias de tipos personalizados (incluyendo uniones, intersecciones, etc.)
type ID = string | number;
```
```
// interface: Define la forma de un objeto (similar a type, pero con diferencias técnicas).   
interface Persona {
  nombre: string;
  edad: number;
}
```

> [!NOTE]
> Recomendado permitir que TypeScript determine automáticamente el tipo de una variable definida mediante la inferencia.
>
> ```
> let a: number = 2    // 
> let a = 2            // recomendado
> ```

# Inferencia de tipos en TypeScript

La inferencia de tipos en TypeScript es la capacidad del compilador para determinar automáticamente el tipo de una variable, expresión o función basándose en su valor inicial o contexto, sin necesidad de anotarlo explícitamente. 

```
let mensaje = "Hola, mundo!";
// TypeScript infiere que `mensaje` es de tipo `string`
```

En este caso, aunque no escribimos : string, TypeScript infirió que mensaje es un string porque se inicializó con un valor de ese tipo.

# Funciones en TypeScript

```
// agregar el tipo de variable en los parámetros de la función
function sumar(a: number, b: number) {
  return a + b;
}
```
```
function imprimir(mensaje: string) {
  console.log(`Hola ${mensaje}`);
}
imprimir("mundo")
```
```
// arrow function
const imprimir = (mensaje: string) => {
  console.log(`Hola ${mensaje}`);
}
imprimir("mundo")

// tipado arrow function - Evitar usar, dejar que TypeScript infiera
const imprimir = (mensaje: string): void => {
  console.log(`Hola ${mensaje}`);
}
imprimir ("mundo")
```
```
// funciones utilizando const que recibe por parámetros otras funciones
// Al pasar funciones como parámetro seguir la estructura obtenerHello: () => string
const imprimir = (obtenerHello: () => string, obtenerWorld: () => string, mensaje: string ) => {
  const mensaje1 = obtenerHello()
  const mensaje2 = obtenerWorld()
  console.log(`Hola ${mensaje}, ${mensaje1} ${mensaje2}`);  
}

const obtenerHello = () => {
    return "Hello"
}

const obtenerWorld = () => {
    return "World"
}

imprimir(obtenerHello, obtenerWorld, "mundo")
```

# Objetos en TypeScript
Un objeto es una colección de pares clave-valor (también llamados propiedades). En TypeScript, además, cada propiedad del objeto tiene un tipo definido.
```
const Persona = {       |      interface Persona = {       |     type Persona = {
  nombre: "Ana",        |        nombre: "Ana",            |       nombre: "Ana",
  edad: 30,             |        edad: 30,                 |       edad: 30, 
  activo: true          |        activo: true              |       activo: true
}                       |      }                           |     }
```
> [!NOTE]
> Tanto type como interface en TypeScript se usan para definir la forma de los objetos, pero tienen diferencias clave en comportamiento, flexibilidad y uso recomendado.
>
> 1. **Extensibilidad**: 
>
> ✅ **interface:** Se puede extender con extends o reabrir (declaración merging)
> ```
> interface Animal {
>   nombre: string;
> }
> interface Perro extends Animal {
>   raza: string;
> }
> // También puedes "reabrir" una interfaz (útil en librerías):
> interface Animal {
>   edad: number; // Ahora Animal tiene nombre + edad
> }
> ```
> ❌ type: No se puede reabrir. Solo se puede extender usando intersecciones (&):
>```
> type Animal = {
>   nombre: string;
> };
> type Perro = Animal & {
>   raza: string;
> };
> // ❌ Esto NO es posible:
> // type Animal = { edad: number; } // Error: duplicado
>```
>📌 Ventaja de interface: Ideal para librerías o cuando necesitas "agregar" propiedades más tarde.
>
>  2. **Flexibilidad de tipos:**
> 
> ✅ **type:** Puede representar cualquier tipo, no solo objetos:
>```
> type ID = string | number;           // Unión
> type Punto = [number, number];       // Tupla
> type Callback = (x: number) => void; // Función
> type Nombre = string;                // Alias simple
>```
>❌ **interface:** Solo puede describir objetos (o tipos de función, pero es raro y limitado):
> ```
> // ❌ Esto NO se puede hacer con interface:
> // interface ID = string | number; // Error de sintaxis
> ```
> 📌 Ventaja de **type**: Es mucho más versátil para tipos complejos. 
     
     

