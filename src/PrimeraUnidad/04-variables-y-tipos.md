# Variables

Por muy gracioso que suene, una de las primeras cosas que los programadores
buscan al aprender un nuevo lenguaje es la manera que tiene de declarar
variables y sus respectivos tipos, en este capítulo aprenderemos a declarar
variables y a manejar diferentes tipos de datos en Rust.

Así mismo discutiremos los tipos primitivos, cuando *"tipear"* una variable o
no, el alcance de las variables y otros tratos especiales que posee rust como la
*inmutabilidad*.

## Comentarios

En un escenario ideal el código debería auto-documentarse por medio de las
buenas prácticas como los nombres descriptivos de variables, un estilo de
código limpio y una estructura impecable, aun así, existen casos donde se
necesita de un elemento extra para explicar o justificar la estructura de una
o varias partes de nuestro programa.

Rust sigue la forma de comentar de nuestro querido lenguaje C, por lo tanto
tenemos dos formas de comentar líneas de código:

* `//` Comentarios con doble barra para una sola línea.

```rust,ignore
// Esto es un comentario y no se compilará
```

* `/* */` Comentarios del tipo bloque para múltiples líneas.

```rust,ignore
/*
Esto
es
un
comentario
largo
*/
```

El estilo recomendado por la guía oficial de Rust es utilizar el comentario de
doble barra, incluso para varias líneas en caso de necesitar explicar tu código.

Procura utilizar el comentario multilínea solamente para comentar código.

> rust-doc también ofrece comentarios para documentación utilizando tres barras
> `///`, estos son útiles en proyectos grandes que necesitarán de algún tipo de
> documentación oficial además, en estos comentarios podemos utilizar sintáxis
> markdown para mejorar la legibilidad de nuestra documentación.


## Asignación de variables

Hasta ahora hemos visto varios tipos de valores, hemos visto cadenas, números
enteros, etc. Pero no debemos de confundir los valores con los objetos ni las
variables.

Tendremos que dejar algo muy claro desde el principio, la asignación de
variables es diferente en Rust si lo comparamos con otros lenguajes, esto es
para preservar 3 carácteristicas que hacen especial al lenguaje:

* **Seguridad**
* **Eficiencia**
* **Concisión**

A diferencia de otros lenguajes (como *JavaScript* o *Ruby*), Rust necesita un
poco más de planeación al escribir código pues necesitarás dictar los tipos de
los parámetros de tus funciones, de sus valores de retorno y de tus variables.

Por lo tanto Rust es un lenguaje *"Estáticamente tipado"* o de
*"Tipado estático"* lo que significa que el compilador deberá conocer el tipo
de los datos antes del tiempo de ejecución, este método que viene de lenguajes
veteranos como C o C++ ha sido rediseñado en Rust, pues el compilador revisará
que todos los caminos posibles de ejecución usarán valores solo de manera consistente con sus tipos. Esto permite que Rust detecte muchos errores de programación antes de tiempo, y es crucial para garantizar de seguridad de Rust.

Por lo tanto, en lugar de utilizar un intérprete o un compilador en tiempo real
como lo harían lenguajes como *Julia* o *LuaJIT (Lua Just In Time Compiler)*,
Rust fue diseñado para utilizar algo llamado *"Compilación anticipada"* es decir
una traducción completa de todo tu programa a código máquina antes de que
comience a ejecutarse, por ello los tipos de datos que Rust utilizar ayudan al
compilador a elegir una representación adecuada en el bajo nivel. 

Cuando Rust compila el código fuente el ejecutable resultante solo contiene
objetos que tengan una dirección de memoria y un valor. Dichos objetos no poseen
un nombre definido, pero estos mismos objetos necesitan de un identificador en
el código fuente para poder referenciarlos más tarde.

En Rust necesitaremos de la palabra reservada `let` para declarar cualquier
variable, por ejemplo:

```rust,ignore
let edad = 20;
let nombre = "Rustaceans";
let flotante = 3.1415;
```

### Variables ¿inmutables?

Tus ojos no te están engañando, las variables en Rust son inmutables por
defecto, esto se hace para preservar la seguridad en Rust, pero esto no es el
fin del mundo, Rust nos ofrece la opción de mutar las variables a nuestro gusto
siempre y cuando cumplamos con ciertas reglas impuestas por el compilador.

> Rust no es el primer lenguaje en hacer esto, muchos lenguajes funcionales
> poseen esta característica. De hecho en los lenguajes 100% funcionales la
> mutabilidad de las variables no está permitida.

Veamos este pequeño ejemplo para ilustrar lo anterior:

<span><b>Ejemplo 1.0</b></span>
```rust,ignore
{{#include ../code/rust/inmutvars.rs}}
```

El siguiente código producirá un error de compilación específicamente este:

```ignore
error[E0384]: cannot assign twice to immutable variable `edad`
 --> src/main.rs:4:5
  |
2 |     let edad = 10;
  |         ---- first assignment to `edad`
3 | 
4 |     edad = 5;
  |     ^^^^^^^^ cannot assign twice to immutable variable
```

Rust aplica la sabiduría que muchos programadores necesitan:
*"Muchos errores surgen de cambios involuntarios o incorrectos de las*
*variables, así que no permiré que el código cambie cualquier valor a menos que*
*lo haya permitido explícitamente."*

Si deseamos una variable mutable necesitaríamos indicarle a Rust específicamente
que la variable es mutable, si deseamos hacer algo así necesitamos hacer lo
siguiente:

<span><b>Ejemplo 1.1</b></span>

```rust,ignore
{{#include ../code/rust/mutvars.rs}}
```

Esto debería imprimir el siguiente mensaje en pantalla:

