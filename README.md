# 🏥 Sistema Tunomático - Gestión de Turnos Médicos  
**Repositorio académico** que modela un sistema de turnos digitales con:  
- **Patrones de diseño en Java** (Singleton, Adapter, Factory Method).  
- **Diagramas UML** generados con PlantUML.  
- **Transición completa** desde casos de uso hasta implementación física.  

---

## 📋 Descripción Técnica  
Sistema para clínicas médicas que gestiona:  
- **Asignación priorizada** de turnos (urgentes/rutinarios).  
- **Notificaciones** vía SMS/Email integradas con ERP hospitalario.  
- **Persistencia** en PostgreSQL usando JDBC.  

**Tecnologías Java**:  
- **Backend**: Spring Boot (Singleton, Adapter).  
- **Frontend**: JavaFX (para demostración).  
- **BD**: PostgreSQL + HikariCP (connection pooling).

---

## Índice
1. Introducción  
2. Patrones de Diseño Implementados  
    - Singleton  
    - Prototype  
    - Adapter  
    - Bridge  
3. Conclusión  
4. Diagramas

---

## 1. Introducción

El presente documento detalla la implementación de diversos patrones de diseño aplicados a un sistema de turnos médicos. Este sistema busca optimizar el proceso de asignación, clonación y notificación de turnos para pacientes, permitiendo una estructura escalable, mantenible y flexible.

---

## 2. Patrones de Diseño Implementados

### 2.1 Singleton

#### Propósito:
Asegurar que una clase tenga solo una instancia y proporcionar un punto de acceso global a ella.

#### Implementación:
Se utiliza el patrón Singleton en la clase `GestorTurnos`, encargada de administrar la lógica de negocio para la asignación de turnos.

#### Código:
```java
public class GestorTurnos {
    private static GestorTurnos instancia;

    private GestorTurnos() {}

    public static GestorTurnos getInstancia() {
        if (instancia == null) {
            instancia = new GestorTurnos();
        }
        return instancia;
    }

    public void asignarTurno(Paciente paciente, Medico medico, Date fecha) {
        // Lógica de asignación
    }
}
```

#### Beneficios:
- Evita múltiples instancias que puedan generar conflictos.
- Centraliza el control de turnos en una sola clase.

---

### 2.2 Prototype

#### Propósito:
Permitir la clonación de objetos complejos de forma eficiente.

#### Implementación:
Se implementa en la clase `Turno`, permitiendo clonar un turno base para asignarlo a distintos pacientes.

#### Código:
```java
public class Turno implements Cloneable {
    private Date fecha;
    private Medico medico;
    private Paciente paciente;

    public Turno(Date fecha, Medico medico, Paciente paciente) {
        this.fecha = fecha;
        this.medico = medico;
        this.paciente = paciente;
    }

    public Turno clonar() {
        try {
            return (Turno) this.clone();
        } catch (CloneNotSupportedException e) {
            return null;
        }
    }
}
```

#### Beneficios:
- Rápida duplicación de turnos existentes.
- Facilita la programación de turnos recurrentes.

---

### 2.3 Adapter

#### Propósito:
Permitir que interfaces incompatibles trabajen juntas.

#### Implementación:
Se utiliza para integrar múltiples canales de notificación (Email, SMS) a través de una interfaz común `INotificador`.

#### Código:
```java
public interface INotificador {
    void enviarNotificacion(String mensaje, String destinatario);
}
```

```java
public class EmailAdapter implements INotificador {
    private SistemaEmail sistemaEmail = new SistemaEmail();

    public void enviarNotificacion(String mensaje, String destinatario) {
        sistemaEmail.enviarCorreo(destinatario, mensaje);
    }
}
```

```java
public class SMSAdapter implements INotificador {
    private SistemaSMS sistemaSMS = new SistemaSMS();

    public void enviarNotificacion(String mensaje, String destinatario) {
        sistemaSMS.enviarMensaje(destinatario, mensaje);
    }
}
```

#### Beneficios:
- Abstracción de los métodos propios de cada sistema.
- Agregar nuevos canales sin modificar el código cliente.

---

### 2.4 Bridge

#### Propósito:
Separar una abstracción de su implementación para que ambas puedan evolucionar independientemente.

#### Implementación:
Se aplica en la abstracción `Notificador`, que puede trabajar con distintas implementaciones (`EmailNotificador`, `SMSNotificador`) sin depender directamente de ellas.

#### Código:
```java
public abstract class Notificador {
    protected INotificador notificador;

    public Notificador(INotificador notificador) {
        this.notificador = notificador;
    }

    public abstract void notificarTurno(Paciente paciente, Turno turno);
}
```

```java
public class NotificadorTurno extends Notificador {
    public NotificadorTurno(INotificador notificador) {
        super(notificador);
    }

    public void notificarTurno(Paciente paciente, Turno turno) {
        String mensaje = "Turno confirmado para: " + turno.getFecha();
        notificador.enviarNotificacion(mensaje, paciente.getContacto());
    }
}
```

#### Beneficios:
- Escalabilidad: se pueden agregar nuevas implementaciones o tipos de notificador fácilmente.
- Mayor reutilización de código.

---

## 3. Conclusión

El uso de patrones de diseño en este sistema permite:

- Mejorar la modularidad.
- Facilitar la mantenibilidad.
- Potenciar la escalabilidad.
- Simplificar la integración con tecnologías externas (como canales de notificación).

