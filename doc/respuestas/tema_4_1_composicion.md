<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

### Respuesta
#include <stdio.h>
#include <math.h>

typedef struct {
    double x, y;
} Punto;

typedef struct {
    Punto p1, p2;
} Linea;

double distancia(Punto a, Punto b) {
    return sqrt(pow(b.x - a.x, 2) + pow(b.y - a.y, 2));
}

double longitudLinea(Linea l) {
    return distancia(l.p1, l.p2);
}

int main() {
    Punto a = {0, 0}, b = {3, 4};
    Linea linea = {a, b};

    printf("Distancia entre puntos: %.2f\n", distancia(a, b));
    printf("Longitud de la línea: %.2f\n", longitudLinea(linea));
    
    return 0;
}

Este código define Punto con coordenadas (x, y) y Linea con dos puntos. Luego, calcula la distancia entre dos puntos y la longitud de una línea.


## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

### Respuesta
public final class Punto {
    private final double x, y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distanciaA(Punto otro) {
        return Math.sqrt(Math.pow(otro.x - this.x, 2) + Math.pow(otro.y - this.y, 2));
    }

    public double getX() { return x; }
    public double getY() { return y; }
}

public final class Linea {
    private final Punto p1, p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double longitud() {
        return p1.distanciaA(p2);
    }

    public Punto getP1() { return p1; }
    public Punto getP2() { return p2; }
}

public class Main {
    public static void main(String[] args) {
        Punto a = new Punto(0, 0);
        Punto b = new Punto(3, 4);
        Linea linea = new Linea(a, b);
        
        System.out.println("Distancia entre puntos: " + a.distanciaA(b));
        System.out.println("Longitud de la línea: " + linea.longitud());
    }
}

Las clases son inmutables (final), los atributos privados (private final) y no hay setters, garantizando que una vez creados los objetos, no se pueden modificar.


## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### Respuesta
La multiplicidad en la composición indica cuántas instancias de una clase están relacionadas con otra. Se expresa con valores como 1, 0..1, *, 1..*, etc.

En el ejemplo de Linea y Punto, la multiplicidad expresada en ambas direcciones es:

De Linea a Punto (1 a 2): Una Linea está compuesta exactamente por dos Punto.

De Punto a Linea (1 a 1): Cada Punto pertenece a una única Linea (aunque los apuntes matizan que podría reutilizarse en otras líneas si el diseño lo permite, lo que cambiaría esta multiplicidad a *).

En notación UML, se representaría globalmente como 1 ---- 2 (una Linea contiene exactamente 2 Punto).


## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### Respuesta
Composición fuerte (Composición propiamente dicha): Indica una relación en la que el objeto contenido (parte) no puede existir sin el objeto contenedor (todo). Si el contenedor se destruye, sus partes también lo hacen. Ejemplo: un Coche y su Motor.

Composición débil (Agregación o Asociación): La parte puede existir de forma independiente del todo. Si el contenedor se destruye, los objetos contenidos pueden seguir existiendo. Ejemplo: un Equipo y sus Jugadores.

Consecuencia en el ciclo de vida: En la composición fuerte, los objetos contenidos dependen completamente del contenedor y se destruyen con él, mientras que en la composición débil pueden ser compartidos y vivir más allá del contenedor.


## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Respuesta
Hablamos de dependencia, no de composición.

La composición implica una relación estructural fuerte y de larga duración entre los objetos, generalmente a nivel de atributos.

La dependencia ocurre cuando una clase usa otra temporalmente, por ejemplo, recibiéndola como parámetro, devolviéndola en un método o creándola dentro de un método con new. No hay una relación permanente entre los objetos.

Se representa en UML con una flecha discontinua (⭢).


## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### Respuesta
1. Composición Fuerte (los Punto dependen de Linea)
Los Punto se crean dentro de Linea y no pueden existir fuera de ella.

public final class LineaFuerte {
    private final Punto p1, p2;

    public LineaFuerte(double x1, double y1, double x2, double y2) {
        this.p1 = new Punto(x1, y1);
        this.p2 = new Punto(x2, y2);
    }

    public double longitud() {
        return p1.distanciaA(p2);
    }
}

Consecuencia: Los puntos solo existen dentro de LineaFuerte y se destruyen con ella.

2. Composición Débil (los Punto pueden existir fuera de Linea)
Los Punto son creados y gestionados fuera de Linea.

public final class LineaDebil {
    private final Punto p1, p2;

    public LineaDebil(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double longitud() {
        return p1.distanciaA(p2);
    }
}

Consecuencia: Los Punto pueden ser compartidos entre varias líneas y sobreviven aunque la LineaDebil se destruya.


## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### Respuesta
En Java, la destrucción de objetos es gestionada automáticamente por el Garbage Collector (GC).

En la composición fuerte, los objetos contenidos (Punto) no se destruyen explícitamente porque Java no tiene delete como en C++. En su lugar:

    1. Cuando el objeto contenedor (LineaFuerte) deja de ser accesible (por ejemplo, ya no hay referencias a él), el GC lo detecta.

