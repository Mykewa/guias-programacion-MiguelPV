<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Aspectos funcionales". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia, polimorfismo y genericidad.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

### Respuesta
Un puntero a una función en C es una variable que, en lugar de almacenar un dato (como un entero o un carácter), almacena la dirección de memoria donde se encuentra el código ejecutable de una función.

Esto es extremadamente útil porque permite que las funciones sean tratadas como datos: se pueden guardar en variables, almacenar en arrays o pasarse como parámetros a otras funciones (lo que se conoce como callbacks).


Ejemplo de código en C:
Para que el ejemplo funcione correctamente, la cadena que le pasemos debe ser modificable (un array de caracteres local, no un string literal constante).


#include <stdio.h>
#include <ctype.h>

// 1. Definimos la función que recibe un char* y devuelve un char*
char* convertirMayusculas(char* cadena) {
    char* punteroActual = cadena;
    
    // Recorremos la cadena hasta llegar al carácter nulo '\0'
    while (*punteroActual != '\0') {
        *punteroActual = toupper((unsigned char)*punteroActual);
        punteroActual++;
    }
    
    return cadena; // Devolvemos el puntero original
}

int main() {
    // Variable con texto modificable
    char miTexto[] = "hola mundo desde c";

    // 2. Declaramos el puntero a la función
    // Sintaxis: TipoRetorno (*nombrePuntero)(TiposParametros)
    char* (*aMayusculas)(char*) = convertirMayusculas;

    // 3. Invocamos la función a través del puntero
    char* resultado = aMayusculas(miTexto);

    printf("Texto original modificado: %s\n", resultado);

    return 0;
}

Puntos clave de la sintaxis:
    - Declaración: Fíjate en los paréntesis alrededor del nombre del puntero (*aMayusculas). Son obligatorios. Si no los pones, el compilador pensará que estás declarando una función normal que devuelve un puntero a char.

    - Asignación: El nombre de una función en C (sin los paréntesis de llamada) ya actúa como un puntero a esa función, por lo que basta con igualar la variable a convertirMayusculas.

    - Invocación: Se utiliza la variable puntero exactamente igual que si fuera la función original, pasándole los argumentos entre paréntesis: aMayusculas(miTexto).


## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### Respuesta
Una función lambda (también conocida como función anónima) es una función pequeña que se define sin nombre. En lugar de utilizar la sintaxis clásica y verbosa de definición de métodos o funciones completas, las lambdas permiten escribir la lógica directamente en el lugar donde se va a utilizar o asignar.

Son la base de la programación funcional en muchos lenguajes modernos y se utilizan muchísimo para pasar comportamientos como si fueran datos (por ejemplo, como parámetros a otras funciones o métodos).


Ejemplo en JavaScript:
En JavaScript, las funciones lambda se conocen comúnmente como "Arrow Functions" (funciones flecha) y utilizan el operador =>.

// Definimos la función lambda y la guardamos en la variable local
const aMayusculas = (cadena) => cadena.toUpperCase();

// La invocamos
const resultado = aMayusculas("hola mundo desde javascript");
console.log(resultado); // Imprime: HOLA MUNDO DESDE JAVASCRIPT


Ejemplo en Java:
En Java, las lambdas se introdujeron en la versión 8 y utilizan el operador flecha ->. Como Java es fuertemente tipado, la variable que guarde la lambda debe ser de un tipo de "Interfaz Funcional". En este caso, usamos Function<String, String>, que significa: toma un String como entrada y devuelve un String como salida.

import java.util.function.Function;

public class EjemploLambda {
    public static void main(String[] args) {
        
        // Definimos la función lambda. No hace falta poner el 'return' 
        // ni las llaves porque es una sola línea.
        Function<String, String> aMayusculas = cadena -> cadena.toUpperCase();

        // Para invocarla, utilizamos el método abstracto de la interfaz (apply)
        String resultado = aMayusculas.apply("hola mundo desde java");
        System.out.println(resultado); // Imprime: HOLA MUNDO DESDE JAVA
    }
}


Puntos clave:
    - Las lambdas hacen el código mucho más conciso.

    - En ambos lenguajes, el compilador infiere gran parte de la información (por ejemplo, en Java no tuvimos que escribir (String cadena) -> ... porque el tipo ya estaba definido en Function<String, String>).


## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### Respuesta
¿Qué es el paradigma funcional?
El paradigma funcional es un estilo de programación declarativo donde el código se construye principalmente mediante la creación y composición de funciones.

Sus características principales son:
    - Inmutabilidad: Los datos no cambian una vez creados (se crean copias en lugar de modificar los originales).

    - Funciones puras: Una función siempre devuelve el mismo resultado para los mismos parámetros de entrada y no produce "efectos secundarios" (no altera variables globales ni el estado externo).

    - Declarativo: Te centras en decir qué quieres lograr, en lugar de detallar el paso a paso de cómo lograrlo (evitando bucles for o while tradicionales).


