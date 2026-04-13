<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

### Respuesta
El polimorfismo en programación orientada a objetos permite que un mismo método tenga diferentes comportamientos según el objeto que lo implemente. Se usa para escribir código más flexible y reutilizable, facilitando la extensión y mantenimiento del software.

La sobreescritura de métodos ocurre cuando una subclase redefine un método heredado de su superclase, manteniendo la misma firma pero cambiando su implementación. Esto permite que una subclase personalice el comportamiento de un método sin alterar la estructura general del programa.


## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### Respuesta
La ligadura dinámica o enlace tardío es el proceso en el que la llamada a un método se resuelve en tiempo de ejecución en lugar de en tiempo de compilación. Se relaciona con el polimorfismo, ya que permite que el método ejecutado dependa del tipo real del objeto y no del tipo de referencia.



En C++, la ligadura dinámica ocurre solo si los métodos son declarados como `virtual`, mientras que en Java es automática para todos los métodos de instancia (excepto los `static` y `final`). En Python, todos los métodos usan ligadura dinámica de forma predeterminada, ya que no hay necesidad de palabras clave como `virtual` o `override`.


## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### Respuesta
class Soldado {
    void saluda() {
        System.out.println("¡Soldado listo para la acción!");
    }
}

class Zapador extends Soldado {
    @Override
    void saluda() {
        System.out.println("¡Zapador listo para despejar el camino!");
    }
}

class Artillero extends Soldado {
    // Usa el saludo estándar de Soldado
}

public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = { new Zapador(), new Artillero() };
        for (Soldado s : ejercito) {
            s.saluda(); // Se resuelve en tiempo de ejecución (ligadura dinámica)
        }
    }
}


**Salida esperada:**

¡Zapador listo para despejar el camino!
¡Soldado listo para la acción!

Aquí, `Zapador` redefine `saluda()`, mientras que `Artillero` hereda el método original de `Soldado`. Al recorrer el array, se usa polimorfismo para llamar a la versión correcta de `saluda()`.


## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Respuesta
Sí, puedes invocar el método de la clase base desde una subclase. En Java, se utiliza la palabra clave `super` para acceder a los métodos y propiedades de la clase base.

En el siguiente ejemplo, la clase `Zapador` invoca el método `saluda()` de la clase `Soldado` y luego añade su propio mensaje:

class Soldado {
    void saluda() {
        System.out.println("¡Soldado listo para la acción!");
    }
}

class Zapador extends Soldado {
    @Override
    void saluda() {
        super.saluda(); // Llama al método saluda() de la clase base
        System.out.println("¡ZAPADOR A SUS ORDENES!");
    }
}

public class Main {
    public static void main(String[] args) {
        Soldado zapador = new Zapador();
        zapador.saluda();
    }
}


**Salida esperada:**
¡Soldado listo para la acción!
¡ZAPADOR A SUS ORDENES!


En este caso, la palabra clave `super` se utiliza para llamar al método `saluda()` de la clase base (`Soldado`) antes de añadir el comportamiento específico de la subclase (`Zapador`).


## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Respuesta
Al sobreescribir un método en Java, existen restricciones estrictas para asegurar que el contrato de la superclase se respete:

1.  **Tipos de los parámetros:** Deben ser **exactamente los mismos** que en el método de la superclase. Si se cambia un solo parámetro, Java lo interpreta como un método distinto (sobrecarga) y no como una sobreescritura.
2.  **Tipo de retorno:** Debe ser el mismo o un **tipo covariante** (un subtipo del retorno original). Por ejemplo, si el padre devuelve `Publicacion`, el hijo puede devolver `Articulo`.
3.  **Visibilidad:** El método en la subclase **no puede ser más restrictivo** que en la superclase. Si el original es `protected`, el nuevo puede ser `protected` o `public`, pero nunca `private`.


Diferencias entre Sobreescritura (*Overriding*) y Sobrecarga (*Overloading*):

La **sobreescritura** ocurre cuando una subclase redefine un método de su padre con la misma firma para cambiar su comportamiento; esto se resuelve en tiempo de ejecución (ligadura dinámica). 

