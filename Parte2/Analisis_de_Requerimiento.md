# Análisis de Requerimientos y Diseño Conceptual - Base de Datos Taller de Reparación "Sigma Byte"

Este documento contiene el levantamiento de información, la especificación de entidades, el diccionario de datos corregido con todas sus claves primarias y foráneas, relaciones justificadas y reglas de integridad para el sistema de control de clientes, equipos, servicios técnicos, inventario de repuestos y garantías del taller "Sigma Byte".

---

## 1. Narración del Cliente, Actores, Eventos y Restricciones

### Descripción del Negocio
"Sigma Byte" es un taller dedicado a la reparación y mantenimiento preventivo y correctivo de computadoras de escritorio, laptops, consolas y otros equipos electrónicos. La operación técnica abarca desde mantenimientos (cambio de pasta térmica) hasta reparaciones complejas de hardware. 

Actualmente, los registros se manejan de forma manual mediante hojas de cálculo. La prioridad operativa es sistematizar el control de las reparaciones (servicios), gestionar los insumos y repuestos aplicados mediante el inventario, y llevar un control preciso de los tiempos de garantía otorgados a los clientes.

### Actores
* **Técnicos / Socios:** Realizan la recepción de equipos, ejecutan los servicios, actualizan estados, descuentan insumos del inventario, controlan herramientas y definen las garantías.
* **Clientes:** Propietarios de los dispositivos electrónicos que solicitan el soporte técnico.

### Eventos del Negocio
1. **Recepción e Identificación de Equipo:** Registro de los datos de contacto del cliente y catalogación del equipo mediante el Número de Serie (S/N) impreso en una etiqueta.
2. **Generación de Servicio Técnico:** Apertura de una orden de trabajo vinculada a un equipo específico, detallando la fecha de ingreso y la falla reportada.
3. **Asignación de Insumos y Repuestos:** Registro opcional de piezas o consumibles del inventario utilizados en el servicio para descontarlos automáticamente del stock.
4. **Ejecución y Seguimiento:** Transición del equipo por distintos estados operativos dentro del taller.
5. **Entrega y Activación de Garantía:** Cierre del servicio tras la entrega al cliente, registrando formalmente los días de garantía aplicables.
6. **Control de Herramientas de Taller:** Asignación y seguimiento del estado de los instrumentos de trabajo.

### Restricciones Operativas
* **Flexibilidad Documental:** El registro de clientes permite omitir la Cédula de Identidad (C.I.) si no la portan.
* **Trazabilidad Única:** Cada equipo se identifica unívocamente por su S/N, el cual no puede repetirse.
* **Dependencia Estricta:** Un equipo pertenece a un único dueño y todo servicio debe estar forzosamente ligado a un equipo existente.
* **Uso Opcional de Inventario:** No todos los servicios requieren piezas de repuesto (por ejemplo, diagnósticos o limpiezas básicas), por lo que el consumo de inventario es variable.

---

## 2. Entidades y Atributos

* **CLIENTE:** Persona que solicita la reparación de uno o varios dispositivos.
* **CATEGORIA_EQUIPO:** Clasificación estandarizada para normalizar los equipos.
* **EQUIPO:** Dispositivo físico de un cliente que ingresa al taller para ser intervenido.
* **TECNICO:** Personal encargado de ejecutar la reparación.
* **SERVICIO:** Orden de trabajo histórica realizada sobre un equipo específico.
* **PRODUCTO:** Catálogo de inventario que abarca repuestos y consumibles.
* **USO_PRODUCTO:** Entidad asociativa que registra los insumos y la cantidad exacta aplicada a un servicio.
* **HERRAMIENTA:** Instrumentos físicos del taller sujetos a seguimiento.

---

## 3. Diccionario de Datos