```ignore
Tienes 20 años
Ahora tienes 21 años
```

Para darnos una mejor idea de como funciona la mutabilidad de las variables
aquí tenemos un ejemplo del código 1.0 escrito en C:

```c,ignore
{{#include ../code/c/inmutvars.c}}
```

> No intentes compilarlo, no funcionará.

A comparación de las variables mutables en el ejemplo 1.1:

```c,ignore
{{#include ../code/c/mutvars.c}}
```

Podríamos decir que, cuando una declaración en Rust no contiene la palabra
clave `mut` su declaración correspondiente en C contiene la palabra clave
`const`. En otras palabras, en el lenguaje C, la manera mas sencilla de
declaración definne una variable mutable y necesitarías de una palabra extra
para obtener la inmutabilidad de la variable.

### Variables mutables sin cambiar.

Como lo dijimos antes, si tratamos de asignar un valor nuevo a una variable
inmutable obtendremos un error de compilación. Por otra parte, no se le
considera un error el declarar una variable como mutable y nunca asignarle un
valor nuevo, afortunadamente el compilador de Rust es inteligente y nos
reportará esta anomalía con una advertencia (puede variar dependiendo del
compilador, las opciones y la versión):

```rust,ignore
{{#include ../code/rust/inmut-warn.rs}}
```

```ignore
warning: variable does not need to be mutable
 --> main.rs:13:2
  |
2 |     let mut numero = 42;
  |         ----^
  |         |
  |         help: remove this `mut`
  |
  = note: #[warn(unused_mut)] on by default
```

La segunda línea de la advertencia nos indica la porción del código fuente que
presenta una anomalía, en este caso en el archivo main.rs, comenzando desde
la columna 13 de la fila 2. Debajo del mensaje anterior se nos muestra la línea
de código anómala y se nos presenta una sugerencia para mejorar nuestro código.

> rustc no es el único que nos puede ayudar. Existe una herramiente llamada
> [clippy](https://github.com/rust-lang/rust-clippy) la cual contiene varias
> linternas para ayudarnos a encontrar varios errores comunes en nuestro código
> fuente en Rust.

## Variables inmutables != Constantes

Seguramente lo habrás pensado, si las variables que declaramos por defecto no
pueden mutar ¿eso no las convierte en constantes?

Las constantes, al igual que las variables inmutables, no tienen permitido
cambiar sus contenidos y están ligadas a un identificador, pero existen
algunas diferencias entre las variables inmutables y las constantes.

La primera y mas importante, las variables inmutables tienen posibilidad de
cambio mediante algo llamado *"Sombreado"* (El cual exploraremos mas tarde), las
constantes por su lado **JAMÁS** cambiarán una vez sean declaradas.

Para declarar una constante se utiliza la palabra reservada `const` en lugar
de la palabra `let`, además una constante tiene que estar *"tipada"* SIEMPRE.

He aquí un ejemplo del uso de constantes, el estílo de código de Rust nos indica
que los identificadores de las constantes deberán ser siempre mayúsculas y
guiones bajos entre cada palabra:

```rust,ignore
const PERROS_VOLADORES: i32 = 200;
```

Las constantes son válidas durante todo el ciclo de vida del programa, siempre
y cuando se encuentren dentro del alcance donde fueron declaradas. Las
constantes tienen su utilidad y son resultan eficientes para el mantenimiento
del código.

## El poderoso guión bajo

Muchas veces pasa, declaramos una variable, le asignamos un valor y nunca la
volvemos a utilizar en todo nuestro programa pero ¿qué pasa si necesitamos
inicializar una variable y nunca utilizar su valor? El compilador detecta esto
y nos arroja una advertencia. Si deseamos suprimirla simplemente podemos
declarar nuestras variables con un guión bajo antes del identificador
de la siguiente manera:

```rust,ignore
# fn main() {
    let _gatos = 30;
# }
```

El guión bajo (Conocido como *underscore* en inglés) `_` es utilizable en
cualquier parte del identificador, pero cuando se encuentra al inicio de este
tiene el efecto de silenciar este tipo de advertencias del compilador.

## Sombreado y alcance

Todas las variables definidas en cualquier programa `.rs` tienen un alcance
local, limitado por las llaves `{}` de las funciones o métodos en las que son
declaradas, una vez salen de la llave `}` estas salen del alcance y la memoria
que se encontraba en uso es liberada.

En pocas palabras, una variable definida en un bloque solo será reconocida
dentro del mismo. Una variable en un bloque también puede repetir su
identificador en un bloque interno (sombreado).

Asi mismo, una variable que no posee el atributo `mut` puede cambiar su
valor mediante algo llamado *"sombreado"*. En pocas palabras estamos declarando
una variable con el mismo nombre que la anterior, podemos lograr esto al
utilizar un nombre utilizado previamente por una variable y la palabra reservada
`let`:

```rust,ignore
{{#include ../code/rust/shadow.rs}}
```

El siguiente programa primero crea una variable llamada `cow` (vaca) y le asigna
el variable de `20`. Después toma a `cow` otra vez y le agrega `1` al valor
total mediante el sombreado y así sucesivamente durante las operaciones
siguientes.

Sombrear variables es diferente a marcarlas como `mut`-ables. Al volver a
utilizar la palabra clave `let` podemos hacer un par de transformaciones y
mantener la variable inmutable una vez completemos dichas transformaciones.

Otra diferencia con respecto al uso de `mut` es que al *sombrear* una variable
técnicamente estamos creando una nueva variable con el mismo identificador, lo
que nos ayuda a cambiar el tipo de la variable en un futuro mientras esta esté
dentro del alcance.