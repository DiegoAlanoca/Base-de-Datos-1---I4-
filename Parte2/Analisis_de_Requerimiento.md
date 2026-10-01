# Análisis de Requerimientos y Diseño Conceptual — Sistema de Control de Garantías e Inventario para Taller de Reparación Electrónica "Sigma Byte"

## 1. Resumen ejecutivo

Este documento especifica los requerimientos y el modelo conceptual de un sistema para digitalizar la operación del taller de reparación electrónica Sigma Byte: registro de clientes y sus equipos, control de garantías por cada reparación, inventario de herramientas del taller e inventario de productos de venta (piezas y consumibles), con trazabilidad de cada ingreso de stock.

---

## 2. Objetivo del sistema

Diseñar e implementar una base de datos que permita:

1. Registrar clientes y los equipos que traen al taller, identificando cada equipo de forma única.
2. Registrar cada reparación (servicio) con su fecha, descripción del problema, estado, técnico responsable y garantía asociada.
3. Consultar si un equipo específico sigue en garantía a la fecha actual.
4. Controlar el inventario de herramientas del taller mediante el registro de cada ingreso de stock.
5. Controlar el inventario de productos de venta (piezas y consumibles) mediante el registro de cada ingreso de stock, y su uso en las reparaciones.
6. Registrar el cobro de cada servicio.

### Alcance

**Incluye:** registro de clientes y equipos, órdenes de servicio con técnico responsable y garantía, inventario de herramientas con historial de ingresos, inventario de productos de venta con historial de ingresos, uso de productos en reparaciones.

**No incluye:** página web pública, manejo de múltiples sucursales, programación de citas.

---

## 3. Entidades y atributos

* **CLIENTE:** persona que trae uno o varios equipos a reparar.
  *Atributos:* id, nombre completo, C.I., celular.

* **EQUIPO:** cada aparato que entra al taller.
  *Atributos:* id, S/N (número de serie, único, funciona también como etiqueta impresa), categoría.

* **SERVICIO:** cada orden de reparación abierta sobre un equipo.
  *Atributos:* id, fecha de ingreso, descripción del problema, estado.

* **GARANTIA:** cobertura otorgada al cerrar un servicio. No toda orden de servicio tiene garantía — solo las que ya fueron entregadas.
  *Atributos:* id, tiempo de validez, términos, estado de la garantía.

* **VENDEDOR_OPERADOR:** persona que trabaja en el taller (técnico, dueño u otro rol).
  *Atributos:* id, nombre, rol.

* **PRODUCTO:** piezas y consumibles que se venden o se usan en las reparaciones.
  *Atributos:* id, nombre, tipo (Pieza / Consumible), precio de venta.

* **HERRAMIENTA:** herramientas propias del taller, usadas para reparar (no se venden ni se usan directamente en un servicio puntual, solo se almacenan).
  *Atributos:* id (código de modelo), marca, fotografía, estado.

* **ENTRADA_PRODUCTO:** cada ingreso de stock de un producto al inventario.
  *Atributos:* id, fecha de entrada, cantidad ingresada, costo unitario de esa entrada, proveedor.

* **ENTRADA_HERRAMIENTA:** cada ingreso de una herramienta al inventario del taller.
  *Atributos:* id, fecha de entrada, cantidad ingresada, costo unitario de esa entrada, proveedor.

> El precio de compra y el stock de un producto ya no son atributos fijos de `PRODUCTO`: se calculan a partir de la suma de sus `ENTRADA_PRODUCTO` (y, en el caso del stock, restando lo usado en servicios), porque el costo varía de una compra a otra y el stock cambia constantemente.
---

## 4. Diccionario de datos

