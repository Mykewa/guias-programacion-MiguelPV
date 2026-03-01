<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Excepciones". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

### Respuesta
Dos formas comunes de indicar el error en C son:
1. Valor de retorno especial: Se devuelve un valor específico que indica error, como -1, junto con un mensaje de error fuera de la función.
2. Uso de una variable de error externa: Se utiliza una variable global o un puntero pasado por referencia para indicar el error.
Aquí tienes un ejemplo de cada una:

1. Devolver un valor especial
#include <stdio.h>
#include <math.h>
double raiz(double x) {
    if (x < 0) return -1; // Indica error
    return sqrt(x);
}
int main() {
    double num = -4;
    double resultado = raiz(num);

    if (resultado == -1)
        printf("Error: número negativo.\n");
    else
        printf("Resultado: %f\n", resultado);
    return 0;
}

2. Usar una variable de error externa
#include <stdio.h>
#include <math.h>
int error = 0; // Variable global de error
double raiz(double x) {
    if (x < 0) {
    error = 1; // Marca error
    return 0;
 }
    error = 0;
    return sqrt(x);
}
int main() {
    double num = -4;
    double resultado = raiz(num);

    if (error)
        printf("Error: número negativo.\n");
    else
        printf("Resultado: %f\n", resultado);
    return 0;
}

Ambas estrategias permiten informar del error desde fuera de la función.


## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Respuesta
Una excepción es un mecanismo que permite manejar errores o eventos inesperados durante la ejecución de un programa sin interrumpir su flujo normal.
Un programador las usa para:
1. Implementar funciones más seguras: Permiten detectar y manejar errores sin depender de valores de retorno especiales o variables globales.
2. Mejorar la legibilidad y mantenimiento del código: Separan la lógica principal del manejo de errores.
3. Facilitar la depuración: Proveen información detallada sobre la causa y ubicación del error.


## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### Respuesta
Aquí tienes el ejemplo en Java, usando excepciones para manejar el error:
class Calculadora {
    public static double raiz(double x) throws IllegalArgumentException {
        if (x < 0) {
            throw new IllegalArgumentException("Error: número negativo.");
    }
    return Math.sqrt(x);
    }
}

public class Main {
    public static void main(String[] args) {
    double num = -4;
    try {
        double resultado = Calculadora.raiz(num);
        System.out.println("Resultado: " + resultado);
    } catch (IllegalArgumentException e) {
    System.out.println(e.getMessage());
    }
    }
}

Aquí, la excepción IllegalArgumentException se lanza dentro de raiz(), y se captura en main(), permitiendo gestionar el error desde fuera de la función.


## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### Respuesta
1. Lanzar una excepción significa interrumpir el flujo normal del programa cuando ocurre un error, usando throw. En el ejemplo de Java, la línea:
throw new IllegalArgumentException("Error: número negativo.");
lanza la excepción cuando se recibe un número negativo en raiz().

2. Capturar o controlar una excepción consiste en usar un bloque try-catch para manejar el error y evitar que el programa se cierre abruptamente. En main(), el catch captura la excepción:

catch (IllegalArgumentException e) {
 System.out.println(e.getMessage());
}
Esto evita que el programa termine inesperadamente y permite informar del error.

3. Propagar una excepción ocurre cuando una función lanza una excepción sin capturarla, dejando que otra función en la pila de llamadas la maneje. En el ejemplo, raiz() lanza la excepción, y esta se propaga hasta main(), que la captura.

4. Efecto en la pila de llamadas: Cada vez que una función no maneja una excepción, se abandona su ejecución y se retrocede a la función que la llamó, buscando un catch. En el ejemplo:

    a. raiz() lanza la excepción.

    b. Se sale de raiz() sin devolver ningún valor.

    c. main() recibe la excepción y la maneja en el catch.

5. Las funciones que no capturan la excepción no se reanudan. Si main() no tuviera try-catch, la excepción se propagaría hasta el sistema y el programa terminaría con un error.


## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Respuesta
La propagación natural de excepciones en Java tiene varias ventajas frente a C, donde este mecanismo no existe:

1. El código es más limpio y modular: En C, los errores deben manejarse manualmente con valores de retorno o variables globales, lo que ensucia el código. En Java, una excepción lanzada se propaga automáticamente hasta encontrar un catch.

2. Manejo centralizado de errores: Permite capturar errores en un solo lugar, sin necesidad de comprobar valores de retorno en cada llamada, como en C.

3. Evita olvidar comprobar errores: En C, si el programador no verifica un valor de retorno de error, el programa puede comportarse de forma inesperada. En Java, si una excepción no se captura, el programa falla con un mensaje claro.

4. Información detallada del error: Java proporciona una traza de la pila (stack trace), que indica dónde y por qué ocurrió el error, mientras que en C, el programador debe gestionar manualmente los mensajes de error.


## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### Respuesta
Sí, en orientación a objetos, las excepciones suelen ser objetos, lo que aporta varias ventajas:

1. Encapsulación de información: Como objetos, pueden contener datos adicionales sobre el error, como mensajes, códigos de error o métodos específicos para obtener detalles.

2. Jerarquía y reutilización: Se pueden organizar en clases que heredan de excepciones estándar, permitiendo una gestión más estructurada y específica de errores.

3. Excepciones personalizadas: Se pueden crear clases de excepciones propias heredando de Exception o RuntimeException. Esto permite definir tipos de errores específicos para una aplicación, diferenciándolos claramente.


## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Respuesta
Comparando C y Java, una excepción en Java encapsula información esencial que facilita la gestión de errores cuando se captura en un catch. Algunas de las más útiles son:

1. El tipo de la excepción: Permite diferenciar errores específicos sin depender de códigos de error como en C.

2. Un mensaje descriptivo: Indica la causa del error (getMessage() en Java). En el ejemplo, "Error: número negativo." se puede recuperar fácilmente.

3. La traza de la pila (stack trace): Muestra la ruta exacta de llamadas hasta el error (printStackTrace() en Java). En C, habría que hacer seguimiento manual.

Esto permite un manejo más preciso y estructurado del error en comparación con C, donde solo se puede devolver un valor especial sin contexto detallado.


## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta
Sí, en Java se pueden tener múltiples bloques catch para manejar diferentes tipos de excepciones.

Solo se ejecuta el primer catch cuyo tipo de excepción coincida con la que fue lanzada. Si una excepción es capturada en un bloque catch, los siguientes bloques catch no se ejecutan.

Esto permite manejar errores de manera específica según su tipo, mejorando la claridad y control del código.


## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta
Para garantizar la ejecución de código crítico, como cierre de archivos o liberación de recursos, se usa el bloque finally, que siempre se ejecuta, haya o no excepción.

Ejemplo con catch:
try {
    int resultado = 10 / 0; // Provoca excepción
} catch (ArithmeticException e) {
    System.out.println("Error: división por cero.");
} finally {
    System.out.println("Este bloque siempre se ejecuta.");
}

Ejemplo sin catch:
try {
    int resultado = 10 / 0; // Provoca excepción
} finally {
    System.out.println("Este bloque siempre se ejecuta.");
}

Sin catch, la excepción se propaga, pero finally se ejecuta antes


## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta
Sí, en Java el bloque finally puede ir sin catch.

• Siempre se ejecuta: El bloque finally se ejecuta tanto si ocurre como si no ocurre una excepción en el bloque try. Esto asegura que el código de limpieza o liberación de recursos se ejecute independientemente del flujo de ejecución.

• Si hay un return en el try: Incluso si el try tiene un return, el bloque finally siempre se ejecuta antes de que el control se transfiera al llamador, justo después de que el try haya terminado. Esto garantiza que los recursos se liberen correctamente antes de que el método retorne.

Ejemplo:
public class Main {
    public static int ejemplo() {
    try {
        System.out.println("En el try.");
        return 1;
    } finally {
        System.out.println("En el finally.");
      }
    }
    public static void main(String[] args) {
    System.out.println(ejemplo());
    }
}

