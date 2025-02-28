# respuestas-examen-backend


Respuesta al examen backend
1. ***¿Cuál es la diferencia entre var y un tipo explícito en Java 11 y Java 17? ***

Explica cómo se manejan las inferencias de tipo en Java 11 y Java 17 al utilizar var. ¿Hay diferencias significativas entre ambas versiones? Da ejemplos. 

1️⃣ Diferencia entre var y un tipo explícito en Java 11 y Java 17
En Java 11 y Java 17, var permite la inferencia de tipos en variables locales, pero no cambia el tipado fuerte de Java.

📌 Diferencias principales:

var: Se usa solo en variables locales y en parámetros de lambda (Java 11).
Tipos explícitos: Se requieren en atributos de clase, parámetros de métodos y retornos de métodos.
💡 Ejemplo de var en Java 11 y 17 (sin diferencias en inferencia):

```=java
var lista = List.of("Java", "Spring", "Micronaut"); // Se infiere como List<String>
var numero = 10; // Se infiere como int

```

❌ Casos donde var NO se puede usar:

```=java
// No permitido en atributos de clase
class Ejemplo {
    var nombre; // ERROR: No se permite `var` en campos de clase
}

// No permitido en parámetros de método
void procesar(var dato) { } //  ERROR


```

 Java 11 vs Java 17:
No hay cambios en la inferencia de var. La diferencia más importante en Java 17 es el Pattern Matching para switch, que mejora el uso de inferencias en expresiones

2. ***Explique la diferencia entre un Record y una Class tradicional en Java. ***

¿Qué ventajas ofrece un Record en comparación con una clase convencional? ¿En qué escenarios es preferible usar un Record? 

2️⃣ Diferencia entre Record y Class en Java
En Java 14 (experimental) y Java 16 (estable), se introdujeron los record, que simplifican la creación de clases inmutables.

```=java
// Record
public record Cliente(String id, String nombre, String email) {}

// Clase tradicional
public class Cliente {
    private final String id;
    private final String nombre;
    private final String email;

    public Cliente(String id, String nombre, String email) {
        this.id = id;
        this.nombre = nombre;
        this.email = email;
    }

    public String getId() { return id; }
    public String getNombre() { return nombre; }
    public String getEmail() { return email; }
}


```
¿Cuándo usar un record?

Modelado de datos inmutables (DTOs, respuestas de APIs, eventos en sistemas reactivos).
Cuando solo se necesita almacenar datos sin lógica adicional.





3. ***¿Qué es un sealed class en Java 17? ¿Cómo se usa y qué ventajas ofrece? ***

Explica cómo funcionan las clases selladas en Java 17 y proporciona un ejemplo de cómo se pueden utilizar para modelar jerarquías de clases. 

3️⃣ ¿Qué es una sealed class en Java 17?
Las clases selladas (sealed) restringen qué clases pueden heredar de ellas. Se introdujeron en Java 15 (experimental) y Java 17 (estable).

 Ventajas:

Mayor control de herencia.
Optimización en switch con Pattern Matching.
Mejora la seguridad y mantenibilidad.
 Ejemplo de sealed class:

 ```=java
public sealed class Vehiculo permits Carro, Moto {}

final class Carro extends Vehiculo {}
final class Moto extends Vehiculo {}


```

Beneficio clave:
Se puede hacer switch con Pattern Matching sin necesidad de default:

 ```=java
void procesarVehiculo(Vehiculo v) {
    switch (v) {
        case Carro c -> System.out.println("Es un carro");
        case Moto m -> System.out.println("Es una moto");
    }
}



```

4. ***En Java 11, se introdujo la API HttpClient. Explica sus principales características. ¿Cómo se realiza una solicitud GET en un servidor usando la API HttpClient en Java 11? ***

4️⃣ API HttpClient en Java 11
📌 Características clave:

