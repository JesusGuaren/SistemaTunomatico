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

**👉 Reemplazar esta línea con el diagrama de clases generado:**  
`![Diagrama de Clases](ruta/diagrama-clases.png)`

---

### 📌 Diagrama de Casos de Uso

**👉 Reemplazar esta línea con el diagrama de casos de uso:**  
`![Diagrama de Casos de Uso](ruta/use-case.png)`

---

### 📌 Diagrama de Implementación

**👉 Reemplazar esta línea con el diagrama de implementación:**  
`![Diagrama de Implementación](ruta/implementation-diagram.png)`

---

Fin del documento.
