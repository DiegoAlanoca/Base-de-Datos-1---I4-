# Análisis de Requerimientos y Diseño Conceptual - Base de Datos Taller de Reparación "Sigma Byte"

Este documento contiene el levantamiento de información, la especificación de entidades, diccionario de datos, relaciones justificadas y reglas de integridad para el sistema de control de clientes, equipos, servicios técnicos y garantías del taller "Sigma Byte".

---

## 1. Narración del Cliente, Actores, Eventos y Restricciones

### Descripción del Negocio
"Sigma Byte" es un taller de reciente apertura dedicado a la reparación y mantenimiento de computadoras de escritorio, laptops, consolas y otros equipos electrónicos. La operación técnica abarca desde mantenimientos preventivos (cambio de pasta térmica, aplicación de thermal pads) hasta reparaciones de hardware complejas (reemplazo de displays, soldadura de lectores). 

Actualmente, el inventario y los registros se manejan de forma manual mediante hojas de cálculo. La prioridad operativa absoluta es sistematizar el control de las reparaciones (servicios) y la gestión precisa de los tiempos de garantía otorgados a los clientes. El sistema será de uso interno, priorizando bajos costos operativos (como hosting) y accesibilidad mediante dispositivos móviles.

### Actores
* **Técnicos / Socios:** Realizan la recepción de equipos, ejecutan la división del trabajo técnico, actualizan el estado de los servicios, gestionan el uso de herramientas compartidas y determinan los tiempos de garantía.
* **Clientes:** Propietarios de los equipos electrónicos que solicitan los servicios de reparación.

### Eventos del Negocio
1. **Recepción e Identificación de Equipo:** Registro de los datos de contacto del cliente y catalogación del equipo. Se genera una etiqueta de identificación rápida utilizando el Número de Serie (S/N) del dispositivo.
2. **Generación de Servicio Técnico:** Apertura de una orden de trabajo (servicio) vinculada a un equipo específico, detallando la fecha de ingreso y la falla reportada.
3. **Ejecución y Seguimiento:** Transición del equipo por distintos estados (ej. recibido, en revisión, esperando repuestos, reparado).
4. **Entrega y Activación de Garantía:** Cierre del servicio tras la entrega al cliente, momento exacto en el que se define y registra el periodo de garantía.
5. **Control de Herramientas de Taller:** Asignación y seguimiento del estado de las herramientas e insumos del taller para evitar pérdidas durante la jornada laboral.

### Restricciones Operativas
* **Flexibilidad Documental:** El registro de clientes debe permitir omitir la Cédula de Identidad (C.I.), ya que no todos los usuarios la portan al momento de dejar su equipo.
* **Trazabilidad Única:** Cada equipo se identifica unívocamente por su S/N, el cual no puede repetirse en el sistema.
* **Dependencia Estricta:** Un equipo pertenece a un único dueño de manera perpetua en el sistema. Todo servicio debe estar forzosamente ligado a un equipo existente.

---

## 2. Entidades y Atributos

* **CLIENTE:** Persona que solicita la reparación de uno o varios dispositivos.
  * *Atributos:* Identificador único, nombre completo, teléfono celular, C.I. (opcional).
* **CATEGORIA_EQUIPO:** Clasificación estandarizada para normalizar los registros (computadora de escritorio, laptop, consola, otro equipo electrónico).
  * *Atributos:* Identificador único, nombre de la categoría.
* **EQUIPO:** Dispositivo físico de un cliente que ingresa al taller para ser intervenido.
  * *Atributos:* Identificador único, número de serie (S/N) que funciona como etiqueta física, identificador del cliente dueño, identificador de la categoría.
* **TECNICO:** Personal del taller encargado de ejecutar la reparación (permite dividir y auditar el trabajo).
  * *Atributos:* Identificador único, nombre completo, especialidad, porcentaje_participacion (para distribución de ganancias/equidad).
* **SERVICIO:** Orden de trabajo o historial de intervenciones realizadas sobre un equipo específico. Un equipo recurrente generará múltiples servicios históricos.
  * *Atributos:* Identificador único, fecha de ingreso, descripción detallada del problema, estado actual de la reparación, tiempo de garantía (en días o meses) aplicable tras la entrega.
* **HERRAMIENTA:** Instrumentos e insumos físicos del taller sujetos a seguimiento.
  * *Atributos:* Identificador único, nombre/modelo, estado_actual (disponible, en uso, mantenimiento, extraviada).

---

## 3. Claves Primarias y Tipos Básicos para los Atributos