¿Por qué a Java 8 se le llama multi-paradigma?
Históricamente, Java nació siendo un lenguaje estrictamente Orientado a Objetos (POO). Sin embargo, a partir de Java 8, se introdujeron herramientas propias de la programación funcional, como las funciones lambda, la API Stream y las interfaces funcionales.

Al permitir al programador elegir y combinar libremente estrategias orientadas a objetos con estrategias funcionales dentro del mismo proyecto, Java se convirtió en un lenguaje multi-paradigma.


¿Qué significa que las funciones son "ciudadanos de primera clase"?
Decir que una función es un "ciudadano de primera clase" (first-class citizen) significa que el lenguaje de programación trata a las funciones exactamente igual que a cualquier otro tipo de dato (como un número entero, un texto o un objeto).

Para que esto se cumpla, una función debe poder:
    1. Asignarse a una variable (como hicimos en la pregunta anterior guardando la lambda en aMayusculas).

    2. Pasarse como parámetro a otras funciones.

    3. Devolverse como resultado de otra función.

En C o Java puro (antes de Java 8), las funciones/métodos no eran ciudadanos de primera clase porque no podías guardar un método dentro de una variable directamente. Con las lambdas, esto cambió.


## 4. Explica la sintaxis básica de una función lambda en Java.

### Respuesta
La sintaxis de una función lambda en Java es muy minimalista. Su objetivo es eliminar todo el "ruido" visual (como nombres de métodos, tipos de datos obvios o modificadores de acceso) para centrarse únicamente en la lógica.

Se compone de tres partes principales: Argumentos, el Operador Flecha y el Cuerpo.

Estructura general
La estructura básica sigue este patrón:
parámetros -> cuerpo

1. Los Parámetros (A la izquierda)
Define qué datos recibe la función. Hay varias reglas de simplificación:
    - Sin parámetros: Se usan paréntesis vacíos: () -> ...

    - Un solo parámetro: No necesita paréntesis: s -> ...

    - Varios parámetros: Obligatorio usar paréntesis: (a, b) -> ...

    - Tipos de datos: Normalmente el compilador los deduce (inferencia de tipos), pero puedes ponerlos si quieres: (String s) -> ...

2. El Operador Flecha (->)
Simplemente conecta los parámetros con la implementación. Es el símbolo que le dice a Java: "Con estos datos de aquí, haz esto de allá".

3. El Cuerpo (A la derecha)
Es donde ocurre la acción:
    - Expresión única: Si solo hay una línea de código, no necesitas llaves {} ni la palabra return. El resultado de la expresión se devuelve automáticamente.
        + Ejemplo: (a, b) -> a + b

    - Bloque de código: Si necesitas varias líneas, debes usar llaves {} y escribir return explícitamente si la función devuelve algo.

        + Ejemplo:
            (a, b) -> {
                int suma = a + b;
                return suma;
            }


## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### Respuesta
Ejemplo en JavaScript:
En JavaScript es muy directo porque, al ser las funciones "ciudadanos de primera clase", se pasan como cualquier otra variable.

// El método 'transformar' recibe un texto y una función (el 'callback')
function transformar(texto, funcionTransformadora) {
    return funcionTransformadora(texto);
}

// Definimos la lambda
const aMayusculas = (cadena) => cadena.toUpperCase();

// Invocamos pasando la función como argumento
const resultado = transformar("hola mundo", aMayusculas);

console.log(resultado); // Imprime: HOLA MUNDO


Ejemplo en Java:
En Java, debemos especificar el tipo de la interfaz funcional que esperamos recibir. En este caso, usaremos Function<String, String>.

import java.util.function.Function;

public class EjemploTransformar {

    // El método recibe el String y un objeto que implementa la interfaz Function
    public static String transformar(String texto, Function<String, String> funcion) {
        // Invocamos la lógica de la lambda usando .apply()
        return funcion.apply(texto);
    }

    public static void main(String[] args) {
        // 1. Definimos la lambda
        Function<String, String> aMayusculas = s -> s.toUpperCase();

        // 2. Pasamos la lambda al método 'transformar'
        String resultado = transformar("hola java", aMayusculas);

        System.out.println(resultado); // Imprime: HOLA JAVA
        
        // 3. También podemos pasarla directamente sin guardarla en una variable:
        System.out.println(transformar("java es potente", s -> s.toLowerCase()));
    }
}


Puntos clave de este diseño:
    - Desacoplamiento: El método transformar no sabe qué le va a hacer al texto. Solo sabe que recibirá una "receta" (la función) y la aplicará.

    - Flexibilidad: Podemos pasarle cualquier lógica (pasar a mayúsculas, añadir asteriscos, contar letras) sin necesidad de modificar el código del método transformar.

    - Invocación: Mientras que en JavaScript llamamos a la función directamente funcion(param), en Java debemos llamar al método específico de la interfaz, en este caso .apply().


## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### Respuesta
Ejemplo en JavaScript:
En JavaScript, usamos el truco de convertir el texto en array, darle la vuelta y volverlo a unir.

function transformar(texto, funcionTransformadora) {
    return funcionTransformadora(texto);
}

// Invocación directa con la lambda definida en el argumento
const resultado = transformar("reconocer", (s) => s.split("").reverse().join(""));

console.log(resultado); // Imprime: reconocer (es un palíndromo)
console.log(transformar("Java", (s) => s.split("").reverse().join(""))); // Imprime: avaJ


Ejemplo en Java:
En Java, aprovechamos la clase StringBuilder para invertir la cadena de forma eficiente en una sola línea.

import java.util.function.Function;

public class EjemploInline {
    public static String transformar(String texto, Function<String, String> funcion) {
        return funcion.apply(texto);
    }

    public static void main(String[] args) {
        // Invocamos pasando la lógica de inversión directamente
        String invertida = transformar("Estructuras", s -> new StringBuilder(s).reverse().toString());

        System.out.println(invertida); // Imprime: sarutcurtsE
    }
}


¿Por qué hacemos esto?
    - Concisión: Ahorras líneas de código al no declarar variables intermedias.

    - Legibilidad: Si la lógica es corta (como invertir un texto), ver la implementación justo donde se usa ayuda a entender qué está pasando sin saltar a otras partes del archivo.

    - Encapsulamiento: La lógica de "invertir" no ensucia el resto del código si solo la necesitas para esa llamada específica.


## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Respuesta
Un cierre (closure) es una función que "recuerda" o "captura" el entorno (las variables locales) en el que fue creada, incluso si ese entorno ya ha terminado de ejecutarse. En el contexto de las lambdas, permite que la función acceda a variables que están fuera de su propio cuerpo (en el bloque de código que la rodea).


Restricción en Java: El concepto de "Effectively Final":
Java impone una regla estricta para que una lambda pueda capturar una variable local: la variable debe ser final o efectivamente final (effectively final). Esto significa que, aunque no le pongas la palabra final, no puedes cambiar su valor después de asignarlo por primera vez.

Si intentas modificar la variable después de que la lambda la haya capturado, el compilador de Java dará un error. Esto se hace para evitar problemas de sincronización de memoria, ya que la lambda podría ejecutarse en un momento o hilo distinto a cuando se definió la variable.


Ejemplo en Java: Concatenación con variable externa:
Vamos a modificar el método transformar para que la lambda utilice una variable definida fuera de ella.

import java.util.function.Function;

public class EjemploClosure {

    public static String transformar(String texto, Function<String, String> funcion) {
        return funcion.apply(texto);
    }

    public static void main(String[] args) {
        // 1. Variable local definida fuera de la lambda
        String sufijo = " - Editado por IA"; 
        
        // s es el parámetro de entrada de la lambda
        // sufijo es una variable capturada del entorno (Closure)
        Function<String, String> añadirSufijo = s -> s + sufijo;

        // 2. Invocamos el método
        String resultado = transformar("Documento confidencial", añadirSufijo);

        System.out.println(resultado); 
        // Imprime: Documento confidencial - Editado por IA

        // IMPORTANTE: Si intentamos cambiar 'sufijo' aquí:
        // sufijo = " otro valor"; 
        // El código NO COMPILARÍA porque dejaría de ser "effectively final".
    }
}


¿Qué está pasando aquí?
    - La lambda añadirSufijo "envuelve" la variable sufijo.

    - Cuando el método transformar ejecuta funcion.apply(texto), la lambda sabe perfectamente qué vale sufijo porque se lo ha llevado "en su mochila" al ser creada.

    - Este mecanismo permite crear funciones altamente personalizadas en tiempo de ejecución basándose en datos que el programa recibe sobre la marcha.


## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### Respuesta
La diferencia fundamental no es solo estética, sino de capacidad y gestión de memoria. Se puede resumir en estos tres pilares:

1. El "Estado" (La diferencia más importante)
    - Puntero a función (C): Es simplemente una dirección de memoria que apunta a un bloque de código ejecutable. No tiene "memoria" de lo que pasaba a su alrededor cuando se creó. Es solo código puro.

    - Lambda (Java/JS): Es un Cierre (Closure). No es solo código, sino código + entorno. Como vimos en la pregunta anterior, la lambda puede "secuestrar" variables de su contexto y llevárselas consigo. Un puntero en C no puede hacer esto de forma nativa.

2. Anonimato y Ubicación
    - Puntero a función (C): Normalmente apunta a una función que ha tenido que ser definida, nombrada y declarada previamente en alguna parte del archivo.

    - Lambda (Java/JS): Puede ser anónima. Se define justo en el momento en que se necesita (inline), lo que evita "ensuciar" el código con nombres de funciones pequeñas que solo se van a usar una vez.

