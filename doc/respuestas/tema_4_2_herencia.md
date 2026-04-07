<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

### Respuesta
La herencia en orientación a objetos permite que una clase (subclase) adquiera atributos y métodos de otra (superclase). La relación “A es-un B” indica que una subclase es un tipo especializado de la superclase.

Compatibilidad de tipos: Una instancia de una subclase puede ser tratada como una instancia de su superclase. Esto permite almacenar objetos de diferentes subclases en una misma estructura de datos (como un array de la superclase).

Herencia de estado y comportamiento: Las subclases heredan atributos y métodos de la superclase, pero pueden añadir nuevos o sobrescribir los heredados.

class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Soy " + nombre + ", un soldado.");
    }
}

class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre); // Llama al constructor de la superclase (Soldado)
        this.cohetes = cohetes;
    }

    public int getCohetes() {
        return cohetes;
    }
}

class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public int getMinas() {
        return minas;
    }
}

public class Ejercito {
    public static void main(String[] args) {
        // Polimorfismo y compatibilidad de tipos en acción
        Soldado[] soldados = {
            new Artillero("Juan", 5),
            new Zapador("Carlos", 3),
            new Artillero("Ana", 2)
        };

        for (Soldado s : soldados) {
            s.saludar();
        }
    }
}
Explicación: Cada objeto de Artillero y Zapador puede almacenarse en el array de Soldado porque son compatibles con él (un Artillero es un Soldado), y pueden invocar métodos heredados como saludar().


## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### Respuesta
1. Número y orden de ejecución de constructores
Ejecución doble: Por cada objeto de una subclase que se crea, se ejecutan dos constructores: primero el de la superclase (Soldado) y luego el de la subclase (Artillero o Zapador).

Prioridad de la superclase: El constructor de la superclase se ejecuta antes porque la subclase necesita que los atributos heredados estén inicializados correctamente antes de poder gestionar sus propios atributos específicos.

2. Significado de super
Llamada explícita: La instrucción super(...) invoca directamente al constructor de la superclase. Es la herramienta que permite pasarle al "padre" los datos que necesita (como el nombre en el ejemplo del soldado) antes de seguir con la lógica propia del "hijo".

3. Llamada obligatoria a super
Regla de Java: Si la superclase tiene definido un constructor con parámetros (y no tiene uno vacío o por defecto), es obligatorio escribir super(...) en la primera línea del constructor de la subclase.

Consecuencia: Si no se hace, el código no compilará, ya que Java no sabrá cómo inicializar la parte de la superclase de forma automática.


## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### Respuesta
Respecto a los objetos en memoria, sí, los atributos privados de la superclase forman parte física de la instancia de la subclase. Cuando se crea un objeto Artillero, en el heap se reserva un único bloque de memoria que contiene tanto los atributos definidos en Soldado (como nombre) como los específicos de Artillero (como cohetes). El objeto es una unidad compacta que engloba toda la jerarquía de su herencia.

Sin embargo, que existan en la memoria no implica que se puedan usar directamente desde el código de la subclase. Al estar declarados como private en Soldado, el principio de ocultación de información sigue vigente: el código dentro de Artillero no puede hacer this.nombre = "Juan". La única forma que tiene la subclase de interactuar con esos datos es a través de la interfaz pública (métodos public o protected) que proporcione la superclase, o mediante el constructor usando super.

En el ejemplo de Soldado, aunque un Zapador tiene el atributo nombre dentro de su estructura en memoria, si intentamos acceder a él desde un método de Zapador, el compilador de Java dará un error. Esto garantiza que la superclase mantenga el control total sobre sus propios datos y sus invariantes, obligando a la subclase a respetar las reglas de acceso establecidas por el "padre".


## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### Respuesta
La compatibilidad de tipos permite que las subclases sean tratadas como instancias de su superclase, lo que mejora drásticamente la extensibilidad del código. Gracias a esto, si queremos agregar nuevos tipos de soldados, no necesitamos modificar el código que ya los gestiona (como el array o el bucle que los recorre).

A continuación, añadimos un nuevo tipo de soldado, Francotirador, con un atributo específico para el número de balas:

class Francotirador extends Soldado {
    private int balas;

    public Francotirador(String nombre, int balas) {
        super(nombre);
        this.balas = balas;
    }

    public int getBalas() {
        return balas;
    }
}
Ahora lo agregamos al array de soldados, observando que el código que realiza el recorrido y las acciones (saludar) permanece intacto:

