# 🏥 Caso Real: Sistema Tunomático de Gestión de Turnos Médicos

Este repositorio presenta un sistema corporativo para la gestión digitalizada de turnos en centros médicos. El sistema **Tunomático** incorpora algoritmos de priorización, integración con ERP hospitalarios y cumplimiento de normativas internacionales como **HIPAA** (EE.UU.) y **MINSAL** (Chile).

---

## 📋 Descripción General

El sistema **Tunomático** automatiza y optimiza la asignación de turnos médicos con características clave como:

- ✅ Priorización inteligente de turnos: atención urgente vs. rutinaria.
- ✅ Integración con ERP hospitalario para envío de notificaciones multicanal (SMS, Email, WhatsApp).
- ✅ Auditoría de accesos conforme a HIPAA.
- ✅ Dashboard en tiempo real.
- ✅ Cumplimiento de normativas de protección de datos en EE.UU. y Chile.

---

## 🧩 Diagrama de Casos de Uso

![Casos de Uso](imagenes/casos_uso.png)

### 🎭 Actores y Roles

| Actor             | Rol en el Sistema                                       |
|------------------|----------------------------------------------------------|
| **Paciente**      | Solicita, cancela o paga turnos médicos.               |
| **Recepcionista** | Administra agendas, sobrecupos y confirmaciones.       |
| **ERP Hospitalario** | Proporciona datos clínicos y servicios de notificación. |

### 🔗 Relaciones Clave

- `<<include>>`: Todos los flujos incluyen validación de identidad conforme a HIPAA.
- `<<extend>>`: El pago en línea se extiende solo en clínicas privadas.

---

## 📦 Diagrama de Clases

![Diagrama de Clases](imagenes/diagrama_clases.png)

Este diseño aplica múltiples patrones de diseño para asegurar escalabilidad y desacoplamiento.

### 🛠️ Patrones de Creación

| Patrón        | Clase         | Justificación                                                                 |
|---------------|---------------|--------------------------------------------------------------------------------|
| **Singleton** | `GestorTurnos` | Centraliza la gestión de turnos. Usa `synchronized` para evitar condiciones de carrera. |
| **Prototype** | `Turno`       | Clonación de turnos recurrentes como controles mensuales.                      |

### 🏛️ Patrones Estructurales

| Patrón       | Clase              | Beneficio                                                                       |
|--------------|--------------------|----------------------------------------------------------------------------------|
| **Adapter**  | `NotificadorAdapter` | Integra múltiples APIs externas (SMS, Email, WhatsApp) bajo una sola interfaz.  |
| **Facade**   | `SistemaFacade`     | Encapsula subsistemas complejos y simplifica la interacción con ERP y BD.       |

### ⚙️ Patrones de Comportamiento

| Patrón       | Clase              | Aplicación Real                                                                  |
|--------------|--------------------|----------------------------------------------------------------------------------|
| **Observer** | `SistemaAlertas`   | Notificaciones en tiempo real ante cancelaciones o reprogramaciones.             |
| **Command**  | `ComandoAuditoria` | Encapsula acciones sensibles para auditoría y rollback legal.                    |

---

## 🚀 Diagrama de Implementación

![Diagrama de Implementación](imagenes/diagrama_implementacion.png)

### Arquitectura Distribuida

| Nodo                | Tecnología             | Patrón Aplicado         | Justificación Técnica                                                                 |
|---------------------|------------------------|--------------------------|----------------------------------------------------------------------------------------|
| **Cliente (Android)** | Java + Retrofit        | `Bridge`                 | Separa vista de lógica con MVP.                                                       |
| **Servidor (AWS)**   | Spring Boot + Redis     | `Singleton` distribuido  | Redis mantiene sincronización entre múltiples instancias del backend.                 |
| **ERP Hospitalario** | REST/SOAP               | `Adapter + CircuitBreaker` | Adaptador resiliente con reintentos y tolerancia a fallos en servicios externos.       |

---

## 💡 Reflexiones Finales del Modelado

### ✅ Lecciones Clave

- Uso de **Singleton distribuido** sincronizado con Redis para coherencia de turnos en instancias escaladas.
- Implementación de **Adapter resiliente** con reintentos y tolerancia a fallos.

### ⚠️ Deuda Técnica Detectada

- **Problema**: La validación de prioridad se realizaba en el cliente.
- **Solución**: Migración al backend mediante GraphQL para centralizar la lógica.

---

## 🛤️ Roadmap Tecnológico

- Separación en **microservicios**:
  - `TurnosCore` (con Redis).
  - `NotificacionesService` (event-driven con Kafka).
- Implementación de **Saga Pattern** para transacciones distribuidas.

---

## 📁 Estructura del Repositorio