3. Seguridad de Tipos (Type Safety)
    - Puntero a función (C): Es muy peligroso. Es fácil equivocarse en la firma (parámetros o retorno) y el compilador a veces es permisivo, lo que acaba en errores de segmentación (segmentation faults) o corrupción de memoria.

    - Lambda (Java/JS): En Java, las lambdas están fuertemente ligadas a Interfaces Funcionales. El compilador verifica estrictamente que lo que entra y sale de la lambda coincide con lo que el método espera, evitando errores en tiempo de ejecución.


## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### Respuesta
En Java, un método puede devolver una interfaz funcional (como Function). Esto permite crear funciones personalizadas en tiempo de ejecución basándose en parámetros iniciales.

Ejemplo en Java: Fábrica de descuentos: 
Aquí tienes el código donde crearDescuento actúa como una factoría:

import java.util.function.Function;

public class FabricaFunciones {

    // Este método devuelve una función que recibe un Double y devuelve un Double
    public static Function<Double, Double> crearDescuento(double porcentaje) {
        // La lambda captura el parámetro 'porcentaje' y lo guarda en su contexto
        return (precio) -> precio * (1 - porcentaje / 100);
    }

    public static void main(String[] args) {
        // 1. Creamos dos funciones de descuento específicas
        Function<Double, Double> aplicarIVA = crearDescuento(-21); // Descuento negativo es un aumento
        Function<Double, Double> descuentoBlackFriday = crearDescuento(50); // 50% de descuento

        double precioBase = 100.0;

        // 2. Aplicamos las funciones creadas
        double conIVA = aplicarIVA.apply(precioBase);
        double conOferta = descuentoBlackFriday.apply(precioBase);

        System.out.println("Precio con IVA: " + conIVA);       // Imprime: 121.0
        System.out.println("Precio en Black Friday: " + conOferta); // Imprime: 50.0
    }
}


Explicación de la Closure (Clausura):
En este ejemplo, la closure ocurre cuando la expresión lambda (precio) -> precio * (1 - porcentaje / 100) es creada dentro del método crearDescuento:
    1. Captura del entorno: La lambda utiliza la variable porcentaje, que es un parámetro local del método.
    
    2. Persistencia: Cuando el método crearDescuento termina y retorna, su ámbito local normalmente desaparecería de la memoria. Sin embargo, como la lambda ha "capturado" el valor de porcentaje, ese valor sobrevive dentro de la función devuelta.
    
    3. Independencia: Cada vez que llamas a crearDescuento, se crea una nueva clausura con un valor distinto. Por eso aplicarIVA "recuerda" el -21 y descuentoBlackFriday "recuerda" el 50, aunque ambas hayan sido generadas por el mismo código.
    
Resumen Matemático: La función devuelta aplica la fórmula:
    precio_final = precio_inicial * (1 - porcentaje/100)
    
Donde porcentaje queda fijado en el momento de la creación de la función gracias a la clausura.


## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Respuesta
En Java, las funciones lambda no "flotan" en el vacío; debido al tipado estático del lenguaje, cada lambda debe tener un tipo definido. Ese tipo es siempre una Interfaz Funcional.

Una interfaz funcional es una interfaz que actúa como un "molde" para una función. Permite que el compilador entienda qué parámetros recibe la lambda y qué tipo de dato devuelve.


Requisitos de una Interfaz Funcional
Para que una interfaz sea considerada funcional, debe cumplir estrictamente con estas reglas:
    1. Exactamente un método abstracto: Debe tener un solo método que no esté implementado (el "contrato" de la función). Este es el requisito fundamental.

    2. Cualquier número de métodos default o static: Desde Java 8, las interfaces pueden tener métodos con cuerpo (implementados). Estos no cuentan para el límite del único método abstracto.

    3. Métodos de la clase Object: Si la interfaz sobrescribe un método público de java.lang.Object (como equals o toString), esto tampoco cuenta para el límite.

    4. La anotación @FunctionalInterface (Opcional pero recomendada): Se coloca encima de la interfaz. No es obligatoria para que funcione, pero sirve para que el compilador te avise con un error si intentas añadir un segundo método abstracto por error.

Ejemplo de estructura:

@FunctionalInterface
public interface Calculadora {
    // EL MÉTODO ABSTRACTO (Solo uno)
    int operar(int a, int b);

    // Métodos default (pueden haber muchos)
    default void mostrarMensaje() {
        System.out.println("Realizando operación...");
    }

    // Métodos estáticos (pueden haber muchos)
    static void info() {
        System.out.println("Interfaz para cálculos matemáticos.");
    }
}


¿Cómo se relaciona con la Lambda?
Cuando escribes Calculadora suma = (a, b) -> a + b;, el compilador de Java hace lo siguiente:
    1. Mira el tipo de la variable (Calculadora).

    2. Comprueba que es una Interfaz Funcional.

    3. "Encaja" los parámetros (a, b) y la lógica a + b dentro del único método abstracto disponible (operar).

