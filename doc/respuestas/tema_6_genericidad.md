<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

### Respuesta
Para lograr que una estructura de datos pueda almacenar cualquier tipo de dato genérico en Java, se utiliza la clase Object. Como en Java absolutamente todas las clases heredan (de forma implícita o explícita) de Object, un array de tipo Object[] tiene la capacidad de referenciar a cualquier objeto.

class ContenedorUniversal {
    // Array primitivo de tipo Object que alojará cualquier cosa
    private Object[] elementos;
    private int indice;

    public ContenedorUniversal(int capacidad) {
        elementos = new Object[capacidad];
        indice = 0;
    }

    // Recibe un Object, por lo que acepta String, Integer, Punto, etc.
    public void anadir(Object elemento) {
        if (indice < elementos.length) {
            elementos[indice] = elemento;
            indice++;
        }
    }

    // Devuelve un Object genérico
    public Object obtener(int posicion) {
        return elementos[posicion];
    }
}

public class Main {
    public static void main(String[] args) {
        ContenedorUniversal caja = new ContenedorUniversal(5);

        // 1. Podemos añadir distintos tipos de datos sin problema
        caja.anadir("Un texto");  // Añade un String
        caja.anadir(2026);        // Añade un Integer (por autoboxing)

        // 2. EL INCONVENIENTE: Al recuperar los datos, el compilador solo sabe 
        // que son "Object". Es obligatorio hacer "casting" (conversión explícita) 
        // para recuperar su tipo original y poder usar sus métodos.
        String miTexto = (String) caja.obtener(0);
        Integer miNumero = (Integer) caja.obtener(1);

        System.out.println("Texto: " + miTexto);
        System.out.println("Número: " + miNumero);
    }
}


## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

### Respuesta
¿Qué significa la "programación genérica"?
La programación genérica es un paradigma que permite escribir código flexible y reutilizable usando tipos genéricos (marcadores de posición), es decir, sin especificar el tipo de dato concreto hasta que el código realmente se instancie o se llame. Su gran ventaja es que permite crear estructuras y algoritmos universales manteniendo una estricta seguridad de tipos (type safety).

¿Es el ejemplo anterior (con Object) programación genérica?
No, no lo es. Aunque ese enfoque permite almacenar cualquier tipo de dato, se basa en el polimorfismo de herencia y en la conversión manual de tipos (casts), lo cual rompe los principios de la genérica real.

Esto presenta diferencias clave:

    - El problema de Object: Depende de que el programador no se equivoque al hacer el cast al sacar el dato. Si te equivocas, el programa compila sin avisar, pero "explota" al ejecutarse mostrando un error.

    -La solución Genérica: La programación genérica real (usando los diamantes <T> en Java o template en C++) le avisa al compilador del tipo exacto que va a contener la estructura. Esto elimina por completo la necesidad de usar casts y asegura que cualquier incompatibilidad de tipos se detecte en tiempo de compilación.

    En resumen: Usar Object es "esconder" el tipo de dato. Usar programación genérica es "parametrizar" el tipo de dato.


## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

### Respuesta
El principal problema de usar void* (en C/C++) u Object (en Java) para crear estructuras genéricas es que se pierde el chequeo de tipos en tiempo de compilación.

Esto provoca tres grandes inconvenientes:

    - Ceguera del compilador: El compilador no puede verificar si estás almacenando o recuperando el tipo de dato correcto (no sabe si dentro hay un texto, un número o un soldado).

    - Casts obligatorios y peligrosos: Al recuperar un dato, necesitas hacer una conversión explícita (cast). Si te equivocas, el programa compilará sin problemas, pero "explotará" en tiempo de ejecución (lanzando un ClassCastException en Java o causando un comportamiento indefinido en C++).

    - Código frágil: Se reduce drásticamente la seguridad de tipos (type safety), lo que dificulta el mantenimiento y aumenta el riesgo de bugs (errores) ocultos.

    La solución: Por estos motivos, el uso de plantillas (template en C++) o genéricos (<T> en Java) es la opción correcta. Ambas herramientas mantienen el chequeo de tipos en tiempo de compilación, eliminan la necesidad de casts manuales y garantizan la seguridad.


## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

### Respuesta


## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### Respuesta
Los parámetros de tipo son identificadores (funcionan como "comodines" o marcadores de posición) que representan un tipo de dato abstracto dentro de una clase, interfaz o función genérica. Su objetivo es permitir que el código trabaje con cualquier tipo de dato sin perder la seguridad de tipos (type safety).

¿Cómo se utilizan?
    - En Java, se representan habitualmente entre diamantes, como <T>, <E> o <K, V>.

    - En C++, se utilizan mediante plantillas, como template <typename T>.

El tipo de dato concreto (por ejemplo, String, Integer o Soldado) no se define al escribir la clase original, sino en el momento exacto en el que se instancia la estructura. El compilador se encarga de sustituir ese parámetro por el tipo real y verificar que todo sea correcto en tiempo de compilación.

Ventajas principales:
    -Eliminan los casts manuales: Al recuperar un dato de la estructura genérica, el programa ya sabe exactamente de qué tipo es (ya no devuelve un Object).

    - Prevención de errores: Las incompatibilidades de tipos se detectan al compilar el código, evitando que el programa falle en tiempo de ejecución.

    - Reutilización y limpieza: Permiten escribir un único algoritmo (como ordenar) o estructura (como una lista) que sirva automáticamente para cualquier tipo de objeto, haciendo el código mucho más legible.


## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Respuesta
Cuando se instancia una clase genérica con parámetros de tipo, los compiladores de Java y C++ actúan de formas completamente distintas para resolverlo.

Java: Type Erasure (Borrado de tipos)
En Java, el compilador aplica una técnica llamada Type Erasure.

    - ¿Qué hace? Elimina los parámetros genéricos (como <T>) durante el proceso de compilación y los reemplaza internamente por la clase Object (o por su tipo límite si lo tiene). Inserta casts automáticos donde sea necesario.

    - Consecuencias: Al ejecutarse el programa, ya no existe información sobre el tipo genérico original. Por este motivo, en Java está prohibido instanciar un genérico directamente (hacer new T()) o comprobar su tipo explícito (usar instanceof T), ya que en ejecución esa "T" ha desaparecido.

C++: Template Instantiation (Instanciación de plantillas)
En C++, el compilador utiliza la Instanciación de plantillas.

    - ¿Qué hace? Genera una copia real e independiente del código base para cada tipo concreto que utilices. Si en tu código declaras un MiVector<int> y luego un MiVector<string>, el compilador escribe en secreto dos clases totalmente distintas, una para enteros y otra para textos.

    - Consecuencias: Esto permite mantener toda la información de tipos intacta y optimiza muchísimo el rendimiento en ejecución. ¿La desventaja? Aumenta drásticamente el tamaño del archivo binario final (un problema técnico conocido como code bloat o inflado de código).


## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

### Respuesta
En Java, puedes definir una clase genérica que acepte más de un parámetro de tipo. Esto es muy útil cuando necesitas devolver múltiples valores de distintos tipos desde una sola función.

public class Par<A, B> {
    private A primero;
    private B segundo;

    public Par(A primero, B segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public A getPrimero() {
        return primero;
    }

    public B getSegundo() {
        return segundo;
    }
}

Ejemplo de uso: Devolver dos valores en una función
En Java, el return tradicional solo permite devolver una cosa. Usando nuestra clase Par, podemos crear una función matemática que devuelva tanto la media (primer valor) como la desviación típica (segundo valor) de golpe.

public class Matematicas {

    // La función devuelve un Par que contiene dos objetos de tipo Double
    public static Par<Double, Double> calcularEstadisticas(double[] datos) {
        double suma = 0;
        for (double d : datos) {
            suma += d;
        }
        double media = suma / datos.length;

        double sumaCuadrados = 0;
        for (double d : datos) {
            sumaCuadrados += Math.pow(d - media, 2);
        }
        double desviacion = Math.sqrt(sumaCuadrados / datos.length);

        // Instanciamos el genérico indicando los tipos concretos
        return new Par<Double, Double>(media, desviacion); 
    }

