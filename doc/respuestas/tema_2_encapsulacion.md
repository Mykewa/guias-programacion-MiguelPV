<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

### Respuesta
En POO, la encapsulación busca agrupar datos y métodos dentro de una clase, mientras que la ocultación de información limita el acceso a los detalles internos de un objeto, exponiendo solo lo necesario.
Ventajas de la ocultación de información:
1.	Seguridad: Protege los datos de modificaciones no autorizadas.
2.	Mantenimiento: Facilita la actualización del código sin afectar otras partes.
3.	Control de acceso: Permite validar cambios mediante métodos.
4.	Reducción del acoplamiento: Hace el código más flexible y reutilizable.
5.	Facilidad en la depuración: Limita el impacto de errores, ya que los cambios solo se realizan mediante interfaces controladas.


## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### Respuesta
La interfaz pública de un objeto o clase en POO es el conjunto de métodos y propiedades accesibles desde el exterior, que permiten interactuar con el objeto sin exponer su implementación interna. Es lo que los usuarios de la clase pueden ver y usar sin necesidad de conocer su funcionamiento interno.
Se relaciona con la ocultación de información porque permite separar la manera en que se usa un objeto de cómo está implementado. Al ocultar los detalles internos (como atributos privados o métodos auxiliares), la interfaz pública actúa como un punto de acceso controlado, garantizando seguridad, flexibilidad y facilidad de mantenimiento.


## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Respuesta
Hay que diseñar con cuidado la interfaz pública de una clase porque define cómo interactúan otras partes del programa con ella. Una mala interfaz puede hacer que el código sea difícil de usar, propenso a errores o difícil de mantener.
No es fácil cambiarla, ya que otros módulos o clases pueden depender de ella. Modificarla puede romper compatibilidad con el código existente, obligando a realizar cambios en múltiples partes del programa. Por eso, una interfaz bien pensada debe ser estable y predecible.


## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Respuesta
Las invariantes de clase son condiciones o reglas que deben mantenerse siempre verdaderas para garantizar la consistencia de un objeto durante su ciclo de vida (como los límites del mapa de minecraft). Estas condiciones suelen aplicarse a los atributos y deben cumplirse después de la creación del objeto y tras cualquier modificación de su estado.
La ocultación de información ayuda a mantener las invariantes de clase porque restringe el acceso directo a los datos internos. Al obligar a modificar los atributos solo a través de métodos controlados, se pueden validar los cambios y asegurar que el objeto siempre permanezca en un estado válido.


## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### Respuesta
Aquí tienes un ejemplo breve de la clase Punto en Java con ocultación de información:
public class Punto {
    private double x, y;

    public Punto(double x, double y) { this.x = x; this.y = y; }

    public double getX() { return x; }
    public double getY() { return y; }
    public void setX(double x) { this.x = x; }
    public void setY(double y) { this.y = y; }

    public double calcularDistanciaAOrigen() { return Math.sqrt(x * x + y * y); }
}
Interfaz pública
•	Métodos: Punto(), getX(), getY(), setX(), setY(), calcularDistanciaAOrigen().
Significado de public y private
•	public: Accesible desde cualquier parte del programa.
•	private: Solo accesible dentro de la clase, protegiendo los datos.


## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta
En Java, los modificadores public y private se pueden aplicar a:
1.	Clases (public únicamente) → Una clase pública es accesible desde cualquier parte del programa.
2.	Atributos (public o private) → Controlan el acceso a las variables dentro de una clase.
3.	Métodos (public o private) → Determinan si un método puede ser llamado desde fuera de la clase.
4.	Constructores (public o private) → Definen si una clase puede ser instanciada desde fuera.
5.	Interfaces (public únicamente) → Son accesibles globalmente.
El modificador private solo permite el acceso dentro de la misma clase, mientras que public permite el acceso desde cualquier parte del programa.


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta
Sí, en POO existen más tipos de visibilidad además de pública y privada.
Visibilidad en Java
1.	public → Accesible desde cualquier parte del programa.
2.	private → Accesible solo dentro de la misma clase.
3.	protected → Accesible dentro de la misma clase, sus subclases y el mismo paquete.
4.	(Sin modificador, "default" o "package-private") → Accesible solo dentro del mismo paquete.
Visibilidad en otros lenguajes
•	C++: Usa public, private, y protected, similares a Java.
•	Python: No tiene modificadores explícitos, pero usa convenciones (_atributo para "protegido" y __atributo para "privado").
•	C#: Además de public, private, y protected, tiene internal (accesible solo dentro del mismo ensamblado) y protected internal (combinación de ambos).
Cada lenguaje adapta los niveles de visibilidad según su paradigma y necesidades.


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta
La respuesta correcta es a) no se pueden acceder desde otras clases, pero sí pueden ser accedidos desde otros objetos de la misma clase.
Aquí tienes un ejemplo con un método calcularDistanciaAPunto(Punto otro), que demuestra que un objeto puede acceder a los miembros privados de otro objeto de la misma clase:
public class Punto {
    private double x, y;