| Entidad | Atributo | Tipo de dato | Rol | Descripción |
| :--- | :--- | :--- | :--- | :--- |
| **CLIENTE** | `id_cliente` | `INT` | PK | Autonumérico. |
| | `nombre_completo` | `VARCHAR(150)` | Atributo | — |
| | `ci` | `VARCHAR(20)` | Atributo | Puede ser `NULL`. |
| | `celular` | `VARCHAR(20)` | Atributo | — |
| **EQUIPO** | `id_equipo` | `INT` | PK | Autonumérico. |
| | `sn` | `VARCHAR(50)` | Atributo | `UNIQUE`. También funciona como etiqueta impresa. |
| | `categoria` | `VARCHAR(30)` | Atributo | `'computadora de escritorio' / 'laptop' / 'consola' / 'otro'`. |
| | `id_cliente` | `INT` | FK → CLIENTE | Dueño del equipo. |
| **SERVICIO** | `id_servicio` | `INT` | PK | Autonumérico. |
| | `fecha_ingreso` | `DATE` | Atributo | — |
| | `descripcion_problema` | `VARCHAR(255)` | Atributo | — |
| | `estado` | `VARCHAR(20)` | Atributo | `'en proceso' / 'terminado' / 'entregado'`. |
| | `id_equipo` | `INT` | FK → EQUIPO | — |
| | `id_vendedor` | `INT` | FK → VENDEDOR_OPERADOR | Técnico responsable. |
| **GARANTIA** | `id_garantia` | `INT` | PK | Autonumérico. |
| | `tiempo_validez` | `INT` | Atributo | Días de validez. |
| | `terminos` | `VARCHAR(255)` | Atributo | — |
| | `estado_garantia` | `VARCHAR(20)` | Atributo | `'vigente' / 'vencida'`. |
| | `id_servicio` | `INT` | FK → SERVICIO | `UNIQUE` (una garantía por servicio). |
| **VENDEDOR_OPERADOR** | `id_usuario` | `INT` | PK | Autonumérico. |
| | `nombre` | `VARCHAR(100)` | Atributo | — |
| | `rol` | `VARCHAR(30)` | Atributo | — |
| **PRODUCTO** | `id_producto` | `INT` | PK | Autonumérico. |
| | `nombre` | `VARCHAR(100)` | Atributo | — |
| | `tipo` | `VARCHAR(20)` | Atributo | `'Pieza' / 'Consumible'`. |
| | `precio_venta` | `DECIMAL(10,2)` | Atributo | ≥ 0. |
| **HERRAMIENTA** | `id_herramienta` | `VARCHAR(50)` | PK | Código de modelo. |
| | `marca` | `VARCHAR(50)` | Atributo | — |
| | `fotografia` | `VARCHAR(255)` | Atributo | Enlace o referencia a la foto. |
| | `estado` | `VARCHAR(30)` | Atributo | Ej. `'disponible' / 'en uso' / 'dañada'`. |
| **ENTRADA_PRODUCTO** | `id_entrada` | `INT` | PK | Autonumérico. |
| | `fecha_entrada` | `DATE` | Atributo | — |
| | `cantidad_ingresada` | `INT` | Atributo | > 0. |
| | `costo_unitario` | `DECIMAL(10,2)` | Atributo | ≥ 0. Costo de esa compra puntual. |
| | `proveedor` | `VARCHAR(100)` | Atributo | — |
| | `id_producto` | `INT` | FK → PRODUCTO | — |
| | `id_usuario` | `INT` | FK → VENDEDOR_OPERADOR | Quién registró el ingreso. |
| **ENTRADA_HERRAMIENTA** | `id_entrada` | `INT` | PK | Autonumérico. |
| | `fecha_entrada` | `DATE` | Atributo | — |
| | `cantidad_ingresada` | `INT` | Atributo | > 0. |
| | `costo_unitario` | `DECIMAL(10,2)` | Atributo | ≥ 0. |
| | `proveedor` | `VARCHAR(100)` | Atributo | — |
| | `id_herramienta` | `VARCHAR(50)` | FK → HERRAMIENTA | — |
| | `id_usuario` | `INT` | FK → VENDEDOR_OPERADOR | Quién registró el ingreso. |
| **USA** *(relación N:M)* | `id_servicio` | `INT` | FK → SERVICIO | — |
| | `id_producto` | `INT` | FK → PRODUCTO | — |
| | `cantidad_usada` | `INT` | Atributo | > 0. |