Cada patrón se seleccionó para resolver un problema de diseño concreto dentro del dominio de turnos médicos, y su implementación permite que el sistema esté preparado para crecer en funcionalidad sin comprometer su estructura base.

---

## 4. Diagramas

### 📌 Diagrama de Clases (UML)
## 🧠 Justificación Profunda de Patrones – Diagrama de Clases

El diseño de clases se apoya en patrones que resuelven necesidades específicas del dominio:

### 🔁 Singleton (`GestorTurnos`)
- Se emplea para centralizar el control de asignaciones y evitar instancias múltiples que puedan provocar **inconsistencias** en la lógica de turnos. Su implementación garantiza una **única instancia compartida** en todo el sistema.

### 🧬 Prototype (`Turno`)
- Permite clonar turnos base para consultas recurrentes, evitando sobrecarga en la creación de objetos y asegurando una **copia exacta del estado** sin violar el encapsulamiento.

### 🔌 Adapter (`EmailAdapter`, `SMSAdapter`)
- Resuelve el problema de **incompatibilidad de interfaces** entre el sistema central y los canales externos de comunicación. Facilita el uso uniforme a través de la interfaz `INotificador`, promoviendo **desacoplamiento** y **facilidad de mantenimiento**.

### 🌉 Bridge (`Notificador`)
- Se separa la abstracción (`NotificadorTurno`) de sus implementaciones concretas (`EmailAdapter`, `SMSAdapter`) permitiendo agregar **nuevos canales** sin alterar la lógica de negocio. Esto aporta **flexibilidad y extensibilidad** al modelo.

Cada patrón responde a principios SOLID como:
- **S**: Principio de Responsabilidad Única (clases especializadas como `GestorTurnos`, `Notificador`)
- **O**: Abierto/Cerrado (nuevos tipos de notificadores no requieren modificar código existente)
- **D**: Inversión de Dependencias (uso de interfaces para desacoplar cliente e implementación)

**👉 Reemplazar esta línea con el diagrama de clases generado:**  
`![Diagrama de Clases](ruta/diagrama-clases.png)`

---

### 📌 Diagrama de Casos de Uso
## 🧩 Justificación de Relaciones – Casos de Uso

El sistema define relaciones entre **actores** (Pacientes, Médicos, Sistema ERP) y **casos de uso** que reflejan las interacciones clave para la gestión de turnos. Las relaciones se justifican en función de los siguientes principios:

- **Inclusión (`<<include>>`)**: Se aplica en casos como `Confirmar Turno` que incluye `Enviar Notificación`, ya que toda confirmación implica una notificación automática. Esto promueve la **reutilización** de funcionalidades comunes.

- **Extensión (`<<extend>>`)**: Se utiliza, por ejemplo, cuando `Asignar Turno` puede extenderse opcionalmente a `Clonar Turno` si se trata de una consulta periódica. Esta relación refleja **variabilidad condicional**, donde una función adicional no siempre se ejecuta.

- **Generalización**: Los actores como `Administrador` y `Paciente` heredan de un actor genérico `Usuario`, reflejando una jerarquía natural en los roles del sistema, donde ciertos casos de uso (como "Ver Historial de Turnos") están disponibles para múltiples tipos de usuarios.

- **Relación directa (asociación)**: Los actores se asocian con los casos de uso principales que pueden ejecutar. Por ejemplo, `Paciente` se asocia con `Solicitar Turno` y `Ver Turno`, mientras que `Médico` se asocia con `Visualizar Agenda`.

Estas relaciones permiten una visión clara, **modular y escalable**, facilitando futuras ampliaciones sin comprometer la estructura actual.


**👉 Reemplazar esta línea con el diagrama de casos de uso:**  
`![Diagrama de Casos de Uso](ruta/use-case.png)`

---

### 📌 Diagrama de Implementación
## ⚙️ Decisiones Técnicas – Diagrama de Implementación

El sistema se desplegó considerando aspectos clave de **desempeño, escalabilidad e integración**:

### 🏗️ Backend: Spring Boot + JDBC (con HikariCP)
- Se optó por **Spring Boot** por su facilidad de configuración y soporte robusto para patrones empresariales.
- **HikariCP** como pool de conexiones mejora la **performance** en operaciones de base de datos, garantizando baja latencia y alta disponibilidad.

### 🖥️ Frontend: JavaFX
- Usado para la **demo interactiva**, permite construir una interfaz rica y nativa en Java. Facilita pruebas sin necesidad de despliegue web, ideal para entornos académicos y demostraciones locales.

### 🧩 ERP Hospitalario (Sistema externo)
- Se conecta vía API (simulada en el modelo), reflejando una arquitectura que considera **interoperabilidad** con sistemas existentes en hospitales.
- Los adaptadores garantizan una transición suave al integrar notificaciones con sistemas externos.

### 🗄️ Base de Datos: PostgreSQL
- Elegida por su solidez, escalabilidad y soporte a consultas complejas.
- El esquema incluye tablas como `pacientes`, `medicos`, `turnos`, modeladas con claves foráneas y restricciones para mantener **integridad relacional**.

### 🧪 Testing & Mantenimiento
- Las clases están diseñadas para facilitar pruebas unitarias (clases como `GestorTurnos` pueden ser instanciadas controladamente).
- El uso de patrones reduce el acoplamiento, lo que mejora la **capacidad de mantenimiento** del sistema a largo plazo.

**👉 Reemplazar esta línea con el diagrama de implementación:**  
`![Diagrama de Implementación](ruta/implementation-diagram.png)`

---

Fin del documento.
