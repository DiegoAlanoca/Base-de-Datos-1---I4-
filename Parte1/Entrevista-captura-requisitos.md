# Narración de Requisitos — Taller de Reparación Electrónica "Sigma Byte"

Documento elaborado siguiendo la guía "De requisitos narrativos a Diagrama Entidad-Relación (DER)", a partir de la entrevista realizada a Alexis Acosta.

---

## Título del caso

Control de clientes, equipos y garantías — Taller de reparación electrónica "Sigma Byte"

---

## Narración del cliente

Soy Alexis Acosta, dueño del taller de reparación electrónica Sigma Byte, un negocio de reciente apertura dedicado a reparar computadoras de escritorio, laptops, consolas y otros equipos electrónicos. Actualmente no cuento con ningún sistema digital para gestionar mi operación: llevo el inventario de herramientas y piezas en una hoja de Excel que tengo que actualizar manualmente cada vez, y no tengo pagina web.

Mi mayor problema es el control de las reparaciones que realizo y saber que tiempo de garantía tiene cada una. Cuando un cliente trae un equipo, necesito registrar sus datos (nombre completo, celular, y su C.I. si es que lo tiene a mano, porque a veces no lo trae), y también los datos del equipo: cada equipo es único y se identifica con su número de serie (S/N), que además me sirve para imprimir una etiqueta rápida y pegarla al equipo mientras está en el taller. Un mismo cliente me puede traer varios equipos distintos, pero cada equipo es siempre de un solo dueño.

Cada vez que recibo un equipo para reparar, se genera un servicio: ahí anoto la fecha en que ingresó, una descripción del problema, en que estado está la reparación, y el tiempo de garantía que doy una vez entregado. Un equipo puede tener varios servicios si vuelve mas adelante por otra falla.

Por ahora me gustaría que se pueda revisar también desde el celular. Como es un proyecto universitario, mis gastos serían solo los operativos (como el hosting o alguna suscripción), no el desarrollo. Lo mas urgente para mi es el control de garantías de clientes, el inventario y la pagina web pueden esperar.

---

## Suposiciones

- El C.I. del cliente puede quedar vacío, no todos lo traen a la mano.
- La etiqueta de impresión rápida no es un dato aparte, es el mismo S/N impreso.
- La categoría del equipo (computadora de escritorio, laptop, consola, otro) es solo un dato más del equipo, no se le puso más vueltas.
- El inventario y la página web no entran en este modelo todavía porque Alexis dijo que no son lo más urgente.

---

## Entidades iniciales

- Cliente
- Equipo
- Servicio

---

## Reglas y restricciones extra

- El S/N del equipo no se puede repetir.
- El C.I. puede ir vacío.
- Cada equipo es de un solo cliente.
- La categoría del equipo solo puede ser una de estas: computadora de escritorio, laptop, consola, otro equipo electrónico.

---

## Entidades y atributos

- **Cliente**
  - Nombre completo
  - C.I. (puede ir vacío)
  - Celular

- **Equipo**
  - S/N (número de serie, sirve también como etiqueta impresa)
  - Categoría

- **Servicio**
  - Fecha de ingreso
  - Descripción del problema
  - Estado
  - Tiempo de garantía

---

## Relaciones y cardinalidades

- **Cliente 1 — N Equipo**: un cliente puede tener uno o varios equipos, cada equipo es de un solo cliente.
- **Equipo 1 — N Servicio**: un equipo puede tener uno o varios servicios, cada servicio es de un solo equipo.

---

## Restricciones de negocio

- No se puede registrar un servicio si no está ligado a un equipo.
- No se puede registrar un equipo si no está ligado a un cliente.
- El tiempo de garantía se anota cuando se entrega el equipo reparado.

---

## Representación para DER

- Entidades: rectángulos para Cliente, Equipo, Servicio.
- Relaciones: rombos "Posee" (Cliente–Equipo) y "Genera" (Equipo–Servicio).
- Cardinalidades: Cliente (1) — (N) Equipo; Equipo (1) — (N) Servicio.
  
