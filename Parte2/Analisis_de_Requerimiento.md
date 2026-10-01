# Análisis de Requerimientos y Diseño Conceptual — Sistema de Control de Garantías e Inventario para Taller de Reparación Electrónica "Sigma Byte"

## 1. Resumen ejecutivo

Este documento especifica los requerimientos y el modelo conceptual de un sistema para digitalizar la operación del taller de reparación electrónica Sigma Byte: registro de clientes y sus equipos, control de garantías por cada reparación, inventario de herramientas del taller e inventario de productos de venta (piezas y consumibles).

---

## 2. Objetivo del sistema

Diseñar e implementar una base de datos que permita:

1. Registrar clientes y los equipos que traen al taller, identificando cada equipo de forma única.
2. Registrar cada reparación (servicio) con su fecha, descripción del problema, estado, técnico responsable y periodo de garantía.
3. Consultar si un equipo específico sigue en garantía a la fecha actual.
4. Controlar el inventario de herramientas del taller (modelo, marca, fotografía de referencia).
5. Controlar el inventario de productos de venta (piezas y consumibles) y su uso en las reparaciones.
6. Registrar el cobro de cada servicio.

### Alcance

**Incluye:** registro de clientes y equipos, órdenes de servicio con técnico responsable y garantía, inventario de herramientas, inventario de productos de venta, uso de productos en reparaciones, cobro de servicios.

**No incluye:** página web pública, manejo de múltiples sucursales, programación de citas.

---

## 3. Entidades y atributos

* **CLIENTE:** persona que trae uno o varios equipos a reparar.
  *Atributos:* id, nombre completo, C.I., celular.

* **EQUIPO:** cada aparato que entra al taller, identificado por su número de serie.
  *Atributos:* S/N (único, funciona también como etiqueta impresa), categoría (computadora de escritorio / laptop / consola / otro).

* **SERVICIO:** cada orden de reparación abierta sobre un equipo.
  *Atributos:* id, fecha de ingreso, descripción del problema, estado, fecha de inicio de garantía, fecha de fin de garantía.

* **TECNICO:** persona que realiza las reparaciones.
  *Atributos:* id, nombre, rol.

* **HERRAMIENTA:** herramientas propias del taller, usadas para reparar.
  *Atributos:* código de modelo (único), marca, fotografía.

* **PRODUCTO_VENTA:** piezas y consumibles que se venden o se usan en las reparaciones.
  *Atributos:* id, nombre, tipo (Pieza / Consumible), precio de venta, stock actual.

* **PAGO_SERVICIO:** cobro de una reparación.
  *Atributos:* id, monto, método de pago, fecha de pago.

---

## 4. Diccionario de datos

| Entidad | Atributo | Tipo de dato | Rol | Descripción |
| :--- | :--- | :--- | :--- | :--- |
| **CLIENTE** | `id_cliente` | `INT` | PK | Autonumérico. |
| | `nombre_completo` | `VARCHAR(150)` | Atributo | — |
| | `ci` | `VARCHAR(20)` | Atributo | Puede ser `NULL`. |
| | `celular` | `VARCHAR(20)` | Atributo | — |
| **EQUIPO** | `sn` | `VARCHAR(50)` | PK | Número de serie, también sirve de etiqueta impresa. |
| | `id_cliente` | `INT` | FK → CLIENTE | Dueño del equipo. |
| | `categoria` | `VARCHAR(30)` | Atributo | `'computadora de escritorio' / 'laptop' / 'consola' / 'otro'`. |
| **SERVICIO** | `id_servicio` | `INT` | PK | Autonumérico. |
| | `sn_equipo` | `VARCHAR(50)` | FK → EQUIPO | — |
| | `id_tecnico` | `INT` | FK → TECNICO | — |
| | `fecha_ingreso` | `DATE` | Atributo | — |
| | `descripcion_problema` | `VARCHAR(255)` | Atributo | — |
| | `estado` | `VARCHAR(20)` | Atributo | `'en proceso' / 'terminado' / 'entregado'`. |
| | `fecha_inicio_garantia` | `DATE` | Atributo | — |
| | `fecha_fin_garantia` | `DATE` | Atributo | — |
| **TECNICO** | `id_tecnico` | `INT` | PK | Autonumérico. |
| | `nombre` | `VARCHAR(100)` | Atributo | — |
| | `rol` | `VARCHAR(30)` | Atributo | — |
| **HERRAMIENTA** | `codigo_modelo` | `VARCHAR(50)` | PK | — |
| | `marca` | `VARCHAR(50)` | Atributo | — |
| | `fotografia` | `VARCHAR(255)` | Atributo | Enlace o referencia a la foto. |
| **PRODUCTO_VENTA** | `id_producto` | `INT` | PK | Autonumérico. |
| | `nombre` | `VARCHAR(100)` | Atributo | — |
| | `tipo` | `VARCHAR(20)` | Atributo | `'Pieza' / 'Consumible'`. |
| | `precio_venta` | `DECIMAL(10,2)` | Atributo | ≥ 0. |
| | `stock_actual` | `INT` | Atributo | ≥ 0. |
| **USA** *(relación N:M)* | `id_servicio` | `INT` | FK → SERVICIO | — |
| | `id_producto` | `INT` | FK → PRODUCTO_VENTA | — |
| | `cantidad_usada` | `INT` | Atributo | > 0. |
| **PAGO_SERVICIO** | `id_pago` | `INT` | PK | Autonumérico. |
| | `id_servicio` | `INT` | FK → SERVICIO | — |
| | `monto` | `DECIMAL(10,2)` | Atributo | > 0. |
| | `metodo_pago` | `VARCHAR(20)` | Atributo | `'Efectivo' / 'QR' / 'Transferencia'`. |
| | `fecha_pago` | `DATE` | Atributo | — |