Salida:
En el try.
En el finally.
1

El bloque finally se ejecuta incluso después del return en try, garantizando la ejecución del código final.


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta
En Java, las excepciones se dividen en dos tipos:

1. Excepciones controladas (checked exceptions): Son excepciones que el programador debe manejar explícitamente, ya sea con un bloque try-catch o declarando que el método las lanza con throws. Son derivadas de Exception pero no de RuntimeException. Estas son excepciones que el compilador exige manejar, como problemas de entrada/salida (I/O) o errores de red.
Ejemplos:
    a. IOException: Si intentamos leer de un archivo y ocurre un error.
    b. SQLException: Si ocurre un error al interactuar con la base de datos.

Ejemplo:
public void leerArchivo() throws IOException {
    // Código que puede lanzar IOException
}

2. Excepciones no controladas (unchecked exceptions): Son excepciones que no requieren ser manejadas explícitamente, y generalmente ocurren por errores de programación, como acceder a índices fuera de rango o divisiones por cero. Estas excepciones derivan de RuntimeException, y no es obligatorio capturarlas.
Ejemplos:
    a. NullPointerException: Se produce al intentar acceder o modificar un objeto null.
    b. ArrayIndexOutOfBoundsException: Cuando intentamos acceder a un índice no válido de un array.

Ejemplo:
int[] array = new int[3];
array[5] = 10; // Esto lanza ArrayIndexOutOfBoundsException

¿Qué es y para qué se usa throws?
El throws se utiliza en la firma de un método para declarar que ese método puede lanzar una excepción controlada. Esto es una forma de delegar la responsabilidad de manejar la excepción a quien llame al método, permitiendo que el error se propague.

Ejemplo:
public void metodo() throws IOException {
 // Código que puede lanzar IOException
}


## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta
El `throws` es una palabra clave que se utiliza en la firma de un método para declarar que dicho método puede lanzar una o varias excepciones. Su función principal es informar al compilador y a otros programadores de que el método no manejará el error internamente, delegando esa responsabilidad a quien lo llame.

Es una alternativa a capturar una excepción controlada (mediante `try-catch`) por los siguientes motivos:

1. Propagación del error: En lugar de gestionar el problema localmente, permitimos que la excepción suba en la pila de llamadas hasta encontrar un bloque que sepa cómo resolverlo adecuadamente.
2. Delegación de responsabilidad: Se utiliza cuando el método actual no tiene suficiente contexto para decidir qué hacer ante el error (por ejemplo, un método que lee un archivo puede no saber si debe cerrar el programa o pedir al usuario que elija otro archivo; esa decisión le corresponde al `main` o a una capa superior).
3. Limpieza del código: Evita anidar bloques `try-catch` innecesarios en métodos que solo realizan tareas intermedias, manteniendo el flujo lógico más legible.

Ejemplo:
public void leerConfiguracion() throws IOException {
    // Si falla, no lo capturamos aquí; dejamos que quien nos llamó decida qué hacer.
    FileReader fr = new FileReader("config.txt");
}


## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta
Aquí tienes un ejemplo de una función en Java que abre un archivo, declara que no quiere manejar la excepción si el archivo no existe, y permite que esta excepción se propague hacia arriba, todo mientras se asegura de ejecutar un bloque finally para cerrar recursos:

import java.io.*;

public class Main {
    public static void abrirFichero(String nombreArchivo) throws FileNotFoundException {
        FileInputStream archivo = null;
        try {
            archivo = new FileInputStream(nombreArchivo);
            System.out.println("Fichero abierto correctamente.");
        } finally {
            if (archivo != null) {
                try {
                    archivo.close();
                    System.out.println("Fichero cerrado.");
                } catch (IOException e) {
                    System.out.println("Error al cerrar el fichero.");
                }
            }
        }
    }