Sin interfaces funcionales, las lambdas no podrían existir en Java porque el lenguaje no sabría cómo gestionar su tipo en memoria.


## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Respuesta
Aunque Java ya incluye muchas interfaces estándar (como Function), crear la tuya propia te permite dar nombres más significativos a tus métodos y mejorar la legibilidad del código según el dominio de tu problema.


Definición de la interfaz Transformador:

@FunctionalInterface
public interface Transformador {
    /**
     * Recibe una cadena y devuelve otra cadena transformada.
     */
    String ejecutar(String cadena);
}


Ejemplo de uso con Lambdas:
Ahora podemos usar nuestra interfaz Transformador en lugar de la genérica Function<String, String>. El funcionamiento es idéntico, pero el código es más "semántico":

public class PruebaTransformador {

    // El método ahora pide un "Transformador" en lugar de una "Function"
    public static String aplicarTransformacion(String texto, Transformador transformador) {
        return transformador.ejecutar(texto);
    }

    public static void main(String[] args) {
        // 1. Definimos una implementación usando una lambda
        Transformador aGritos = s -> s.toUpperCase() + "!!!";

        // 2. La usamos
        String resultado = aplicarTransformacion("hola", aGritos);
        
        System.out.println(resultado); // Imprime: HOLA!!!

        // 3. También podemos pasarla directamente (inline)
        String anonimo = aplicarTransformacion("Secreto", s -> s.replaceAll(".", "*"));
        System.out.println(anonimo); // Imprime: *******
    }
}


¿Por qué crear la nuestra?
    1. Claridad: El método se llama ejecutar(String) en lugar de apply(Object). Es más descriptivo para alguien que lea tu API.

    2. Especialización: Si mañana quisiéramos que Transformador lanzara una excepción específica (ej: TransformationException), podemos añadirla a la firma del método ejecutar, algo que no podríamos hacer con la interfaz estándar de Java.


## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Respuesta
Al añadir genéricos, nuestra interfaz se vuelve mucho más potente. Utilizaremos dos parámetros:

    - T (Input): El tipo de dato que entra.

    - R (Result): El tipo de dato que sale.


Definición de la interfaz genérica:

@FunctionalInterface
public interface Transformador<T, R> {
    /**
     * Transforma un objeto de tipo T en uno de tipo R.
     */
    R ejecutar(T entrada);
}


Ejemplo: Redondear de Double a Integer:
Ahora podemos usar esta misma interfaz para realizar operaciones numéricas, como convertir un número decimal en su entero más cercano.

public class EjemploGenerico {
    public static void main(String[] args) {
        
        // 1. Definimos el transformador: Entrada Double, Salida Integer
        // Usamos Math.round y hacemos cast a int porque round devuelve long
        Transformador<Double, Integer> redondear = d -> (int) Math.round(d);

        // 2. Probamos la transformación
        Double valorOriginal = 7.6;
        Integer valorRedondeado = redondear.ejecutar(valorOriginal);

        System.out.println("Original: " + valorOriginal);   // 7.6
        System.out.println("Redondeado: " + valorRedondeado); // 8

        // 3. Otro ejemplo: Transformar un String en su longitud (Integer)
        Transformador<String, Integer> contarLetras = s -> s.length();
        System.out.println("Letras en 'Hola': " + contarLetras.ejecutar("Hola")); // 4
    }
}


¿Qué hemos ganado?
    - Reutilización: Ya no necesitamos crear TransformadorString, TransformadorNumeros, etc. Una sola interfaz sirve para todo.

    - Seguridad de tipos: El compilador nos garantiza que si el transformador espera un Double, no le pasaremos accidentalmente un String.

    - Similitud con el estándar: Acabamos de replicar manualmente el funcionamiento de la interfaz java.util.function.Function<T, R> que viene por defecto en Java.


## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Respuesta
Efectivamente, el Transformador<T, R> que planteábamos es el equivalente exacto a la interfaz Function<T, R>. En Java 8, los diseñadores del lenguaje introdujeron el paquete java.util.function, que contiene un conjunto de interfaces funcionales predefinidas para cubrir casi cualquier necesidad básica, evitando que tengamos que crear nuestras propias versiones genéricas una y otra vez.


Las 4 Interfaces Funcionales Principales
Casi toda la programación funcional en Java gira en torno a estas cuatro interfaces base, que se diferencian por lo que reciben como parámetro y lo que devuelven:

    - Function<T, R> (Transformación)

        + Método Abstracto: R apply(T t)

        + Descripción: Recibe un objeto de tipo T y devuelve un resultado de tipo R.

        + Ejemplo de uso: Convertir un String a Integer.

    - Predicate<T> (Filtro / Condición)

        + Método Abstracto: boolean test(T t)

        + Descripción: Recibe un objeto de tipo T y devuelve un booleano (true o false).

        + Ejemplo de uso: Comprobar si un número es par.

    - Consumer<T> (Acción)

        + Método Abstracto: void accept(T t)

        + Descripción: Recibe un objeto de tipo T, hace algo con él y no devuelve nada (void).

        + Ejemplo de uso: Imprimir un objeto por consola o guardarlo en una base de datos.

    - Supplier<T> (Generación / Fábrica)

        + Método Abstracto: T get()

        + Descripción: No recibe ningún parámetro, pero devuelve un objeto de tipo T.

        + Ejemplo de uso: Generar un número aleatorio o proveer una fecha actual.