En cambio, la **sobrecarga** ocurre cuando se definen varios métodos con el mismo nombre pero **distintos parámetros** (en cantidad o tipo) dentro de una misma clase; esto se resuelve en tiempo de compilación (ligadura estática).


La anotación `@Override`:

La anotación `@Override` sirve para comunicarle explícitamente al compilador que nuestra intención es redefinir un método heredado. Es altamente recomendable usarla por dos motivos:

* **Evita errores humanos:** Si escribimos mal el nombre del método o nos equivocamos en un parámetro, el compilador nos avisará de que "no estamos sobreescribiendo nada", evitando que el programa se comporte de forma inesperada.
* **Mejora la documentación:** Facilita que cualquier otra persona que lea el código identifique rápidamente qué métodos forman parte del polimorfismo de la jerarquía.


## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Respuesta
Sí, cuando se estudia Java, el polimorfismo se introduce desde el principio, ya que es un concepto clave en la programación orientada a objetos. Al sobreescribir métodos como `toString()` o `equals()`, ya estás utilizando polimorfismo, ya que estás redefiniendo el comportamiento de esos métodos en tus clases para que se adapten a sus necesidades específicas.

Por ejemplo, al sobrescribir `toString()`, puedes modificar cómo se presenta la representación en cadena de un objeto. Cuando invocas `toString()` en un objeto, Java seleccionará la versión correcta (la sobrescrita o la de la clase base) en tiempo de ejecución, lo que es una demostración directa de polimorfismo. Lo mismo ocurre con `equals()`, que se sobrescribe para comparar de manera más apropiada dos instancias de una clase.

En ambos casos, el polimorfismo se emplea de manera implícita, ya que el comportamiento de los métodos depende del tipo de objeto en tiempo de ejecución.


## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Respuesta
Una **clase abstracta** es una clase diseñada exclusivamente para ser heredada; funciona como un "molde" o base común. Su característica principal es que **no puede ser instanciada directamente** (no puedes hacer `new Soldado()`), obligando a usar sus subclases concretas.

Un **método abstracto** es un método que se declara en la clase base pero **no tiene implementación** (no tiene llaves `{}`). Es una forma de obligar a todas las subclases a implementar ese comportamiento específico, asegurando que todos los tipos de la jerarquía sepan responder a esa acción.

Ejemplo en Java:

// El modificador abstract va antes de 'class'
abstract class Soldado {
    
    // El modificador abstract va antes del tipo de retorno. No tiene cuerpo {}.
    abstract void atacar(); 

    void saluda() { // Una clase abstracta puede tener métodos concretos
        System.out.println("¡Soldado listo!");
    }
}

class Zapador extends Soldado {
    @Override
    void atacar() {
        System.out.println("El zapador usa explosivos para atacar.");
    }
}

class Artillero extends Soldado {
    @Override
    void atacar() {
        System.out.println("El artillero dispara desde larga distancia.");
    }
}

public class Main {
    public static void main(String[] args) {
        // Soldado s = new Soldado(); // ERROR: No se puede instanciar
        
        Soldado[] ejercito = { new Zapador(), new Artillero() };

        for (Soldado s : ejercito) {
            s.atacar(); // Polimorfismo: cada uno ataca a su manera
        }
    }
}


Respuestas clave:
* **¿Dónde poner `abstract`?**: Se debe poner antes de la palabra `class` para definir la clase, y antes del tipo de retorno en los métodos que no tengan cuerpo.
* **¿Por qué `atacar` es abstracto?**: Porque no existe una forma "genérica" de atacar para un soldado; cada especialidad (subclase) tiene su propia lógica, pero queremos garantizar que **todos** los soldados puedan atacar.


## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### Respuesta
La palabra clave `final` en Java se utiliza para aplicar restricciones de herencia y modificación. Dependiendo de dónde se use, su efecto es el siguiente:

1. Aplicado a una Clase:
Cuando una **clase** es declarada como `final`, **no puede ser heredada**. Es decir, no se pueden crear subclases de ella. Se utiliza para garantizar la seguridad o para evitar que se altere el comportamiento de una clase diseñada para ser inmutable.

**Ejemplo en la API de Java:** La clase **`String`** es `final`. Esto es así por seguridad y rendimiento; si se pudiera heredar de `String`, alguien podría crear una versión "maliciosa" que alterase el manejo de textos en el sistema.