| Entidad | Atributo | Tipo de Dato | Rol | Descripción |
| :--- | :--- | :--- | :--- | :--- |
| **CLIENTE** | `id_cliente` | `INT` | **PK** | Clave primaria autonumérica. |
| | `nombre_completo` | `VARCHAR(100)` | Atributo | Nombre y apellido del cliente. |
| | `celular` | `VARCHAR(20)` | Atributo | Número de contacto para notificaciones. |
| | `ci` | `VARCHAR(20)` | Atributo | Cédula de identidad (Acepta nulos). |
| **CATEGORIA_EQUIPO** | `id_categoria` | `INT` | **PK** | Clave primaria autonumérica. |
| | `nombre` | `VARCHAR(50)` | Atributo | 'Laptop', 'Escritorio', 'Consola', 'Otro'. |
| **EQUIPO** | `id_equipo` | `INT` | **PK** | Clave primaria autonumérica. |
| | `sn_etiqueta` | `VARCHAR(100)` | Atributo | Número de serie físico, usado como etiqueta. |
| | `id_cliente` | `INT` | **FK** | Relación con el cliente dueño del equipo. |
| | `id_categoria` | `INT` | **FK** | Relación con la categoría del hardware. |
| **TECNICO** | `id_tecnico` | `INT` | **PK** | Clave primaria autonumérica. |
| | `nombre` | `VARCHAR(100)` | Atributo | Nombre del socio o técnico responsable. |
| **SERVICIO** | `id_servicio` | `INT` | **PK** | Clave primaria autonumérica. |
| | `fecha_ingreso` | `DATETIME` | Atributo | Fecha y hora exacta de recepción del equipo. |
| | `descripcion` | `TEXT` | Atributo | Falla reportada y detalles técnicos. |
| | `estado` | `VARCHAR(30)` | Atributo | Estado actual ('Recibido', 'Reparado', etc.). |
| | `garantia_dias` | `INT` | Atributo | Días de vigencia de la garantía post-entrega. |
| | `id_equipo` | `INT` | **FK** | Referencia al equipo intervenido. |
| | `id_tecnico` | `INT` | **FK** | Referencia al técnico responsable. |
| **PRODUCTO** | `id_producto` | `INT` | **PK** | Clave primaria autonumérica del inventario. |
| | `nombre` | `VARCHAR(100)` | Atributo | Nombre comercial del repuesto o consumible. |
| | `tipo` | `VARCHAR(20)` | Atributo | Clasificación ('Pieza' o 'Consumible'). |
| | `stock` | `DECIMAL(8,2)` | Atributo | Cantidad disponible en almacén. |
| | `precio_compra` | `DECIMAL(10,2)` | Atributo | Costo de adquisición del insumo. |
| | `precio_venta` | `DECIMAL(10,2)` | Atributo | Valor cobrado al cliente por la pieza. |
| **USO_PRODUCTO** | `id_uso` | `INT` | **PK** | Clave primaria autonumérica del detalle. |
| | `cantidad_usada` | `DECIMAL(8,2)` | Atributo | Unidades o porciones gastadas en el servicio. |
| | `id_servicio` | `INT` | **FK** | Referencia a la orden de trabajo asociada. |
| | `id_producto` | `INT` | **FK** | Referencia al producto utilizado. |
| **HERRAMIENTA** | `id_herramienta` | `INT` | **PK** | Clave primaria autonumérica. |
| | `nombre` | `VARCHAR(100)` | Atributo | Nombre de la herramienta del taller. |
| | `estado_uso` | `VARCHAR(30)` | Atributo | Estado operativo ('Disponible', 'Asignada'). |

---

## 4. Relaciones y Cardinalidades

* **CLIENTE (1) a EQUIPO (N):** Un cliente puede registrar uno o varios equipos, pero cada equipo pertenece de forma estricta a un solo dueño.
* **CATEGORIA_EQUIPO (1) a EQUIPO (N):** Una categoría agrupa a múltiples equipos; un equipo cuenta con una sola categoría estandarizada.
* **EQUIPO (1) a SERVICIO (N):** Un equipo acumula múltiples órdenes de servicio en el tiempo; cada servicio atiende a un único equipo.
* **TECNICO (1) a SERVICIO (N):** Un técnico gestiona diversas reparaciones; cada orden de trabajo principal tiene asignado un responsable.
* **SERVICIO (1) a USO_PRODUCTO (N) y PRODUCTO (1) a USO_PRODUCTO (N):** Relación de muchos a muchos resuelta mediante la entidad asociativa `USO_PRODUCTO`. Permite que un servicio consuma múltiples repuestos, y que un producto sea aplicado en diferentes órdenes a lo largo del tiempo.

---

## 5. Reglas de Integridad y Restricciones

### Restricciones de Dominio y Chequeo (`CHECK`)
* **Integridad Financiera e Inventario:** `stock >= 0`, `precio_compra >= 0`, `precio_venta >= 0`. El sistema impide existencias negativas.
* **Consumo Válido:** `cantidad_usada > 0` en la tabla `USO_PRODUCTO`.
* **Garantías Lógicas:** `garantia_dias >= 0`. No se permiten periodos negativos.
* **Tipos de Inventario Válidos:** `tipo IN ('Pieza', 'Consumible')`.
* **Flujo de Estados:** `estado IN ('Recibido', 'En Diagnóstico', 'Esperando Repuesto', 'Reparado', 'Entregado')`.

### Restricciones de Unicidad y Obligatoriedad
* **`NOT NULL`:** Obligatorio en nombres, descripciones, llaves foráneas y campos críticos para evitar registros huérfanos.
* **`UNIQUE`:** `EQUIPO.sn_etiqueta`. El número de serie físico es un identificador irrepetible.

### Políticas de Eliminación
* **Protección Histórica (`ON DELETE RESTRICT`):** No se permite eliminar un cliente, equipo o producto si estos poseen registros históricos de servicios o consumos financieros asociados.
* **Liberación de Stock en Cascada (`ON DELETE CASCADE`):** Si un servicio se elimina antes de procesarse, sus consumos asociados en `USO_PRODUCTO` se eliminan en cascada para reincorporar los insumos al inventario.