Interfaces Funcionales Especializadas
A partir de esas cuatro básicas, Java incluye variantes para casos más específicos y para optimizar el rendimiento:

    - Operadores (Entrada y salida del mismo tipo):

        + UnaryOperator<T>: Es una subinterfaz de Function<T, T>. Recibe un tipo y devuelve un objeto de ese mismo tipo (ej. elevar un número al cuadrado).

        + BinaryOperator<T>: Recibe dos parámetros del mismo tipo y devuelve un resultado de ese mismo tipo (ej. sumar dos números).

    - Versiones "Bi" (Dos parámetros de entrada):

        + BiFunction<T, U, R>: Recibe dos objetos de tipos distintos (T y U) y devuelve un resultado (R).

        + BiConsumer<T, U>: Recibe dos objetos distintos y no devuelve nada.

        + BiPredicate<T, U>: Recibe dos objetos distintos y devuelve un booleano.

    - Versiones para Tipos Primitivos (Para evitar el Autoboxing):

        + Trabajar con genéricos como Integer o Double consume más memoria y tiempo que usar primitivos como int o double. Por eso, Java ofrece versiones optimizadas como IntPredicate, DoubleFunction<R>, LongConsumer, o IntSupplier. Estas operan directamente con los tipos primitivos básicos sin necesidad de convertirlos a objetos.


## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### Respuesta
En Java 8, se introdujo el método forEach en la interfaz Iterable (de la cual hereda List). Este método es la alternativa funcional a los bucles for tradicionales.

¿Cómo funciona? Recibe por parámetro un Consumer<T>. Como vimos en la pregunta anterior, un Consumer es una función que "recibe un dato y hace algo con él, pero no devuelve nada" (ideal para imprimir o modificar estados).


Ejemplo en código:
En lugar de escribir la lógica del bucle y decir cómo iterar, simplemente le pasamos a la lista una lambda que le dice qué hacer con cada elemento:

import java.util.Arrays;
import java.util.List;

public class EjemploForEach {
    public static void main(String[] args) {
        // Creamos una lista de enteros mixtos
        List<Integer> numeros = Arrays.asList(-5, 12, 0, 7, -3, 8);

        // Versión Funcional con forEach
        System.out.println("Buscando números positivos:");
        
        numeros.forEach(num -> {
            if (num > 0) {
                System.out.println("El entero " + num + " es positivo.");
            }
        });
    }
}


¿Por qué esto es mejor o más expresivo?
    - Declarativo vs Imperativo: Con el for clásico (imperativo), tú controlas el flujo paso a paso. Con el forEach (declarativo), delegas el control de la iteración a la propia lista y tú solo te preocupas de proporcionar el comportamiento (la lambda).

    - Concisión: El código queda más limpio y directo al grano, especialmente cuando la operación a realizar es de una sola línea.

    - Paralelización: Aunque el forEach normal es secuencial, este estilo funcional prepara el terreno para cosas más avanzadas como .parallelStream().forEach(...), donde Java divide el trabajo automáticamente entre los núcleos de tu procesador.


## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta
Si miramos el código fuente de Java, el método forEach de las listas no recibe un Consumer<T>, sino un Consumer<? super T>. Esto se debe al principio PECS, que permite que el código sea mucho más flexible y reutilizable.


¿Por qué Consumer<? super T>?
Imagina que tienes una lista de enteros (List<Integer>). Si el método fuera estricto (Consumer<T>), solo aceptaría un Consumer<Integer>.

Sin embargo, ¿qué pasa si ya tienes creado un Consumer<Number> o un Consumer<Object> que simplemente imprime lo que recibe? Un entero es un número y es un objeto, por lo que debería ser perfectamente válido imprimir enteros usando un consumidor genérico de objetos.

Al usar ? super T (contravarianza), Java permite que a una lista de Integer le apliques cualquier función diseñada para consumir Integer o cualquiera de sus superclases.


¿Qué significa PECS?
PECS es un acrónimo creado por Joshua Bloch (autor de Effective Java) que significa "Producer Extends, Consumer Super". Es la regla de oro para saber cuándo usar cada wildcard (?) en los genéricos:

    - Producer Extends (? extends T): Si tu estructura de datos o función te va a devolver/producir datos, usa extends. Así garantizas que lo que sale es al menos de tipo T.

    - Consumer Super (? super T): Si tu estructura de datos o función va a recibir/consumir datos, usa super. Así permites que acepte herramientas diseñadas para tipos más genéricos.