2. Aplicado a un Método:
Cuando un **método** es declarado como `final`, **no puede ser sobreescrito** (*overridden*) por ninguna subclase. La subclase hereda el método y puede usarlo, pero no puede cambiar su implementación.

3. Relación con el Polimorfismo:
El uso de `final` **limita o anula el polimorfismo**. 
* Si un **método** es `final`, se elimina la ligadura dinámica para ese método, ya que el compilador sabe con total seguridad que no existe otra implementación posible. 
* Si una **clase** es `final`, se impide cualquier tipo de jerarquía y, por tanto, no puede haber polimorfismo basado en herencia con esa clase como base.


Ejemplo de código:
final class SistemaSeguro { // Nadie puede heredar de aquí
    // ...
}

class Soldado {
    // Todos los soldados saludan igual y no quiero que nadie lo cambie
    final void identificarse() { 
        System.out.println("Identificación de soldado obligatoria.");
    }
}

class Zapador extends Soldado {
    // void identificarse() { ... } // ERROR DE COMPILACIÓN: No se puede sobreescribir
}


## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Respuesta
En Java, una **interfaz** es un tipo de referencia especial que define un **contrato de métodos**, pero no proporciona (por lo general) implementaciones. Las interfaces pueden contener métodos abstractos, métodos predeterminados (`default`), constantes (`static final`) y métodos estáticos.


Las interfaces son similares a las clases abstractas en que ambas no pueden ser instanciadas directamente, pero tienen diferencias clave:

1.  **Herencia múltiple:** Una clase puede implementar **múltiples interfaces**, mientras que solo puede heredar de una única clase base (Java no permite la herencia múltiple de clases).
2.  **Estado:** Las interfaces no pueden tener estado (atributos de instancia), mientras que las clases abstractas sí pueden.
3.  **Propósito:** Las interfaces definen un contrato de comportamiento ("qué puede hacer el objeto"), mientras que las clases abstractas ofrecen una base común con implementación parcial ("qué es el objeto").


Ejemplo de interfaz en Java:
interface Atacante {
    void atacar(); // Método abstracto por defecto
}

interface Defensor {
    void defender(); // Método abstracto por defecto
}

// Implementación múltiple
class Soldado implements Atacante, Defensor {
    @Override
    public void atacar() {
        System.out.println("Soldado atacando...");
    }

    @Override
    public void defender() {
        System.out.println("Soldado defendiendo...");
    }
}

public class Main {
    public static void main(String[] args) {
        Soldado soldado = new Soldado();
        soldado.atacar();
        soldado.defender();
    }
}

**Respuestas clave:**
* Una clase puede implementar más de una interfaz en Java, lo que permite combinar múltiples comportamientos.
* A diferencia de las clases abstractas, las interfaces están diseñadas para ser desacopladas de la jerarquía de herencia.


## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### Respuesta
Para resolver este problema, definimos una jerarquía donde la clase base `Punto` obliga a sus hijos a implementar la lógica de distancia, permitiendo que la clase `Linea` trabaje de forma genérica.

1. La jerarquía de Puntos (Abstracción y Downcasting)

La clase `Punto` es abstracta porque la fórmula de la distancia cambia según las dimensiones. Usamos `instanceof` para asegurar que un `Punto2D` solo se compare con otro `Punto2D`.

abstract class Punto {
    // Método polimórfico: cada subtipo sabrá cómo calcular su distancia
    public abstract double calcularDistanciaA(Punto otro);
}

class Punto2D extends Punto {
    private double x, y;

    public Punto2D(double x, double y) { this.x = x; this.y = y; }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (otro instanceof Punto2D) {
            // Downcasting: convertimos la referencia genérica a una específica
            Punto2D p2 = (Punto2D) otro;
            return Math.sqrt(Math.pow(p2.x - this.x, 2) + Math.pow(p2.y - this.y, 2));
        }
        throw new IllegalArgumentException("El punto debe ser 2D");
    }
}

class Punto3D extends Punto {
    private double x, y, z;