    2. Como los objetos contenidos (Punto) solo eran accesibles desde el contenedor, también quedan inaccesibles.

    3. El GC los libera automáticamente cuando lo considere necesario.

Por eso, en Java no se necesita destruir los objetos manualmente.


## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### Respuesta
public final class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        if (nombre == null || nombre.isBlank()) 
            throw new IllegalArgumentException("El nombre no puede estar vacío");
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

public final class Departamento {
    private static final int MAX_PROFESORES = 50;
    private final Profesor[] profesores;
    private int numProfesores;
    private Profesor director;

    public Departamento(Profesor director) {
        if (director == null) 
            throw new IllegalArgumentException("El director no puede ser nulo");
        
        this.profesores = new Profesor[MAX_PROFESORES];
        this.profesores[0] = director;
        this.numProfesores = 1;
        this.director = director;
    }

    public void agregarProfesor(Profesor profesor) {
        if (profesor == null) 
            throw new IllegalArgumentException("El profesor no puede ser nulo");
        if (numProfesores >= MAX_PROFESORES) 
            throw new IllegalStateException("No caben más profesores");
            
        profesores[numProfesores++] = profesor;
    }

    public void eliminarProfesor(int indice) {
        if (indice < 0 || indice >= numProfesores) 
            throw new IndexOutOfBoundsException("Índice fuera de rango");
        if (profesores[indice] == director) 
            throw new IllegalStateException("No se puede eliminar al director");
            
        for (int i = indice; i < numProfesores - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        profesores[--numProfesores] = null;
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) 
            throw new IllegalArgumentException("El director no puede ser nulo");
        if (!contieneProfesor(nuevoDirector)) 
            throw new IllegalStateException("El nuevo director debe ser un profesor del departamento");
            
        this.director = nuevoDirector;
    }

    private boolean contieneProfesor(Profesor profesor) {
        for (int i = 0; i < numProfesores; i++) {
            if (profesores[i] == profesor) return true;
        }
        return false;
    }

    public Profesor getDirector() {
        return director;
    }

    public int getNumeroProfesores() {
        return numProfesores;
    }

    public Profesor getProfesor(int indice) {
        if (indice < 0 || indice >= numProfesores) 
            throw new IndexOutOfBoundsException("Índice fuera de rango");
            
        return profesores[indice];
    }
}

Explicación:

    - Departamento contiene varios Profesor en un array privado, sin exponerlo.

    - Se puede agregar y eliminar profesores, salvo el director.

    - El director siempre debe estar en la lista; se valida antes de cambiarlo.

Se controlan excepciones para mantener la invariante y evitar errores.


## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Respuesta
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public final class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        if (nombre == null || nombre.isBlank()) 
            throw new IllegalArgumentException("El nombre no puede estar vacío");
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

public final class Departamento {
    private final List<Profesor> profesores;
    private Profesor director;

    public Departamento(Profesor director) {
        if (director == null) 
            throw new IllegalArgumentException("El director no puede ser nulo");
        
        this.profesores = new ArrayList<>();
        this.profesores.add(director);
        this.director = director;
    }

    public void agregarProfesor(Profesor profesor) {
        if (profesor == null) 
            throw new IllegalArgumentException("El profesor no puede ser nulo");
        profesores.add(profesor);
    }

    public void eliminarProfesor(int indice) {
        if (indice < 0 || indice >= profesores.size()) 
            throw new IndexOutOfBoundsException("Índice fuera de rango");
        if (profesores.get(indice) == director) 
            throw new IllegalStateException("No se puede eliminar al director");
            
        profesores.remove(indice);
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) 
            throw new IllegalArgumentException("El director no puede ser nulo");
        if (!profesores.contains(nuevoDirector)) 
            throw new IllegalStateException("El nuevo director debe ser un profesor del departamento");
            
        this.director = nuevoDirector;
    }

    public Profesor getDirector() {
        return director;
    }

    public int getNumeroProfesores() {
        return profesores.size();
    }

    public Profesor getProfesor(int indice) {
        if (indice < 0 || indice >= profesores.size()) 
            throw new IndexOutOfBoundsException("Índice fuera de rango");
        return profesores.get(indice);
    }
    
    // --- SOLUCIÓN AL PROBLEMA DE DEVOLVER LA LISTA ---
    public List<Profesor> getTodosLosProfesores() {
        // Devolvemos una vista de solo lectura para proteger la lista original
        return Collections.unmodifiableList(profesores);
    }
}

Lo que nos hemos ahorrado:
1. El manejo manual del tamaño del array y el contador numProfesores, ya que List expande y reduce la colección automáticamente.

2. La manipulación compleja en eliminarProfesor (hacer un bucle for para desplazar todos los elementos a la izquierda), usando simplemente remove().

3. El método para buscar si un profesor existe, ya que ahora usamos directamente profesores.contains().