---

## 5. Relaciones y cardinalidades

* `CLIENTE (1) — EQUIPO (N)`: un cliente tiene uno o varios equipos; cada equipo es de un único cliente.
* `EQUIPO (1) — SERVICIO (N)`: un equipo puede tener uno o varios servicios; cada servicio pertenece a un único equipo.
* `SERVICIO (0,1) — GARANTIA (1,1)`: un servicio puede tener 0 o 1 garantía (solo si ya fue entregado); una garantía pertenece siempre a un único servicio.
* `VENDEDOR_OPERADOR (0,N) — SERVICIO (1,1)`: un operador puede realizar cero, uno o varios servicios (permite dar de alta operadores que aún no atendieron ninguno); cada servicio lo realiza un único operador.
* `SERVICIO (0,N) — USA — (0,N) PRODUCTO`: un servicio puede usar varios productos o ninguno; un producto puede usarse en varios servicios o en ninguno (puede quedarse en inventario sin usarse). Relación N:M con atributo propio `cantidad_usada`.
* `VENDEDOR_OPERADOR (1) — ENTRADA_PRODUCTO (N)`: un operador registra varias entradas de producto; cada entrada la registra un único operador.
* `PRODUCTO (1) — ENTRADA_PRODUCTO (N)`: un producto puede tener varias entradas de stock a lo largo del tiempo; cada entrada corresponde a un único producto.
* `VENDEDOR_OPERADOR (1) — ENTRADA_HERRAMIENTA (N)`: un operador registra varias entradas de herramienta; cada entrada la registra un único operador.
* `HERRAMIENTA (1) — ENTRADA_HERRAMIENTA (N)`: una herramienta puede tener varias entradas de stock; cada entrada corresponde a una única herramienta.

---

## 6. Restricciones e integridad de datos

### 6.1 Restricciones de dominio (`CHECK`)

- `categoria IN ('computadora de escritorio','laptop','consola','otro')`.
- `estado (SERVICIO) IN ('en proceso','terminado','entregado')`.
- `estado_garantia IN ('vigente','vencida')`.
- `tipo IN ('Pieza','Consumible')`.
- `precio_venta >= 0`, `costo_unitario >= 0`, `cantidad_usada > 0`, `cantidad_ingresada > 0`.

### 6.2 Unicidad y obligatoriedad

- `UNIQUE` en `EQUIPO.sn` y `GARANTIA.id_servicio`.
- `NOT NULL` en `celular`, `sn`, `fecha_ingreso`, `estado`, y en todas las llaves foráneas obligatorias.
- `ci` es el único atributo de `CLIENTE` que permite valor vacío.

### 6.3 Ciclo de vida: composición vs. agregación

**Composición (dependen de su padre, se eliminan en cascada):**
- `GARANTIA` respecto a `SERVICIO` (`ON DELETE CASCADE`).
- `USA` respecto a `SERVICIO` (`ON DELETE CASCADE`).
- `ENTRADA_PRODUCTO` respecto a `PRODUCTO` (`ON DELETE RESTRICT`, ver 6.4 — se conserva el historial de compras).
- `ENTRADA_HERRAMIENTA` respecto a `HERRAMIENTA` (`ON DELETE RESTRICT`, por la misma razón).

**Agregación (independientes, se restringe el borrado si hay historial):**
- `CLIENTE` en `EQUIPO` (`ON DELETE RESTRICT`).
- `EQUIPO` en `SERVICIO` (`ON DELETE RESTRICT`).
- `VENDEDOR_OPERADOR` en `SERVICIO`, `ENTRADA_PRODUCTO` y `ENTRADA_HERRAMIENTA` (`ON DELETE RESTRICT`).
- `PRODUCTO` en `USA` (`ON DELETE RESTRICT`).

### 6.4 Reglas de negocio derivadas