public class Ejercito {
    public static void main(String[] args) {
        Soldado[] soldados = {
            new Artillero("Juan", 5),
            new Zapador("Carlos", 3),
            new Artillero("Ana", 2),
            new Francotirador("Luis", 10) // Nuevo tipo añadido sin problemas
        };

        for (Soldado s : soldados) {
            s.saludar(); // Este código NO cambia, es agnóstico al tipo de subclase
        }
    }
}

Conclusión: El Principio Abierto/Cerrado
Esto demuestra la extensibilidad: podemos añadir nuevas subclases sin afectar al código que usa la superclase (Soldado).

Este concepto es la base del Principio Abierto/Cerrado (Open/Closed Principle): el software debe estar abierto a extensiones (podemos añadir nuevas clases como Francotirador) pero cerrado a modificaciones (no tenemos que tocar el bucle for ni la lógica del Ejercito cada vez que ampliamos la jerarquía).


## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### Respuesta
Referencia del supertipo apuntando a un subtipo: Sí, una variable de tipo Soldado puede referenciar un objeto de Artillero o Zapador gracias a la compatibilidad de tipos.

Invocar métodos del subtipo desde el supertipo: Solo si el método está en la superclase. Si el método es específico del subtipo, es necesario hacer un downcasting para acceder a él.

Upcasting y Downcasting:

Upcasting: Convertir un objeto de una subclase a una referencia del supertipo (implícito y seguro).

Downcasting: Convertir una referencia del supertipo a su subtipo original (explícito y puede fallar en tiempo de ejecución).

instanceof: Operador que verifica si un objeto es una instancia de una clase específica.

Ejemplo recorriendo un array de Soldado y accediendo al número de cohetes de los Artillero:

for (Soldado s : soldados) {
    s.saludar();
    if (s instanceof Artillero) {
        Artillero a = (Artillero) s; // Downcasting
        System.out.println("Tengo " + a.getCohetes() + " cohetes.");
    }
}
Esto asegura que solo los Artillero accedan a su método específico, sin errores de ejecución

## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### Respuesta
El acceso protegido (protected) permite que métodos y atributos sean accesibles dentro de la misma clase, sus subclases y otras clases del mismo paquete. Es una forma intermedia entre private (solo accesible dentro de la clase) y public (accesible desde cualquier parte).

En Java, se implementa con la palabra clave protected:

class Soldado {
    protected String nombre; // Visible en subclases y paquete

    protected void saludar() {
        System.out.println("Soy " + nombre + ", un soldado.");
    }
}
Aquí, nombre y saludar() pueden ser usados directamente en subclases, sin necesidad de métodos getter o setter, pero no desde clases externas de otro paquete.


## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta
1. ¿Existe una clase base universal?
    - En algunos lenguajes como Java y C#, todas las clases heredan implícitamente de una clase base universal (Object en Java, object en C#).

    - En otros lenguajes como C++, no hay una clase base universal obligatoria.

2. ¿Qué ocurre en Java?
    - En Java, todas las clases heredan implícitamente de java.lang.Object, incluso si no se especifica.

    - Object proporciona métodos básicos como toString(), equals() y hashCode(), que pueden ser sobrescritos en las subclases.


## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta
La herencia múltiple es cuando una clase puede heredar directamente de más de una superclase, combinando atributos y métodos de múltiples fuentes.

En Java, no existe herencia múltiple de clases para evitar problemas como la ambigüedad del diamante (cuando dos superclases tienen métodos con el mismo nombre y la subclase no sabe cuál usar).

Sin embargo, Java permite herencia múltiple de interfaces, ya que las interfaces solo declaran métodos sin implementación (hasta Java 8, donde pueden tener métodos default). Esto evita conflictos porque las clases que implementan múltiples interfaces deben definir su comportamiento explícitamente.


## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### Respuesta
Aquí tienes una excepción personalizada UsuarioNoEncontradoException, que es no controlada (extiende RuntimeException). Incluye un objeto Usuario para identificar el usuario problemático y permite agregar una causa subyacente mediante un constructor sobrecargado.

class Usuario {
    private String nombre;