Mejorando el método transformar con PECS:
Vamos a aplicar PECS a nuestro método genérico transformar. Recordemos que la función transformadora hace dos cosas: consume una entrada de tipo T y produce una salida de tipo R.

Versión rígida (Sin PECS):

public static <T, R> R transformar(T entrada, Function<T, R> funcion) {
    return funcion.apply(entrada);
}

Versión flexible (Con PECS):
Como la función consume T (Consumer Super) y produce R (Producer Extends), la mejora ideal es:

public static <T, R> R transformar(T entrada, Function<? super T, ? extends R> funcion) {
    return funcion.apply(entrada);
}


¿Qué ganamos con esto?
Gracias a esta firma mejorada:

    - Por el lado de la entrada (? super T): Si queremos transformar un String, podemos usar una función que esté diseñada para transformar un Object genérico (porque String es un Object).

    - Por el lado de la salida (? extends R): Si esperamos que el método devuelva un Number (R), la función transformadora puede devolvernos perfectamente un Integer (porque Integer hereda de Number).


## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Respuesta
Las referencias a métodos nos permiten tratar un método existente de un objeto o clase como si fuera una función lambda, guardándolo en una variable o pasándolo como parámetro.

Ejemplo en JavaScript: El problema del contexto (this):
En JavaScript, si simplemente sacamos el método de su objeto, este "olvida" a qué objeto pertenecía (pierde el valor de this). Para solucionarlo y que funcione correctamente, debemos usar el método .bind().

class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }

    saludar() {
        console.log("Hola, soy " + this.nombre);
    }
}

// 1. Creamos la instancia
const persona = new Persona("Carlos");

// 2. Obtenemos la referencia al método. 
// OBLIGATORIO usar .bind(persona) para que no pierda su contexto (this)
const referenciaSaludar = persona.saludar.bind(persona);

// 3. Invocamos la referencia
referenciaSaludar(); // Imprime: Hola, soy Carlos


Ejemplo en Java: El operador de resolución (::):
En Java 8 se introdujo el operador :: para crear referencias a métodos. A diferencia de JavaScript, Java "ata" automáticamente el método a su objeto, por lo que no pierde su estado. Como el método saludar no recibe parámetros ni devuelve nada, encaja perfectamente en la interfaz funcional estándar Runnable.

public class ReferenciaMetodo {

    static class Persona {
        private String nombre;

        public Persona(String nombre) {
            this.nombre = nombre;
        }

        public void saludar() {
            System.out.println("Hola, soy " + this.nombre);
        }
    }

    public static void main(String[] args) {
        // 1. Creamos la instancia
        Persona persona = new Persona("Ana");

        // 2. Obtenemos la referencia usando el operador ::
        // Usamos Runnable porque su método run() no recibe ni devuelve nada, igual que saludar()
        Runnable referenciaSaludar = persona::saludar;

        // 3. Invocamos la referencia
        referenciaSaludar.run(); // Imprime: Hola, soy Ana
    }
}


Resumen de diferencias:
    - Sintaxis: Java usa objeto::metodo, mientras que JS asigna la función como propiedad objeto.metodo.

    - Manejo del estado: En Java, la referencia persona::saludar recuerda automáticamente que pertenece a Ana. En JavaScript, si no usas .bind(persona), al invocar la variable, el programa fallará o imprimirá undefined porque no sabrá quién es this.nombre.


## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta
1. Referencia a un método estático
    - Sintaxis: Clase::metodoEstatico

    - Cómo funciona: Se pasa directamente una llamada a un método estático. Los parámetros de la lambda se convierten en los parámetros del método estático.

2. Referencia a un constructor
    - Sintaxis: Clase::new

    - Cómo funciona: Llama al constructor de la clase para crear un nuevo objeto. Dependiendo de la interfaz funcional que uses (por ejemplo, Supplier o Function), Java sabrá si llamar al constructor vacío o al que recibe parámetros.

3. Referencia a un método de instancia de un objeto concreto
    - Sintaxis: objetoInstanciado::metodoDeInstancia

    - Cómo funciona: Es el caso que vimos en la pregunta 16. La referencia guarda el contexto de un objeto que ya existe en memoria. Cuando se invoca, el método se ejecuta sobre ese objeto en particular.

4. Referencia a un método de instancia de un objeto arbitrario de un tipo particular
    - Sintaxis: Clase::metodoDeInstancia

    - Cómo funciona: Es la más confusa al principio. A diferencia del caso 3, aquí no hay un objeto previo. Al invocar la lambda, el primer parámetro que recibe la función se convierte en el objeto sobre el que se ejecuta el método.

Ejemplo de código con los 4 tipos:

import java.util.function.Consumer;
import java.util.function.Function;
import java.util.function.Supplier;
import java.util.ArrayList;
import java.util.List;

public class TiposReferencias {

    // Clase auxiliar para el ejemplo del constructor
    static class Persona {
        private String nombre;
        public Persona(String nombre) { this.nombre = nombre; }
        public String getNombre() { return this.nombre; }
    }