- El stock actual de un producto se calcula como `SUM(ENTRADA_PRODUCTO.cantidad_ingresada) - SUM(USA.cantidad_usada)` para ese producto.
- El costo de compra de referencia de un producto se obtiene de su `ENTRADA_PRODUCTO` más reciente (o un promedio ponderado, según se necesite).
- Un servicio solo puede marcarse como `'entregado'` si tiene una `GARANTIA` registrada.
- Un equipo está en garantía si `fecha_actual` está dentro del `tiempo_validez` de la `GARANTIA` de su servicio más reciente marcado como `'entregado'`.

---

## 7. Normalización

### 7.1 Primera Forma Normal (1FN)

Una tabla está en 1FN si todos sus atributos son atómicos (no hay listas ni grupos repetidos) y existe una clave primaria.

Todas las entidades del modelo cumplen 1FN:
- Ningún atributo almacena más de un valor (por ejemplo, `categoria` y `tipo` son valores únicos de un dominio fijo, no listas).
- No hay grupos repetidos de columnas (como `producto1`, `producto2`, `producto3`); en su lugar, el uso de productos en un servicio se modela con la tabla `USA`, una fila por cada producto usado.
- Todas las tablas tienen una clave primaria definida (`id_cliente`, `id_equipo`, `id_servicio`, etc.).

### 7.2 Segunda Forma Normal (2FN)

Una tabla está en 2FN si está en 1FN y, además, todo atributo no clave depende de la **clave primaria completa** (relevante solo cuando la clave es compuesta).

- Todas las entidades principales (`CLIENTE`, `EQUIPO`, `SERVICIO`, `GARANTIA`, `VENDEDOR_OPERADOR`, `PRODUCTO`, `HERRAMIENTA`, `ENTRADA_PRODUCTO`, `ENTRADA_HERRAMIENTA`) usan una clave primaria simple (un solo campo autonumérico), por lo que no puede existir dependencia parcial — cumplen 2FN automáticamente.
- La única tabla con clave compuesta es `USA` (`id_servicio` + `id_producto`). Su único atributo no clave, `cantidad_usada`, depende de **ambas** partes de la clave a la vez (la cantidad usada solo tiene sentido para esa combinación específica de servicio y producto), no de una sola. Cumple 2FN.

### 7.3 Tercera Forma Normal (3FN)

Una tabla está en 3FN si está en 2FN y, además, ningún atributo no clave depende de otro atributo no clave (sin dependencias transitivas).

Revisando cada entidad:
- `CLIENTE`, `EQUIPO`, `SERVICIO`, `GARANTIA`, `VENDEDOR_OPERADOR`, `HERRAMIENTA`: cada atributo describe directamente a la entidad por su id; ningún atributo depende de otro atributo no clave. Cumplen 3FN.
- `PRODUCTO`: en la versión anterior del modelo, `PRODUCTO` incluía `precio_compra` y `stock`, pero ambos en realidad dependían del historial de compras (de qué `ENTRADA_PRODUCTO` se trate), no directamente de `id_producto` — una dependencia transitiva vía el historial de entradas, que además obligaba a actualizar manualmente esos valores cada vez que entraba o salía stock, generando redundancia e inconsistencia. Al moverlos a `ENTRADA_PRODUCTO` (donde `costo_unitario` sí depende directamente de `id_entrada`, y el stock se calcula con una consulta en vez de almacenarse), `PRODUCTO` queda en 3FN: sus atributos restantes (`nombre`, `tipo`, `precio_venta`) dependen únicamente de `id_producto`.
- `ENTRADA_PRODUCTO` y `ENTRADA_HERRAMIENTA`: `fecha_entrada`, `cantidad_ingresada`, `costo_unitario` y `proveedor` dependen directamente de `id_entrada` (cada entrada es un evento específico); ninguno depende de otro atributo no clave. Cumplen 3FN.
- `USA`: `cantidad_usada` depende de la clave completa, sin relación transitiva con otro atributo no clave. Cumple 3FN.

**Conclusión:** el modelo, con las entidades `ENTRADA_PRODUCTO` y `ENTRADA_HERRAMIENTA` separadas de `PRODUCTO` y `HERRAMIENTA`, cumple con las tres primeras formas normales.