    public Usuario(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario;

    public UsuarioNoEncontradoException(Usuario usuario) {
        super("Usuario no encontrado: " + usuario.getNombre());
        this.usuario = usuario;
    }

    public UsuarioNoEncontradoException(Usuario usuario, Throwable cause) {
        super("Usuario no encontrado: " + usuario.getNombre(), cause);
        this.usuario = usuario;
    }

    public Usuario getUsuario() {
        return usuario;
    }
}
Con esto, puedes lanzar la excepción con o sin causa subyacente y obtener información sobre el usuario involucrado.


## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Respuesta
No se debe usar herencia solo para reutilizar código porque:

Acoplamiento fuerte: La subclase depende fuertemente de la superclase. Si la superclase cambia, puede romper el código de las subclases.

Rigidez y falta de flexibilidad: La herencia define una relación "es-un", lo que puede llevar a estructuras forzadas si solo se usa para compartir código.

Problemas de mantenimiento: Si una subclase hereda métodos innecesarios o no deseados, puede ser difícil modificar o extender el código sin afectar otras partes.

Composición es preferible cuando una clase necesita reutilizar funcionalidades sin estar fuertemente acoplada a otra. Se usa cuando la relación es "tiene-un" en lugar de "es-un", lo que permite mayor flexibilidad y facilidad de cambios.


## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Respuesta
Se dice que se debe favorecer la composición frente a la herencia porque:

Mayor flexibilidad: La composición permite cambiar o extender comportamientos sin afectar a las clases existentes. Puedes cambiar o añadir nuevas funcionalidades fácilmente sin modificar la estructura jerárquica de clases, lo cual no es posible con la herencia, que es más rígida.

Desacoplamiento: La composición favorece un diseño con menor acoplamiento entre clases. Las clases no dependen directamente de otras, lo que facilita el mantenimiento y la evolución del código.

Evita problemas de herencia múltiple: La herencia puede crear complejas relaciones jerárquicas difíciles de manejar (especialmente en lenguajes que no permiten herencia múltiple, como Java), mientras que la composición permite combinar diferentes comportamientos de forma más sencilla y sin complicaciones.

En resumen: la composición permite crear clases más modulares, reutilizables y fáciles de modificar sin crear dependencias excesivas, mientras que la herencia tiende a ser más rígida y menos flexible para cambiar o extender el comportamiento.


## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta
Se dice que la herencia rompe la encapsulación porque:

Acceso a detalles internos: Cuando una clase hereda de otra, la subclase tiene acceso a los miembros protegidos (protected) de la superclase. Esto puede exponer detalles internos que deberían permanecer ocultos, violando el principio de encapsulación, que busca proteger el estado interno de un objeto y limitar el acceso directo a sus atributos.

Dependencia en la implementación interna: Las subclases dependen de la implementación interna de la superclase. Si la superclase cambia su implementación (por ejemplo, cambia el comportamiento de un método), puede afectar a las subclases, lo que crea una relación más frágil y difícil de mantener, comprometiendo la encapsulación.

Desacoplamiento: Con la herencia, las subclases quedan estrechamente acopladas a la implementación de la superclase, lo que reduce la capacidad de ocultar detalles y hace más difícil modificar la superclase sin afectar a las subclases. Esto va en contra de la idea de encapsular los detalles internos y proporcionar interfaces bien definidas.

Por estas razones, la composición suele ser preferida, ya que permite a los objetos delegar responsabilidades sin exponer directamente sus detalles internos.


## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta
1. Modelo usando Herencia (Superclase Persona)

class Persona {
    private String dni;
    private String nombre;

    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() { return dni; }
    public String getNombre() { return nombre; }
}

class Estudiante extends Persona {
    public Estudiante(String dni, String nombre) {
        super(dni, nombre);
    }
}

class Trabajador extends Persona {
    public Trabajador(String dni, String nombre) {
        super(dni, nombre);
    }
}
En este caso, Estudiante y Trabajador heredan de la clase Persona y comparten el mismo comportamiento de los atributos dni y nombre.

2. Modelo usando Composición (Clase DatosPersonales)

class DatosPersonales {
    private String dni;
    private String nombre;

    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() { return dni; }
    public String getNombre() { return nombre; }
}

class Estudiante {
    private DatosPersonales datos;

    public Estudiante(DatosPersonales datos) {
        this.datos = datos;
    }

    public String getDni() { return datos.getDni(); }
    public String getNombre() { return datos.getNombre(); }
}

class Trabajador {
    private DatosPersonales datos;

    public Trabajador(DatosPersonales datos) {
        this.datos = datos;
    }

    public String getDni() { return datos.getDni(); }
    public String getNombre() { return datos.getNombre(); }
}
En este segundo modelo, Estudiante y Trabajador no heredan de una clase común, sino que cada uno tiene un objeto DatosPersonales, lo que permite reutilizar los mismos datos sin una jerarquía estricta.

Comparación
Herencia: Las clases Estudiante y Trabajador son subclases de Persona, lo que las une bajo una jerarquía común. Esto es útil si las dos entidades comparten muchos comportamientos comunes, pero puede hacer que la relación entre clases sea rígida.

Composición: Las clases Estudiante y Trabajador contienen un objeto de la clase DatosPersonales, lo que permite mayor flexibilidad. No están acopladas a una jerarquía común y pueden evolucionar de manera independiente.