    public Punto3D(double x, double y, double z) { this.x = x; this.y = y; this.z = z; }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (otro instanceof Punto3D) {
            Punto3D p3 = (Punto3D) otro;
            return Math.sqrt(Math.pow(p3.x - this.x, 2) + Math.pow(p3.y - this.y, 2) + Math.pow(p3.z - this.z, 2));
        }
        throw new IllegalArgumentException("El punto debe ser 3D");
    }
}


2. La clase Linea (Polimorfismo puro):

La clase `Linea` no sabe si los puntos son 2D o 3D; solo sabe que son objetos de tipo `Punto` y que tienen un método llamado `calcularDistanciaA`. Esto es **desacoplamiento**.

class Linea {
    private Punto puntoA;
    private Punto puntoB;

    public Linea(Punto a, Punto b) {
        this.puntoA = a;
        this.puntoB = b;
    }

    public double obtenerLongitud() {
        // Ligadura dinámica: Java ejecutará la versión 2D o 3D según el objeto real
        return puntoA.calcularDistanciaA(puntoB);
    }
}


Ejemplo de uso:
public class Main {
    public static void main(String[] args) {
        // Línea en 2D
        Linea lineaRana = new Linea(new Punto2D(0,0), new Punto2D(3,4));
        System.out.println("Longitud 2D: " + lineaRana.obtenerLongitud()); // Salida: 5.0

        // Línea en 3D
        Linea lineaEspacial = new Linea(new Punto3D(0,0,0), new Punto3D(1,1,1));
        System.out.println("Longitud 3D: " + lineaEspacial.obtenerLongitud()); // Salida: 1.732...
    }
}

Conceptos clave aplicados:
* **Clase Abstracta:** `Punto` define el contrato pero no la lógica.
* **Instanceof:** Verifica en tiempo de ejecución si el punto recibido es compatible.
* **Downcasting:** Permite acceder a las coordenadas específicas (`x, y, z`) que no existen en la clase padre `Punto`.
* **Polimorfismo:** `Linea` invoca un método que se resuelve de forma distinta dependiendo de si los puntos creados son 2D o 3D.


## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### Respuesta
La **herencia de interfaces** ocurre cuando una interfaz hereda de otra utilizando la palabra clave `extends`. A diferencia de las clases, en las interfaces **sí existe la herencia múltiple**, lo que significa que una interfaz puede extender varias interfaces de forma simultánea.

Esto permite crear jerarquías de comportamientos, donde una interfaz hija acumula los contratos de sus padres, añadiendo además sus propios métodos.


Ejemplo de código:
Fíjate cómo `FicheroEscribible` hereda de `Fichero` y cómo una clase concreta está obligada a implementar toda la cadena de métodos.

// Interfaz base
interface Fichero {
    String leerContenido();
}

// Herencia de interfaces: FicheroEscribible "es un" Fichero
interface FicheroEscribible extends Fichero {
    void escribirContenido(String contenido);
    void eliminar();
}

// Clase que implementa la interfaz hija
class DocumentoTexto implements FicheroEscribible {
    private String texto = "";

    @Override
    public String leerContenido() {
        return "Contenido: " + texto;
    }

    @Override
    public void escribirContenido(String contenido) {
        this.texto = contenido;
        System.out.println("Texto escrito con éxito.");
    }

    @Override
    public void eliminar() {
        this.texto = null;
        System.out.println("Archivo vaciado/eliminado.");
    }
}

public class Main {
    public static void main(String[] args) {
        DocumentoTexto miDoc = new DocumentoTexto();
        
        // Gracias a la herencia, miDoc tiene todos los métodos
        miDoc.escribirContenido("Hola Mundo");
        System.out.println(miDoc.leerContenido());
        miDoc.eliminar();
    }
}


Puntos clave:
* **Reutilización:** `FicheroEscribible` no necesita volver a declarar `leerContenido()`, ya lo tiene por herencia.
* **Herencia múltiple:** Java permite hacer algo como `interface SuperInterfaz extends InterfazA, InterfazB { ... }`. Esto es legal y muy común para combinar capacidades.
* **Polimorfismo:** Un objeto de tipo `DocumentoTexto` puede ser referenciado como `Fichero` (solo para lectura) o como `FicheroEscribible` (para acceso total).