    public Punto(double x, double y) { this.x = x; this.y = y; }

    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.x - otro.x;  // Se accede a x de otro objetodelamisma class
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}
Explicación
Los atributos x e y son private, por lo que no pueden ser accedidos directamente desde otras clases. Sin embargo, dentro de la clase Punto, cualquier objeto Punto puede acceder a los atributos privados de otro objeto Punto, ya que ambos pertenecen a la misma clase.



## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta
Los métodos getter y setter en POO permiten acceder y modificar los atributos privados de una clase de manera controlada.
•	Getter: Método que obtiene el valor de un atributo privado, asegurando que el acceso sea seguro. Usualmente se nombra getNombreAtributo().
•	Setter: Método que establece o modifica el valor de un atributo privado, pudiendo incluir validaciones para asegurar que los valores sean correctos. Se nombra setNombreAtributo().
Propósito:
•	Encapsulamiento: Protege los atributos privados y evita el acceso directo.
•	Control: Permite incluir lógica en los setters para validar o restringir cambios en los datos.


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta
No tiene que ver con “hackear” si no con protección de datos en general, seguridad.

## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta
•	Miembro de instancia: Es un atributo o método que pertenece a una instancia específica de la clase. Cada objeto creado a partir de la clase tiene su propia copia de estos miembros.
•	Miembro de clase: Es un atributo o método que pertenece a la clase en sí, y no a una instancia. Todos los objetos de la clase comparten el mismo valor para estos miembros.
¿Pueden los miembros de clase ser ocultados?
Sí, los miembros de clase también pueden ser ocultados mediante modificadores de acceso (private, protected, public), lo que controla su visibilidad y acceso, de la misma manera que los miembros de instancia.



## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta
Sí, tiene sentido que los constructores sean privados en ciertos casos. Esto se utiliza comúnmente en patrones de diseño como el Singleton, donde se quiere evitar que se creen múltiples instancias de una clase. Al hacer el constructor privado, se restringe la creación de objetos fuera de la propia clase, controlando así la instancia.

## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta
En Java, los miembros de clase se indican con la palabra clave static, lo que significa que son compartidos por todas las instancias de la clase.
Ejemplo con la clase Punto:
public class Punto {
    private double x, y;
    
    // Miembros de clase (static)
    private static double maxX = Double.MIN_VALUE;
    private static double maxY = Double.MIN_VALUE;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        // Actualizar los valores máximos de x e y
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }

    public double getX() { return x; }
    public double getY() { return y; }

    // Métodos de clase para obtener los valores máximos
    public static double getMaxX() { return maxX; }
    public static double getMaxY() { return maxY; }
}
Explicación:
•	Miembros de clase: maxX y maxY son static, compartidos entre todas las instancias de Punto.
•	Actualización de máximos: Cada vez que se crea un nuevo punto, se verifica y actualiza si las coordenadas x e y superan los valores máximos registrados.
•	Acceso estático: Los métodos getMaxX() y getMaxY() permiten acceder a los valores máximos sin crear una instancia de la clase.


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta
public static Punto crearPuntoRedondeado(double x, double y) {
    return new Punto(Math.round(x), Math.round(y));
}
Explicación:
•	El método crearPuntoRedondeado es estático (static), lo que permite llamarlo sin tener que instanciar un objeto de la clase Punto.
•	Usa Math.round(x) y Math.round  para redondear las coordenadas x e y al entero más cercano antes de crear el nuevo objeto Punto.



## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta
public class Punto {
    private double[] coordenadas = new double[2];  // Array interno para almacenar las coordenadas

    public Punto(double x, double y) {
        coordenadas[0] = x;
        coordenadas[1] = y;
    }