    public static void main(String[] args) {
        double[] valores = {2, 4, 4, 4, 5, 5, 7, 9};
        
        // Almacenamos el resultado en nuestro objeto genérico
        Par<Double, Double> resultado = calcularEstadisticas(valores);
        
        System.out.println("Media: " + resultado.getPrimero());
        System.out.println("Desviación típica: " + resultado.getSegundo());
    }
}


## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

### Respuesta
En Java, no es obligatorio que toda la clase sea genérica; podemos aplicar parámetros de tipo únicamente a un método concreto. Esto se hace colocando el parámetro <T> justo antes del tipo de retorno.

1. Implementación con Genéricos (<T>)
import java.util.Random;

public class EjemploGenerico {
    
    // El parámetro <T> se declara antes del tipo de retorno (T)
    public static <T> T seleccionaUno(T objeto1, T objeto2) {
        Random rand = new Random();
        return rand.nextBoolean() ? objeto1 : objeto2;
    }

    public static void main(String[] args) {
        String s1 = "Hola";
        String s2 = "Mundo";
        
        // Devuelve un String directamente, no hay que hacer casting
        String ganador = seleccionaUno(s1, s2); 
        System.out.println(ganador);
    }
}

2. Implementación clásica (con Object)
import java.util.Random;

public class EjemploSinGenericos {
    
    public static Object seleccionaUno(Object objeto1, Object objeto2) {
        Random rand = new Random();
        return rand.nextBoolean() ? objeto1 : objeto2;
    }

    public static void main(String[] args) {
        String s1 = "Hola";
        String s2 = "Mundo";
        
        // OBLIGATORIO hacer downcasting al recuperar el dato
        String ganador = (String) seleccionaUno(s1, s2); 
        System.out.println(ganador);
    }
}

Diferencias clave: ¿Por qué es mejor <T>?
    1. Evitar el Downcasting

        - Con <T>: El compilador sabe exactamente qué tipo de dato has introducido, por lo que el método devuelve ese mismo tipo (si metes String, devuelve String). Te ahorras el casting manual.

        - Con Object: El método siempre devuelve un Object genérico. Para poder guardarlo en una variable String, estás obligado a hacer un downcasting explícito (String), introduciendo el riesgo de sufrir un ClassCastException si te equivocas.


    2. Forzar que ambos objetos sean del mismo tipo

        - Con <T>: El compilador garantiza la coherencia. Si llamas al método pasando un String y un Integer (seleccionaUno("Hola", 5)), el programa no compilará, protegiéndote de errores lógicos.

        - Con Object: No hay ninguna restricción. Puedes pasar un texto como primer parámetro y un número como segundo. El compilador lo tragará sin problemas, pero provocará un desastre en tiempo de ejecución cuando intentes recuperar el dato.


## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Respuesta
Sí se pueden establecer restricciones. En Java, se utiliza la sintaxis <T extends ClaseBase> para garantizar que el parámetro genérico introducido herede de una clase concreta (o implemente una interfaz). Esto permite usar dentro de la clase los métodos de esa clase base (como .doubleValue() en el caso de los números).

Solución 1: Usando polimorfismo clásico (Sin genéricos)
En este enfoque usamos directamente la clase abstracta Number. El problema es que perdemos la información del tipo original: al hacer getX(), obtenemos un Number genérico y estaríamos obligados a hacer un casting si queremos tratarlo como un Integer.

class PuntoFijo {
    private Number x;
    private Number y;

    public PuntoFijo(Number x, Number y) {
        this.x = x;
        this.y = y;
    }

    public Number getX() { return x; }
    public Number getY() { return y; }

    public double calcularDistanciaA(PuntoFijo otro) {
        // Number nos garantiza que existe el método doubleValue()
        double dx = this.x.doubleValue() - otro.x.doubleValue();
        double dy = this.y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(Math.pow(dx, 2) + Math.pow(dy, 2));
    }
}

Solución 2: Usando Genéricos Acotados (<T extends Number>)
Esta es la solución ideal. El compilador se asegura de que solo le pases números, pero retiene el tipo original. Si creas un PuntoGenerico<Integer>, getX() te devolverá directamente un Integer, sin necesidad de hacer casts.

// Restringimos T para que solo acepte clases que hereden de Number
class PuntoGenerico<T extends Number> {
    private T x;
    private T y;