---

## 5. Relaciones y cardinalidades

* `CLIENTE (1) — EQUIPO (N)`: un cliente tiene uno o varios equipos; cada equipo es de un único cliente.
* `EQUIPO (1) — SERVICIO (N)`: un equipo puede tener uno o varios servicios; cada servicio pertenece a un único equipo.
* `TECNICO (1) — SERVICIO (N)`: un técnico realiza varios servicios; cada servicio lo hace un solo técnico.
* `SERVICIO (0,N) — USA — (0,N) PRODUCTO_VENTA`: un servicio puede usar varios productos; un producto puede usarse en varios servicios. Relación N:M con atributo propio `cantidad_usada`.
* `SERVICIO (1) — PAGO_SERVICIO (N)`: un servicio puede cobrarse en uno o varios pagos; cada pago corresponde a un solo servicio.
* `HERRAMIENTA`: entidad independiente, sin relaciones con las demás.

---

## 6. Restricciones e integridad de datos

### 6.1 Restricciones de dominio (`CHECK`)

- `categoria IN ('computadora de escritorio','laptop','consola','otro')`.
- `estado IN ('en proceso','terminado','entregado')`.
- `tipo IN ('Pieza','Consumible')`.
- `metodo_pago IN ('Efectivo','QR','Transferencia')`.
- `stock_actual >= 0`, `precio_venta >= 0`, `cantidad_usada > 0`, `monto > 0`.
- `fecha_fin_garantia >= fecha_inicio_garantia`.

### 6.2 Unicidad y obligatoriedad

- `UNIQUE` en `EQUIPO.sn` y `HERRAMIENTA.codigo_modelo`.
- `NOT NULL` en `celular`, `sn`, `fecha_ingreso`, `estado`, y en todas las llaves foráneas.
- `ci` es el único atributo que permite valor vacío.

### 6.3 Ciclo de vida: composición vs. agregación

**Composición (dependen de su padre, se eliminan en cascada):**
- `USA` respecto a `SERVICIO` (`ON DELETE CASCADE`).
- `PAGO_SERVICIO` respecto a `SERVICIO` (`ON DELETE CASCADE`).

**Agregación (independientes, se restringe el borrado si hay historial):**
- `CLIENTE` en `EQUIPO` (`ON DELETE RESTRICT`).
- `EQUIPO` en `SERVICIO` (`ON DELETE RESTRICT`).
- `TECNICO` en `SERVICIO` (`ON DELETE RESTRICT`).
- `PRODUCTO_VENTA` en `USA` (`ON DELETE RESTRICT`).

### 6.4 Reglas de negocio derivadas

- Al registrar un uso en `USA`, se descuenta `cantidad_usada` del `stock_actual` del producto.
- Un servicio solo puede marcarse como `'entregado'` si tiene `fecha_inicio_garantia` y `fecha_fin_garantia` registradas.
- Un equipo está en garantía si `fecha_actual <= fecha_fin_garantia` de su servicio más reciente marcado como `'entregado'`.