    public static void main(String[] args) {
        try {
            abrirFichero("archivo_no_existente.txt");
        } catch (FileNotFoundException e) {
            System.out.println("Excepción capturada: El archivo no existe.");
        }
    }
}

Explicación:
1. El método abrirFichero tiene la firma throws FileNotFoundException, lo que significa que no maneja la excepción de que el archivo no exista, sino que la propaga hacia el llamador (en este caso, main()).
2. El bloque finally garantiza que el archivo se cierre correctamente, incluso si ocurre una excepción al intentar abrirlo.
3. Si el archivo no existe, el FileNotFoundException se lanza desde abrirFichero, y es capturada en main, donde podemos manejarla como deseemos.


## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta
Sí, en Java, se puede poner excepciones no controladas (como RuntimeException y sus subclases) en la declaración throws, pero no es obligatorio hacerlo. Sin embargo, hacerlo no es común ni necesario porque:

1. Excepciones no controladas no requieren manejo explícito: Las excepciones derivadas de RuntimeException son excepciones no controladas, lo que significa que no necesitan ser declaradas en throws ni ser manejadas con try-catch. Estas excepciones suelen ser el resultado de errores de programación, como un NullPointerException o un ArrayIndexOutOfBoundsException, y no se espera que el código siempre las maneje.

2. ¿Debería el llamador usar try-catch?: Si se declara una RuntimeException en throws, el llamador no está obligado a capturarla. A pesar de que se puede poner en throws, es innecesario que el llamador lo maneje explícitamente, ya que estas excepciones no requieren manejo forzoso.

3. ¿Qué sentido tendría?: Declarar una excepción no controlada en throws puede ser útil en ciertos casos si queremos documentar que el método podría lanzar una excepción no controlada, para que quien lo use esté consciente del posible error, aunque no tenga obligación de capturarlo.

Ejemplo:
public class Main {
 // El método lanza una excepción no controlada
 public static void metodoConRuntimeException() throws RuntimeException {
    throw new RuntimeException("Excepción no controlada");
 }
 
 public static void main(String[] args) {
    try {
        metodoConRuntimeException();
    } catch (RuntimeException e) {
        System.out.println("Capturada la excepción: " + e.getMessage());
    }
 }
}

En este caso, el uso de throws es innecesario, ya que no se requiere declarar una RuntimeException. El try-catch en main() sigue siendo válido, pero no es obligatorio.


## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta
Las excepciones controladas (como IOException) se usan cuando el error es previsible y fuera del control del programador, mientras que las no controladas (como IllegalArgumentException) se utilizan para errores de lógica que deben ser evitados.

En lenguajes como Python o JavaScript solo existen excepciones no controladas, lo que permite mayor flexibilidad en el manejo de errores.


## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta
Sí, tiene sentido lanzar excepciones dentro de un bloque catch si queremos propagar el error o transformarlo en otro tipo de excepción. Esto puede ser útil para agregar más contexto o cambiar la naturaleza del error.

Ejemplo:
try {
 int resultado = 10 / 0;
} catch (ArithmeticException e) {
 throw new RuntimeException("Error al realizar la operación matemática", e);
}

En este caso, la ArithmeticException es capturada y se lanza una RuntimeException con más información.


## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta
Cuando una excepción es la causa de otra, significa que una excepción original es encapsulada en una nueva excepción para proporcionar más contexto o transformar el error en un tipo más adecuado. La causa original se puede mantener usando el constructor de la nueva excepción.

Ejemplo en Java:
class ExcepcionPersonalizada extends Exception {
    public ExcepcionPersonalizada(String mensaje, Throwable causa) {
    super(mensaje, causa);
    }
}

public class Main {
    public static void main(String[] args) {
    try {
        // Código que puede lanzar una IOException
        throw new java.io.IOException("Error de archivo");
    } catch (java.io.IOException e) {
        throw new ExcepcionPersonalizada("Excepción de alto nivel", e); // Encapsulamos la causa
      }
    }
}

En este ejemplo, la IOException es capturada y encapsulada en una ExcepcionPersonalizada, manteniendo la causa original.