    public PuntoGenerico(T x, T y) {
        this.x = x;
        this.y = y;
    }

    public T getX() { return x; }
    public T getY() { return y; }

    // Usamos <?> (comodín) para permitir calcular la distancia 
    // entre un punto de Integers y un punto de Doubles
    public double calcularDistanciaA(PuntoGenerico<?> otro) {
        double dx = this.x.doubleValue() - otro.x.doubleValue();
        double dy = this.y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(Math.pow(dx, 2) + Math.pow(dy, 2));
    }
}
El efecto del "Type Erasure" con límites
Cuando se compila el código, Java aplica el Type Erasure (borrado de tipos).

    - Si declaramos un genérico libre como <T>, tras compilar se convierte en Object.

    - Sin embargo, al ponerle un límite como <T extends Number>, el compilador reemplaza todas las apariciones de la T por la clase Number.

Por tanto, a nivel de bytecode (el archivo .class compilado), la clase PuntoGenerico se verá exactamente igual que la clase PuntoFijo de la Solución 1, pero con la ventaja de que el compilador ya ha insertado todos los casts seguros por ti.


## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### Respuesta
Al comparar ambas soluciones, vemos que el uso de genéricos cambia por completo la rigurosidad con la que el compilador trata a los tipos de datos.

1. Sobre la posibilidad de mezclar tipos (Integer y Double)

    - Sin genéricos (usando Number): Sí se pueden mezclar. Como ambas coordenadas se declaran independientemente como Number, Java te permite instanciar el punto dándole a x un valor Integer (ej: 5) y a y un valor Double (ej: 3.14).

    - Con genéricos (<T>): No se pueden mezclar. Al instanciar la clase (por ejemplo, Punto<Integer>), la T se define estrictamente para todo el objeto. El compilador te obligará a que tanto x como y sean exactamente del mismo tipo, garantizando la coherencia.


2. Sobre el tipo que devuelve el método getX()

    - Sin genéricos: El método devuelve un objeto de la superclase Number. El compilador ha "olvidado" qué tipo de número exacto introdujiste. Si quieres hacer una operación matemática específica de los enteros, estarás obligado a hacer un casting manual (Integer), asumiendo el riesgo de que el programa falle al ejecutarse si resulta que realmente era un Double.

    - Con genéricos: El método devuelve el tipo exacto T (por ejemplo, Integer). El compilador conserva la información original, por lo que te ahorras el casting y ganas la seguridad de que el tipo es 100% correcto desde antes de ejecutar el código.

Conclusión: Usar la clase abstracta Number te da más flexibilidad para tener coordenadas mixtas, pero pierdes el chequeo de tipos. Usar genéricos es más restrictivo, pero refuerza enormemente la seguridad de tu programa evitando bugs ocultos.


## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.
```java
public interface Punto { 
    public double distanciaA(Punto p); 
} 

public class Punto2D implements Punto { 
     private final double x, y; 
     public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto p) { 
        if (p instanceof Punto2D) { 
            Punto2D p2d = (Punto2D) p; 
            return Math.sqrt(Math.pow(x - p2d.x, 2) 
                    + Math.pow(y - p2d.y, 2)); 
        } else { 
            throw new RuntimeException("p debe ser Punto 2D"); 
        } 
    } 
} 
public class Punto3D implements Punto { 
    // Igual que Punto2D, pero con tres coordenadas
    ...
} 
```

### Respuesta
Al definir Punto<T extends Punto<T>>, estamos obligando a que cualquier clase que implemente la interfaz especifique con qué tipo exacto de "Punto" es compatible.

1. La Interfaz y Punto2D
// T debe ser un subtipo de Punto que acepte a T como parámetro
public interface Punto<T extends Punto<T>> { 
    public double distanciaA(T p); 
} 

public class Punto2D implements Punto<Punto2D> { 
    private final double x, y; 

    public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    // Ahora el parámetro es Punto2D, no la interfaz genérica
    public double distanciaA(Punto2D p) { 
        // Acceso directo a p.x y p.y sin casting ni instanceof
        return Math.sqrt(Math.pow(x - p.x, 2) + Math.pow(y - p.y, 2)); 
    } 
}

2. Implementación de Punto3D
public class Punto3D implements Punto<Punto3D> { 
    private final double x, y, z; 

