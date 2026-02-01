<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

### Respuesta
Las cuatro características básicas de la programación orientada a objetos (POO) son:
1.	Abstracción: Permite representar entidades del mundo real en términos de clases y objetos, ocultando detalles internos y exponiendo solo lo esencial para su uso.
2.	Encapsulamiento: Restringe el acceso directo a los datos de un objeto y solo permite modificarlos mediante métodos definidos, lo que mejora la seguridad y modularidad del código.
3.	Herencia: Permite que una clase (subclase) herede atributos y métodos de otra (superclase), favoreciendo la reutilización de código y la organización jerárquica.
4.	Polimorfismo: Permite que una misma interfaz o método tenga diferentes comportamientos según el contexto o el objeto que lo implemente, facilitando la flexibilidad y extensibilidad del software.


## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Respuesta
Cuatro lenguajes populares que permiten la programación orientada a objetos son:
1.	Java – Es completamente orientado a objetos y muy utilizado en aplicaciones empresariales, móviles y web.
2.	C++ – Soporta POO junto con programación estructurada, siendo común en sistemas de alto rendimiento.
3.	Python – Ofrece un enfoque flexible hacia la POO y es ampliamente usado en desarrollo web, ciencia de datos e inteligencia artificial.
4.	C# – Diseñado por Microsoft, es utilizado principalmente en desarrollo de aplicaciones Windows y videojuegos con Unity.


## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Respuesta
La programación estructurada es un paradigma que organiza el código en bloques lógicos usando estructuras de control como secuencias, decisiones (if-else) e iteraciones (bucles). Su objetivo es mejorar la legibilidad y mantenimiento del código, evitando el uso excesivo de instrucciones como goto.
La programación modular es una evolución de la estructurada que divide un programa en módulos independientes y reutilizables. Cada módulo encapsula una funcionalidad específica y puede ser desarrollado, probado y mantenido por separado, facilitando la escalabilidad del software.



## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Respuesta
Un objeto en programación orientada a objetos se define por tres elementos principales:
1.	Estado: Representado por los atributos o propiedades del objeto, que almacenan su información en un momento dado.
2.	Comportamiento: Definido por los métodos o funciones del objeto, que determinan las acciones que puede realizar.
3.	Identidad: Es la referencia única que distingue a un objeto de otros, incluso si tienen el mismo estado y comportamiento.


## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Respuesta
Una clase es un molde o plantilla que define las propiedades (atributos) y comportamientos (métodos) de un conjunto de objetos. No es lo mismo que un objeto; un objeto es una entidad concreta creada a partir de una clase.
Una instancia es un objeto específico creado a partir de una clase. Cuando se crea una instancia, se reserva memoria y se inicializan sus atributos según lo definido en la clase.
No todos los lenguajes orientados a objetos manejan el concepto de clase. Algunos, como Java y C#, son basados en clases, mientras que otros, como JavaScript y Python, permiten una orientación a objetos basada en prototipos o instancias sin necesidad de clases.


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Respuesta
Los objetos generalmente se almacenan en el heap, un área de memoria dinámica reservada para datos que pueden cambiar en tiempo de ejecución. Sin embargo, la ubicación exacta puede variar según el lenguaje y el sistema de administración de memoria.
No es igual en todos los lenguajes. En C++, por ejemplo, los objetos pueden crearse en el stack (si son variables locales) o en el heap (si se asignan dinámicamente con new). En lenguajes como Java y Python, los objetos siempre se crean en el heap, y su acceso se gestiona mediante referencias.
La recolección de basura (garbage collection) es un proceso automático en algunos lenguajes que libera memoria ocupada por objetos que ya no tienen referencias activas, evitando fugas de memoria. Java, Python y C# tienen recolectores de basura, mientras que en C++ la gestión de memoria debe hacerse manualmente.


## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Respuesta
Un método es una función definida dentro de una clase que representa el comportamiento de los objetos de esa clase. Permite manipular los atributos del objeto e interactuar con otros objetos.
La sobrecarga de métodos es una técnica que permite definir múltiples métodos con el mismo nombre dentro de una clase, pero con diferentes parámetros (tipo o cantidad). Esto facilita la flexibilidad y legibilidad del código, ya que se pueden invocar métodos con el mismo nombre pero con distintas entradas. La sobrecarga es común en lenguajes como Java y C++, pero no está presente en todos los lenguajes orientados a objetos.


## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### Respuesta
class Punto {
    int x, y;

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}
En este código:
•	x e y son los atributos con visibilidad por defecto (package-private).
•	calculaDistanciaAOrigen usa el teorema de Pitágoras para calcular la distancia al origen (0,0).


## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Respuesta
El punto de entrada en Java es el método main:
public static void main(String[] args)
¿Qué es static?
Indica que un método o variable pertenece a la clase, no a instancias. Se accede sin crear objetos.
¿Solo se usa en main?
No, también en métodos y variables de clase, como Math.sqrt() o Math.PI.
¿Para qué se combina con final?
Define constantes de clase, valores fijos accesibles sin instancias:
static final double PI = 3.1416;
Esto es útil para valores globales que deben permanecer constantes en todo el programa.


## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta
Para compilar y ejecutar un programa en Java desde la línea de comandos, sigue estos pasos:
1.	Escribe el código fuente en un archivo, por ejemplo, Punto.java.
2.	Compila el archivo con javac:
3.	javac Punto.java
Esto generará un archivo .class (por ejemplo, Punto.class), que contiene el bytecode que la máquina virtual de Java puede ejecutar.
4.	Ejecuta el programa con java:
5.	java Punto
Aquí, Punto es el nombre de la clase que contiene el método main.
¿Java es compilado?
Java no es directamente compilado a código de máquina, sino a bytecode. Este bytecode es independiente de la plataforma y se ejecuta en la Máquina Virtual de Java (JVM).
¿Qué es la máquina virtual?
La JVM (Java Virtual Machine) es una máquina virtual que interpreta el bytecode y lo ejecuta en cualquier sistema operativo, asegurando que el programa Java sea independiente de la plataforma (escribe una vez, ejecuta en cualquier lugar).


## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta
¿Qué es new?
new crea una nueva instancia de una clase, asignando memoria y llamando a su constructor.
¿Qué es un constructor?
Es un método especial que se ejecuta al crear un objeto, inicializando sus atributos. Si no se define, Java usa un constructor por defecto.
Ejemplo en la clase Punto:
Punto(int x, int y) {  
    this.x = x;  
    this.y = y;  
}


## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta
La referencia this en Java hace referencia al objeto actual. Se utiliza dentro de un método o constructor para acceder a los atributos y métodos de la instancia en la que se está ejecutando.
No se llama igual en todos los lenguajes. En Python se usa self y en JavaScript, aunque también se usa this, su comportamiento puede variar dependiendo del contexto.


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta
Aquí tienes la clase Punto con el nuevo método distanciaA, que calcula la distancia entre el objeto actual (this) y otro Punto proporcionado como parámetro:
class Punto {
    int x, y;

    // Constructor
    Punto(int x, int y) {
        this.x = x;
        this.y = y;
    }

    // Método para calcular la distancia al origen
    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    // Nuevo método para calcular la distancia entre dos puntos
    double distanciaA(Punto otroPunto) {
        return Math.sqrt(Math.pow(this.x - otroPunto.x, 2) + Math.pow(this.y - otroPunto.y, 2));
    }
}
El método distanciaA recibe un objeto Punto como parámetro y calcula la distancia entre el objeto actual y el punto proporcionado usando el teorema de Pitágoras.


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta
Paso por referencia y por valor en Java
•	Paso de objeto (como Punto): En Java, se pasa la referencia al objeto, no el objeto completo. Si se modifica un atributo dentro del método, esos cambios afectan al objeto original fuera de la función.
•	Paso de tipo primitivo (como int): Se pasa por valor. Esto significa que el valor se copia y cualquier cambio dentro del método no afecta al valor original fuera de la función.
Ejemplo
•	Si modificas un atributo de un Punto dentro de un método, se verá reflejado en el objeto original.
•	Si modificas un int, el cambio no afectará al valor original fuera del método.



## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta
El método toString() en Java es un método especial que se encuentra en la clase base Object, y se utiliza para obtener una representación en forma de cadena de texto (String) del objeto. Este método puede ser sobrescrito en una clase para proporcionar una descripción más significativa del objeto cuando se convierte a cadena.
El método toString() existe en otros lenguajes de programación, pero la implementación y el propósito exacto pueden variar. Por ejemplo, en C# también existe un método ToString() con una funcionalidad similar.
Aquí tienes un ejemplo de cómo sobrescribir el método toString() en la clase Punto para que devuelva una representación más legible del objeto:
class Punto {
    int x, y;

    // Constructor
    Punto(int x, int y) {
        this.x = x;
        this.y = y;
    }

    // Método toString sobrescrito
    @Override
    public String toString() {
        return "Punto(" + x + ", " + y + ")";
    }
}
En este caso, el método toString() devolvería una cadena como "Punto(3, 4)", donde x y y son los valores de los atributos del objeto. Al imprimir un objeto Punto en Java, se invocará automáticamente toString() para representar el objeto como una cadena.


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### Respuesta
Una clase en Java y un struct en C tienen similitudes, ya que ambos agrupan atributos bajo un mismo tipo. Sin embargo, las clases en Java son más poderosas:
•	Métodos: Las clases pueden tener métodos que definen comportamientos asociados a los datos, mientras que un struct en C solo agrupa datos.
•	Encapsulamiento: Java permite controlar el acceso a los atributos con modificadores como private, mientras que en C los miembros de un struct son públicos por defecto.
•	Constructores y OOP: Las clases tienen constructores, herencia y polimorfismo, características que no existen en los structs de C.
En resumen, los structs son más simples y no ofrecen las ventajas de la programación orientada a objetos que tienen las clases en Java.


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta
Para emular la clase Punto con un struct en C, tendríamos que usar un struct para almacenar los atributos (como x e y) y luego crear una función que opere sobre ese struct, como el método calculaDistanciaAOrigen() en Java. Aquí te muestro cómo se podría hacer:
#include 
#include 

// Definición del struct
typedef struct {
    int x, y;
} Punto;

// Función que calcula la distancia al origen
double calculaDistanciaAOrigen(Punto p) {
    return sqrt(p.x * p.x + p.y * p.y);
}

int main() {
    // Crear una instancia de Punto
    Punto p1 = {3, 4};

    // Calcular y mostrar la distancia al origen
    printf("La distancia al origen es: %f\n", calculaDistanciaAOrigen(p1));

    return 0;
}
Explicación:
1.	Struct: Definimos un struct Punto con dos atributos, x e y.
2.	Función: Creamos una función calculaDistanciaAOrigen, que recibe un Punto y calcula la distancia al origen usando el teorema de Pitágoras.
3.	Main: En la función main, creamos un objeto Punto (p1) y pasamos ese objeto a la función para obtener la distancia.
Aunque los structs no tienen métodos como las clases, con funciones separadas puedes lograr un comportamiento similar en C.