El problema de devolver la lista interna:
Si creáramos un método que hiciera return this.profesores;, estaríamos provocando una fuga de referencias y rompiendo la encapsulación. Cualquier clase externa que llame a ese método podría hacer departamento.getTodosLosProfesores().clear() o .add(nuevoProfesor). Esto modificaría la lista interna del departamento saltándose todas nuestras validaciones (como la invariante de que no se puede borrar al director).

Cómo resolverlo:
Nunca se devuelve la lista original. Se debe devolver una vista inmodificable (return Collections.unmodifiableList(profesores);) que lanzará un error si alguien intenta modificarla, o bien una copia defensiva (return new ArrayList<>(profesores);).


## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Respuesta
En este ejemplo, se muestra cómo representar una composición recursiva en Java utilizando una clase Persona, donde cada Persona tiene una referencia a su madre, que es otra Persona. La relación es recursiva porque cada Persona puede tener una madre, que también es una Persona con su propia madre, y así sucesivamente.

// Ejemplo de Java con Persona y Composición Recursiva:
public final class Persona {
    private final String nombre;
    private final Persona madre;

    // Constructor que crea una persona con su madre (puede ser nula para la raíz de la familia)
    public Persona(String nombre, Persona madre) {
        if (nombre == null || nombre.isBlank()) 
            throw new IllegalArgumentException("El nombre no puede estar vacío");
        this.nombre = nombre;
        this.madre = madre;
    }

    public String getNombre() {
        return nombre;
    }

    public Persona getMadre() {
        return madre;
    }

    public String getLineaDeAscendencia() {
        if (madre == null) {
            return nombre; // Si no tiene madre, es la raíz de la línea.
        } else {
            return nombre + " -> " + madre.getLineaDeAscendencia(); // Recursivamente agrega la madre
        }
    }
}

public class Main {
    public static void main(String[] args) {
        // Creando la familia, desde el nieto hasta la abuela.
        Persona abuela = new Persona("Abuela", null); // La abuela no tiene madre.
        Persona madre = new Persona("Madre", abuela); // La madre tiene como madre a la abuela.
        Persona hijo = new Persona("Hijo", madre);    // El hijo tiene como madre a la madre.
        
        // Ejemplo de uso
        System.out.println("Ascendencia de " + hijo.getNombre() + ": " + hijo.getLineaDeAscendencia());
        System.out.println("Ascendencia de " + madre.getNombre() + ": " + madre.getLineaDeAscendencia());
        System.out.println("Ascendencia de " + abuela.getNombre() + ": " + abuela.getLineaDeAscendencia());
    }
}

Explicación:

    - Composición Recursiva: La clase Persona tiene un atributo madre, que es de tipo Persona. Esto crea una relación recursiva, ya que cada Persona puede tener otra Persona como madre.

    - Inmutabilidad: La clase Persona es inmutable porque los atributos (nombre y madre) son finales y solo se inicializan en el constructor.

    - Método Recursivo (getLineaDeAscendencia): Este método construye una cadena con el nombre de la persona y sus ascendientes de manera recursiva, retrocediendo por las madres.

Ejemplo de uso:

    - hijo tiene como madre a madre, y madre tiene como madre a abuela.

    - El resultado en consola sería algo así:
        Ascendencia de Hijo: Hijo -> Madre -> Abuela
        Ascendencia de Madre: Madre -> Abuela
        Ascendencia de Abuela: Abuela

Otros ejemplos clásicos de composiciones recursivas:

    1. Estructuras de árbol (como directorios en un sistema de archivos): Cada directorio puede contener otros directorios, creando una estructura jerárquica recursiva.

    2. Árboles de decisión o árboles binarios: En donde cada nodo tiene dos hijos, y esos hijos pueden ser otros nodos que también tienen hijos.

    3. Cadenas de dependencias: Como cuando un objeto depende de otro objeto, que depende de otro, creando una cadena recursiva de dependencias.

La clave en estas composiciones recursivas es que el objeto puede contener una referencia a sí mismo a través de una relación jerárquica o de dependencia.


## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta
Una relación de composición (o agregación) es bidireccional cuando ambas clases se conocen mutuamente. Es decir, el objeto "contenedor" tiene referencias a sus "partes" (como es habitual), pero las "partes" también tienen un atributo con una referencia de vuelta hacia su "contenedor".

Para que la relación pase de ser unidireccional (el departamento conoce a sus profesores) a bidireccional (el profesor también sabe en qué departamento trabaja), habría que hacer lo siguiente:

1. Añadir la referencia en la parte: En la clase Profesor, añadir un atributo private Departamento departamento; con su respectivo getter y setter.

2. Sincronizar la relación: En la clase Departamento, dentro del método agregarProfesor, justo después de guardar al profesor en la lista, el departamento debe pasarse a sí mismo al profesor: profesor.setDepartamento(this);.

3. Cuidado con los bucles infinitos: Al programar esto, hay que tener mucho cuidado con métodos como toString(). Si el Departamento imprime a sus profesores, y el Profesor imprime su departamento, entrarán en un bucle infinito que colapsará el programa (StackOverflowError).