    public Punto3D(double x, double y, double z) { 
        this.x = x; this.y = y; this.z = z; 
    } 

    @Override 
    public double distanciaA(Punto3D p) { 
        return Math.sqrt(Math.pow(x - p.x, 2) 
                       + Math.pow(y - p.y, 2) 
                       + Math.pow(z - p.z, 2)); 
    } 
}

¿Por qué es esto mejor?
En tu código original, el error de mezclar un Punto2D con un Punto3D ocurría en tiempo de ejecución (lanzando una RuntimeException). Con esta solución, el error ocurre en tiempo de compilación. El compilador simplemente no te permitirá pasar un Punto3D al método distanciaA de un Punto2D.

Ejemplo de uso y seguridad de tipos
Punto2D p2 = new Punto2D(1, 2);
Punto2D p2_otro = new Punto2D(4, 6);
Punto3D p3 = new Punto3D(1, 2, 3);

p2.distanciaA(p2_otro); // OK: Compila y funciona perfectamente.
// p2.distanciaA(p3);    // ERROR DE COMPILACIÓN: El método espera Punto2D, no Punto3D.


El efecto del "Type Erasure" aquí:
A pesar de que el código es mucho más seguro para nosotros, recuerda que tras la compilación (Type Erasure), el parámetro T se sustituye por su límite superior. Como pusimos <T extends Punto<T>>, el código compilado en el bytecode usará la interfaz Punto. Sin embargo, el compilador de Java inserta automáticamente los bridge methods necesarios para que la seguridad de tipos se mantenga sin que tú tengas que escribir ni un solo instanceof.


## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### Respuesta
Aunque String es un subtipo de Object, las colecciones genéricas y los arrays en Java se comportan de forma completamente opuesta respecto a la herencia.

1. ¿Es List<String> subtipo de List<Object>?
No. Los genéricos en Java son invariantes.
Esto significa que no existe ninguna relación de herencia entre List<String> y List<Object>. Una lista de Strings solo puede ser tratada como una lista de Strings. El compilador lo prohíbe para garantizar la seguridad total de tipos en tiempo de compilación.

2. ¿Es String[] subtipo de Object[]?
Sí. Los arrays en Java son covariantes.
Esto significa que si String hereda de Object, entonces un array String[] hereda de Object[]. Por tanto, puedes asignar un array de Strings a una variable de tipo array de Objects.


¿Por qué la diferencia y qué problema causan los arrays?
Los arrays nacieron en la primera versión de Java (cuando no existían los genéricos). Se hicieron covariantes porque era la única forma de escribir métodos genéricos útiles (por ejemplo, un método ordenar(Object[] array) que pudiera recibir tanto un array de Strings como uno de Integers).

Sin embargo, esta decisión introdujo una falla de seguridad en tiempo de ejecución, conocida como ArrayStoreException. Al ser covariantes, el compilador te permite hacer esto:

String[] arrayTextos = new String[5];
Object[] arrayGenerico = arrayTextos; // Permitido: los arrays son covariantes

// El compilador lo permite porque arrayGenerico es de tipo Object[]
// ¡Pero en tiempo de ejecución explotará! El array real en memoria solo admite Strings.
arrayGenerico[0] = 42; // Lanza ArrayStoreException

Los genéricos (introducidos en Java 5) aprendieron de este error. Al ser invariantes, el compilador simplemente se niega a compilar la línea List<Object> lista = new ArrayList<String>();, evitando que el programa pueda llegar a fallar mientras se ejecuta.


Definiciones clave de Varianza
Para entender cómo se relacionan los tipos complejos (como listas o arrays) en función de sus tipos base (como String y Object), utilizamos estos tres conceptos:

    - Covariante (Covariancia): Conserva la relación de herencia original. Si A es subtipo de B, entonces Tipo<A> es subtipo de Tipo<B>.

        + Ejemplo en Java: Los arrays (String[] es subtipo de Object[]). En genéricos se puede forzar usando comodines: List<? extends Object>.