| Entidad | Atributo | Tipo de Dato | Rol | Descripción |
| :--- | :--- | :--- | :--- | :--- |
| **CLIENTE** | `id_cliente` | `INT` | **PK** | Clave primaria autonumérica. |
| | `nombre_completo`| `VARCHAR(100)`| Atributo | Nombre y apellido del cliente. |
| | `celular` | `VARCHAR(20)` | Atributo | Número de contacto para notificaciones. |
| | `ci` | `VARCHAR(20)` | Atributo | Cédula de identidad (Acepta nulos). |
| **CATEGORIA** | `id_categoria` | `INT` | **PK** | Clave primaria autonumérica. |
| | `nombre` | `VARCHAR(50)` | Atributo | 'Laptop', 'Escritorio', 'Consola', 'Otro'. |
| **EQUIPO** | `id_equipo` | `INT` | **PK** | Clave primaria autonumérica. |
| | `sn_etiqueta` | `VARCHAR(100)`| Atributo | Número de serie, usado para imprimir etiqueta. |
| | `id_cliente` | `INT` | **FK** | Relación con el dueño del equipo. |
| | `id_categoria` | `INT` | **FK** | Clasificación del hardware. |
| **TECNICO** | `id_tecnico` | `INT` | **PK** | Clave primaria autonumérica. |
| | `nombre` | `VARCHAR(100)`| Atributo | Nombre del socio/técnico. |
| **SERVICIO** | `id_servicio` | `INT` | **PK** | Clave primaria autonumérica. |
| | `fecha_ingreso` | `DATETIME` | Atributo | Fecha y hora en la que se recibe el equipo. |
| | `descripcion` | `TEXT` | Atributo | Falla reportada y detalles técnicos del ingreso. |
| | `estado` | `VARCHAR(30)` | Atributo | 'Recibido', 'En Diagnóstico', 'Reparado', 'Entregado'. |
| | `garantia_dias` | `INT` | Atributo | Días de vigencia de la garantía pos-entrega. |
| | `id_equipo` | `INT` | **FK** | Referencia al equipo intervenido. |
| | `id_tecnico` | `INT` | **FK** | Responsable de ejecutar la labor técnica. |
| **HERRAMIENTA**| `id_herramienta`| `INT` | **PK** | Clave primaria autonumérica. |
| | `nombre` | `VARCHAR(100)`| Atributo | Ej: Estación de calor, programador de EEPROM. |
| | `estado_uso` | `VARCHAR(30)` | Atributo | 'Disponible', 'Asignada'. |

---

## 4. Relaciones entre Entidades y Cardinalidades

* **CLIENTE (1) a EQUIPO (N) — [1:N]**
  * *Cardinalidad:* `1:N` (Un cliente puede traer uno o varios equipos al taller; un equipo pertenece estricta y permanentemente a un solo cliente).
  * *Justificación:* Permite agrupar todos los dispositivos de un usuario bajo un solo perfil de contacto.
* **CATEGORIA (1) a EQUIPO (N) — [1:N]**
  * *Cardinalidad:* `1:N` (Una categoría agrupa a múltiples equipos; un equipo tiene una sola categoría).
  * *Justificación:* Eleva la "categoría" de un simple texto a una entidad, estandarizando la base de datos para futuros reportes (ej. saber si entran más laptops que consolas).
* **EQUIPO (1) a SERVICIO (N) — [1:N]**
  * *Cardinalidad:* `1:N` (Un equipo puede generar múltiples servicios en el tiempo; un servicio se aplica a un solo equipo).
  * *Justificación:* Si un cliente trae una laptop hoy por cambio de pasta térmica, y en 6 meses vuelve por un display roto, el equipo es el mismo pero se generan dos servicios históricos diferentes para auditar garantías de forma aislada.
* **TECNICO (1) a SERVICIO (N) — [1:N]**
  * *Cardinalidad:* `1:N` (Un técnico puede tener asignados varios servicios; un servicio principal es ejecutado por un técnico responsable).
  * *Justificación:* Fundamental para la división del trabajo y clarificar responsabilidades sobre garantías de mano de obra.

---

## 5. Restricciones e Integridad de Datos

### Restricciones de Dominio y Chequeo (`CHECK`)
* **Garantías Lógicas:** `garantia_dias >= 0`. El tiempo de garantía no puede ser negativo. Se asigna formalmente en el cambio de estado a 'Entregado'.
* **Flujo de Estados Permitidos:** `estado IN ('Recibido', 'En Diagnóstico', 'Esperando Repuesto', 'Reparado', 'Entregado')`.

### Restricciones de Unicidad (`UNIQUE`) y Obligatoriedad (`NOT NULL`)
* **`NOT NULL`:** Obligatorio en `nombre_completo`, `celular`, `sn_etiqueta`, `descripcion`, y todas las llaves foráneas. Garantiza que no existan registros "huérfanos".
* **Admitir Nulos:** El campo `ci` en `CLIENTE` debe aceptar explícitamente valores nulos.
* **`UNIQUE`:** `EQUIPO.sn_etiqueta`. Por reglas de negocio, el número de serie es un identificador físico irrepetible.

### Ciclos de Vida y Dependencias (Reglas de Eliminación)
* **EQUIPO en relación a CLIENTE:** Si se intenta eliminar un cliente, la base de datos debe rechazar la acción (`ON DELETE RESTRICT`) si este tiene equipos registrados, preservando el historial.
* **SERVICIO en relación a EQUIPO:** No se permite eliminar un equipo si este tiene servicios técnicos históricos asociados (`ON DELETE RESTRICT`), garantizando que los registros de garantías y reparaciones pasadas sean inmutables.
* **Creación Estricta:** Las llaves foráneas exigen que no se pueda registrar un servicio sin apuntar a un ID de equipo existente, ni un equipo sin apuntar a un ID de cliente.