    public static void main(String[] args) {

        // ---------------------------------------------------------
        // 1. Método Estático (Clase::metodoEstatico)
        // Lambda equivalente: (s) -> Integer.parseInt(s)
        // ---------------------------------------------------------
        Function<String, Integer> conversorEntero = Integer::parseInt;
        Integer numero = conversorEntero.apply("42");
        System.out.println("1. Estático: " + numero);


        // ---------------------------------------------------------
        // 2. Constructor (Clase::new)
        // Lambda equivalente: (nombre) -> new Persona(nombre)
        // Usamos Function porque recibe un String y devuelve una Persona
        // ---------------------------------------------------------
        Function<String, Persona> creadorPersona = Persona::new;
        Persona p = creadorPersona.apply("Laura");
        System.out.println("2. Constructor: Creada persona " + p.getNombre());


        // ---------------------------------------------------------
        // 3. Instancia sobre un objeto concreto (objeto::metodo)
        // Lambda equivalente: (texto) -> System.out.println(texto)
        // System.out es un objeto estático ya instanciado en memoria
        // ---------------------------------------------------------
        Consumer<String> imprimir = System.out::println;
        imprimir.accept("3. Objeto concreto: Hola desde el Consumer");


        // ---------------------------------------------------------
        // 4. Instancia sobre objeto arbitrario (Clase::metodo)
        // Lambda equivalente: (cadena) -> cadena.toUpperCase()
        // El parámetro de entrada (String) es el que ejecuta el método.
        // ---------------------------------------------------------
        Function<String, String> convertirMayusculas = String::toUpperCase;
        String resultado = convertirMayusculas.apply("java es genial");
        System.out.println("4. Objeto arbitrario: " + resultado);
    }
}


## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta
Antes de ver las dos versiones, definimos la clase Persona y la lista de prueba que vamos a utilizar:

import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

class Persona {
    private String nombre;
    private int edad;

    public Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }

    public String getNombre() { return nombre; }
    public int getEdad() { return edad; }

    @Override
    public String toString() {
        return nombre + " (" + edad + ")";
    }
}


Versión 1: Lambda con lógica manual:
En esta versión, le pasamos a Collections.sort una lambda que implementa el método compare(p1, p2). Aquí tenemos que escribir manualmente la lógica algorítmica: restar las edades o usar Integer.compare, y si el resultado es 0 (misma edad), usar el compareTo de los String.

public class OrdenacionManual {
    public static void main(String[] args) {
        List<Persona> personas = new ArrayList<>();
        personas.add(new Persona("Carlos", 30));
        personas.add(new Persona("Ana", 25));
        personas.add(new Persona("Beatriz", 30));

        // Ordenación con lambda manual
        Collections.sort(personas, (p1, p2) -> {
            // 1. Comparamos las edades
            int comparacionEdad = Integer.compare(p1.getEdad(), p2.getEdad());
            
            // 2. Si son iguales, comparamos los nombres
            if (comparacionEdad == 0) {
                return p1.getNombre().compareTo(p2.getNombre());
            }
            
            // 3. Si no son iguales, devolvevers el resultado de la edad
            return comparacionEdad;
        });

        System.out.println(personas); 
        // Resultado: [Ana (25), Beatriz (30), Carlos (30)]
    }
}


Versión 2: Empleando la API de Comparator (Declarativo):
A partir de Java 8, la interfaz Comparator incluye métodos estáticos y por defecto (comparing y thenComparing) que nos permiten construir comparadores complejos concatenando operaciones, igual que si estuviéramos hablando. Se suele combinar con referencias a métodos (::) para hacerlo aún más limpio.

public class OrdenacionDeclarativa {
    public static void main(String[] args) {
        List<Persona> personas = new ArrayList<>();
        personas.add(new Persona("Carlos", 30));
        personas.add(new Persona("Ana", 25));
        personas.add(new Persona("Beatriz", 30));

        // Ordenación usando la API de Comparator
        Collections.sort(personas, 
            Comparator.comparingInt(Persona::getEdad)
                      .thenComparing(Persona::getNombre)
        );

        System.out.println(personas); 
        // Resultado: [Ana (25), Beatriz (30), Carlos (30)]
    }
}


Análisis de la mejora
La Versión 2 es el estándar actual en la industria de Java por tres razones:

    1. Es declarativa: No dices cómo comparar los números o las cadenas (con if y devolviendo -1, 0 o 1), sino que dices qué quieres hacer: "Compara por edad, y luego compara por nombre".

    2. Menos código boilerplate: Desaparece todo el ruido visual de las llaves, los return y las variables temporales.

    3. Prevención de errores: Evitas el clásico error de equivocarte al restar p1 de p2 y acabar ordenando la lista al revés sin querer. Además, comparingInt está optimizado para trabajar con primitivos sin generar objetos basura (autoboxing).