    - Contravariante (Contravariancia): Invierte la relación de herencia original. Si A es subtipo de B, entonces Tipo<B> se considera subtipo de Tipo<A>.

        + Ejemplo en Java: Se logra con el comodín super. Por ejemplo, List<? super String> puede aceptar una List<Object>. Es útil cuando solo quieres escribir o insertar datos en una estructura.

    - Invariante (Invariancia): Ignora por completo la relación de herencia. Tipo<A> y Tipo<B> son tipos completamente inconexos, sin importar si A hereda de B.

        + Ejemplo en Java: Los genéricos por defecto. List<String> no tiene relación con List<Object>.


## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Respuesta
Un wildcard o comodín (?) representa un "tipo desconocido" en los genéricos de Java. Su función principal es relajar la restricción de invariancia (el hecho de que List<Integer> no sea subtipo de List<Number>) para permitir la covarianza y contravarianza de forma segura.

Diferencia entre ? extends y ? super
Para saber cuál usar, en Java existe una regla de diseño creada por Joshua Bloch (arquitecto de Java) llamada PECS (Producer Extends, Consumer Super):

    1. Covarianza: List<? extends T> (Límite Superior)
        - Qué significa: "Una lista de T o de cualquier clase que herede de T".

        - Para qué se usa (Productor): Se usa cuando la lista va a producir/entregar datos (solo lectura). Al leer, tienes la garantía absoluta de que lo que sacas es, como mínimo, de tipo T.

        - Restricción: No puedes añadir nuevos elementos a la lista (excepto null). El compilador te lo prohíbe porque no sabe de qué subtipo exacto es la lista original (podría ser una List<Double> y tú intentar meterle un Integer).


    2. Contravarianza: List<? super T> (Límite Inferior)
Qué significa: "Una lista de T o de cualquier clase de la que T herede (hasta llegar a Object)".

        - Para qué se usa (Consumidor): Se usa cuando la lista va a consumir/recibir datos (solo escritura). Tienes la garantía de que la lista es lo suficientemente genérica como para aceptar un objeto de tipo T.

        - Restricción: No puedes leer elementos esperando un tipo concreto. Lo único que el compilador te garantiza al sacar un dato es que será un Object.


Ejemplos en código:

Ejemplo 1: ? extends (Leer números y sumarlos)
Aquí usamos el wildcard porque queremos que el método acepte List<Integer>, List<Double>, etc. Solo vamos a leer los datos.

import java.util.List;
import java.util.Arrays;

public class EjemploExtends {
    
    // Acepta una lista de Number o de cualquier hijo suyo
    public static double sumar(List<? extends Number> lista) {
        double suma = 0;
        for (Number num : lista) { // Garantizado que podemos leerlo como Number
            suma += num.doubleValue();
        }
        // lista.add(5); // ERROR DE COMPILACIÓN: No se puede escribir
        return suma;
    }

    public static void main(String[] args) {
        List<Integer> enteros = Arrays.asList(1, 2, 3);
        List<Double> decimales = Arrays.asList(1.5, 2.5);
        
        System.out.println("Suma enteros: " + sumar(enteros));
        System.out.println("Suma decimales: " + sumar(decimales));
    }
}


Ejemplo 2: ? super (Añadir enteros a una lista)
Aquí usamos el wildcard porque queremos meter enteros, y nos sirve cualquier lista que tenga capacidad para albergarlos (una List<Integer>, una List<Number>, o una List<Object>).

import java.util.List;
import java.util.ArrayList;

public class EjemploSuper {
    
    // Acepta una lista de Integer o de cualquiera de sus padres (Number, Object)
    public static void agregarEnteros(List<? super Integer> lista) {
        // Garantizado que podemos escribir/insertar un Integer
        lista.add(10);
        lista.add(20);
        
        // Integer n = lista.get(0); // ERROR DE COMPILACIÓN: No podemos asegurar que devuelva Integer
    }

    public static void main(String[] args) {
        List<Number> listaNumeros = new ArrayList<>();
        List<Object> listaObjetos = new ArrayList<>();
        
        agregarEnteros(listaNumeros); // OK
        agregarEnteros(listaObjetos); // OK
        
        System.out.println(listaNumeros); // Imprime: [10, 20]
    }
}