    public double getX() {
        return coordenadas[0];  // Accede al primer valor del array
    }

    public double getY() {
        return coordenadas[1];  // Accede al segundo valor del array
    }

    public void setX(double x) {
        coordenadas[0] = x;
    }

    public void setY(double y) {
        coordenadas[1] = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(coordenadas[0] * coordenadas[0] + coordenadas[1] * coordenadas[1]);
    }
}
Explicación:
•	Array de coordenadas: En lugar de dos variables x y y, se utiliza un array coordenadas de dos elementos, donde coordenadas[0] representa x y coordenadas[1] representa y.
•	Métodos getter y setter: Se modifican para acceder y modificar los elementos del array sin cambiar la interfaz pública de la clase.
•	Interfaz pública: La interfaz pública sigue siendo la misma, con los métodos getX(), getY(), setX(), setY(), y calcularDistanciaAOrigen() intactos.


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta
No es recomendable declarar un atributo como público si tiene getter y setter públicos. La convención habitual es hacer los atributos privados para mantener el encapsulamiento y controlar el acceso a los datos mediante los métodos. Esto asegura que puedas validar y proteger las invariantes de clase, es decir, las condiciones que deben mantenerse siempre para que el objeto esté en un estado válido.
Hacer los atributos privados también facilita el mantenimiento y permite cambiar la implementación interna sin afectar a otras partes del código.


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta
Clase inmutable:
Una clase inmutable es aquella cuyo estado no puede cambiar una vez que el objeto ha sido creado. Sus atributos se inicializan en el constructor y no existen métodos que modifiquen su estado.
Método modificador:
Es un método que cambia el estado de un objeto, modificando uno o más de sus atributos.
¿Un método modificador es siempre un setter?
No. Un método modificador no siempre es un setter. Aunque los setters son métodos modificadores comunes, otros métodos pueden modificar el estado de un objeto sin ser setters.
Ventajas de una clase inmutable:
•	Seguridad: No se pueden modificar desde fuera, lo que asegura un comportamiento predecible.
•	Thread-safe: Son seguras en entornos multihilo.
•	Facilidad de uso: Pueden compartirse sin riesgo de alteración accidental.



## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta
No, no es recomendable incluir métodos setter siempre ni como convención. Los setters deben usarse solo cuando sea necesario.
Razones para no incluirlos siempre:
1.	Encapsulamiento: Los setters exponen el estado interno, lo que puede romper el encapsulamiento. A veces es mejor mantener los atributos privados y proporcionar acceso controlado.
2.	Control de cambios: Si un atributo puede modificarse, debe haber una razón clara. En lugar de un setter, se pueden usar métodos que validen o transformen los datos.
3.	Clases inmutables: En clases inmutables, no se usan setters porque el estado de los objetos no cambia después de su creación.
Conclusión:
Los setters deben incluirse solo cuando sea necesario y no como una regla general. Es importante evaluar si realmente se necesita modificar un atributo tras la creación del objeto.



## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta
La clase String en Java es inmutable, lo que significa que una vez que se crea un objeto String, su contenido no puede ser modificado.
¿Qué ocurre al concatenar dos cadenas?
Cuando concatenas dos cadenas en Java, se crea un nuevo objeto String que contiene el resultado de la concatenación. Esto ocurre porque los objetos String son inmutables, por lo que cada operación de concatenación genera un nuevo objeto, lo que puede ser ineficiente si se realizan muchas concatenaciones.
¿Qué hacer si se va a concatenar muchas veces?
Si vas a realizar muchas concatenaciones, es más eficiente utilizar StringBuilder o StringBuffer. Estas clases permiten modificar el contenido de las cadenas sin crear nuevos objetos en cada operación, lo que mejora el rendimiento cuando se construyen cadenas largas paso a paso.


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta
En POO, los objetos de una misma clase se pueden comparar por su identidad o por su contenido, dependiendo del contexto y de cómo se implemente la comparación.
¿Por su contenido o por su identidad?
•	Por identidad: Compara si dos referencias apuntan al mismo objeto en memoria (es decir, si son la misma instancia).
•	Por contenido: Compara si dos objetos tienen el mismo valor en sus atributos.
Método equals en Java:
El método equals en Java se usa para comparar el contenido de dos objetos. Por defecto, en la clase Object, equals compara por identidad (es decir, si las dos referencias apuntan al mismo objeto en memoria). Sin embargo, muchas clases, como String o Integer, sobrescriben este método para comparar los valores de los objetos, no sus direcciones en memoria.
¿Cómo se deben comparar dos cadenas en Java?
En Java, para comparar cadenas correctamente, se debe usar el método equals de la clase String, no el operador ==. El operador == solo comprueba si las dos referencias apuntan al mismo objeto, mientras que equals compara el contenido de las cadenas (el texto que contienen).


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta
Las clases wrapper son clases que envuelven tipos primitivos, permitiendo tratarlos como objetos. Esto es útil en situaciones donde se necesitan objetos, como en colecciones o para utilizar métodos adicionales.
¿Cómo se hace?
Cada tipo primitivo tiene una clase wrapper correspondiente en Java:
•	int → Integer
•	double → Double
•	char → Character
•	boolean → Boolean
¿Es un proceso automático?
Sí, Java tiene autoboxing, que convierte automáticamente entre tipos primitivos y sus clases wrapper. Por ejemplo:
int x = 5;
Integer y = x;  // Autoboxing
¿Qué ventajas tienen?
1.	Uso en colecciones: Permiten almacenar tipos primitivos en colecciones, ya que estas solo aceptan objetos.
2.	Métodos útiles: Las clases wrapper proporcionan métodos para manipular valores, como conversiones o comparaciones.
3.	Interoperabilidad: Permiten que los primitivos actúen como objetos en bibliotecas y frameworks.
¿Todos los lenguajes orientados a objetos necesitan wrappers?
No, lenguajes como Python o Ruby no distinguen entre tipos primitivos y objetos, tratándolo todo como objeto. En cambio, lenguajes como Java, C# o C++ utilizan wrappers para tratar los primitivos como objetos cuando es necesario.


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta
Un tipo de dato enumerado (o enum) es un tipo que define un conjunto limitado de valores constantes predefinidos, como los días de la semana o los estados de un proceso.
¿En Java, un tipo de dato enumerado es una clase?
Sí, en Java, los enumerados se implementan como clases especiales que extienden java.lang.Enum. Aunque se definen con la palabra clave enum, internamente son tratados como clases con características de clase, como métodos y campos adicionales.
¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?
1.	Control sobre los valores: Al ser clases, los enumerados pueden tener atributos y métodos adicionales, lo que permite gestionar de manera más precisa sus valores.
2.	Seguridad: Los valores posibles están limitados y definidos dentro del enum, garantizando que solo se usen esos valores y evitando errores de asignación.
3.	Métodos y lógica asociada: Pueden incluir métodos y lógica personalizada que pueden ser específicos para cada valor, lo que mejora la modularidad y el mantenimiento del código.
En resumen, los enumerados en Java ofrecen control, seguridad y flexibilidad al asociar valores constantes con comportamientos específicos.


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado.

### Respuesta
public enum Mes {
    ENERO(31), FEBRERO(28), MARZO(31), ABRIL(30), MAYO(31), JUNIO(30),
    JULIO(31), AGOSTO(31), SEPTIEMBRE(30), OCTUBRE(31), NOVIEMBRE(30), DICIEMBRE(31);

    private final int dias;
    private final int ordinal;

    Mes(int dias) {
        this.dias = dias;
        this.ordinal = this.ordinal() + 1;
    }

    public int getDias() { return dias; }
    public int getOrdinal() { return ordinal; }
}


## 24. Añade a la clase `Mes` del ejercicio anterior cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta
public boolean esDePrimavera(boolean enHemisferioNorte) {
    return (enHemisferioNorte ? this == MARZO || this == ABRIL || this == MAYO : this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE);
}

public boolean esDeVerano(boolean enHemisferioNorte) {
    return (enHemisferioNorte ? this == JUNIO || this == JULIO || this == AGOSTO : this == DICIEMBRE || this == ENERO || this == FEBRERO);
}

public boolean esDeOtoño(boolean enHemisferioNorte) {
    return (enHemisferioNorte ? this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE : this == MARZO || this == ABRIL || this == MAYO);
}

public boolean esDeInvierno(boolean enHemisferioNorte) {
    return (enHemisferioNorte ? this == DICIEMBRE || this == ENERO || this == FEBRERO : this == JUNIO || this == JULIO || this == AGOSTO);
}