Basado en java.net.http.HttpClient.
Soporta HTTP/2 y WebSockets.
Uso de asincronía con CompletableFuture.
💡 Ejemplo de una petición GET con HttpClient:

 ```=java
HttpClient client = HttpClient.newHttpClient();
HttpRequest request = HttpRequest.newBuilder()
    .uri(URI.create("https://jsonplaceholder.typicode.com/posts/1"))
    .GET()
    .build();

HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());

System.out.println(response.body());




```

5. ***¿Qué mejoras importantes introdujo Java 17 respecto a Java 11 en términos de rendimiento y nuevas características? ***

Menciona al menos tres mejoras clave que afecten el rendimiento o la experiencia de desarrollo. 

5️⃣ Mejoras clave en Java 17 vs Java 11

Pattern Matching en switch
sealed class
Record

6. ***En Java, ¿cuál es la diferencia entre HashMap y ConcurrentHashMap? ***

¿En qué situaciones utilizarías cada uno de ellos? Explica cómo manejan la concurrencia. 
6️⃣ HashMap vs ConcurrentHashMap

Características	
HashMap	
ConcurrentHashMap
Seguridad en concurrencia: HashMap	❌ No es seguro - ConcurrentHashMap 	✅ Operaciones seguras
Bloqueo en accesos concurrentes: HashMap	❌ Bloqueo total con synchronized- ConcurrentHashMap	✅ Segmentación con locks internos
Uso recomendado:	HashMap Uso en entornos de un solo hilo - ConcurrentHashMap	Uso en entornos concurrentes

 ```=java
Map<String, Integer> mapa = new ConcurrentHashMap<>();

ExecutorService executor = Executors.newFixedThreadPool(5);
for (int i = 0; i < 100; i++) {
    executor.submit(() -> mapa.put(Thread.currentThread().getName(), 1));
}
executor.shutdown();


```

7. ¿Cómo se implementan y gestionan los Streams en Java 11 y 17? 

Explica el concepto de Streams en Java, destacando sus diferencias de uso en Java 11 y 17. Proporciona un ejemplo donde se utilicen operaciones de filtrado, transformación y agrupación de datos. 

7️⃣ Streams en Java 11 y 17
 Streams permiten operaciones funcionales sobre colecciones.
 En Java 11 y 17, los Collectors han mejorado en rendimiento.

 Ejemplo de Stream con filtrado, transformación y agrupación:

 ```=java
List<String> nombres = List.of("Java", "Spring", "Micronaut", "Quarkus");

Map<Integer, List<String>> agrupados = nombres.stream()
        .filter(n -> n.length() > 5)
        .map(String::toUpperCase)
        .collect(Collectors.groupingBy(String::length));

System.out.println(agrupados); // Agrupa por longitud

```

8. Explica cómo funciona el sistema de módulos en Java 9 y su evolución hasta Java 17. 

¿Cómo se maneja el acceso entre módulos? ¿Cómo afecta esto a la estructura de aplicaciones grandes y monolíticas? 
8️⃣ Módulos en Java 9 y evolución hasta Java 17
Introducidos en Java 9, permiten dividir aplicaciones en módulos encapsulados.

Ejemplo de module-info.java:

 ```=java
module com.example {
    exports com.example.servicio;
    requires java.sql;
}


```
Beneficio clave:

* Evita dependencias cíclicas.
* Mejora el arranque de aplicaciones grandes


9. ¿Qué es un Pattern Matching en Java 17 y cómo mejora la legibilidad del código? 

Describe el concepto y la sintaxis de Pattern Matching en Java 17 y cómo ayuda a reducir el código repetitivo. 

9️⃣ Pattern Matching en Java 17
 Evita instanceof y cast manuales, haciendo el código más limpio.

 Ejemplo sin Pattern Matching:

 ```=java

if (obj instanceof String) {
    String str = (String) obj;
    System.out.println(str.length());
}


```

Ejemplo con Pattern Matching:

 ```=java

if (obj instanceof String str) {
    System.out.println(str.length());
}



```
Beneficio: Menos código repetitivo y más legibilidad.
