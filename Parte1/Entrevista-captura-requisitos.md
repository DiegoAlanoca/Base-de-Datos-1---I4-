# Entrevista — Captura de requisitos

**Materia:** Base de Datos 1
**Actividad:** Captura de requisitos para sistema de información real
**Entrevistador:** Diego Ronald Alanoca Avircata
**Entrevistado:** Alexis Acosta — Propietario, taller de reparación electrónica *Sigma Byte*
**Modalidad:** Presencial
**Duración aproximada:** 8 min

---

## Contexto

El objetivo de la entrevista es identificar la necesidad real de un usuario para, a partir de ella, construir el modelo conceptual de una base de datos. Alexis Acosta administra un taller de reparación electrónica de reciente apertura (computadoras y consolas) y actualmente no cuenta con ningún sistema digital para gestionar su operación.

---

## Transcripción

**D:** Hola Alexis, ¿qué tal?

**A:** Bien, gracias.

**D:** ¿Te parece si comenzamos con la entrevista?

**A:** Sí, dale.

**D:** Perfecto. Vamos entendiendo cómo solucionar tu problema. ¿Qué problemas tienes actualmente con tus clientes o con tu sistema de trabajo?

**A:** Actualmente no tengo ningún sistema para mi taller, porque recién lo estoy montando. Mi mayor problema es llevar el control de las reparaciones que realizo y saber qué tiempo de garantía tiene cada una.

**D:** O sea que ahora mismo no manejas ningún sistema de control, ¿verdad?

**A:** No, ninguno.

**D:** ¿Y para tus herramientas y repuestos? ¿Están manejando algún tipo de inventario? Porque me imagino que algo de control debes tener.

**A:** Tengo una hoja de Excel donde anoto todos los activos y piezas, pero tengo que actualizarla manualmente cada vez, y a veces se me pasa.

**D:** Entendido. Entonces, resumiendo, te falta tanto el sistema de inventario como el de control de clientes.

**A:** Sí, así es.

**D:** Bueno, hablemos un poco de cómo te gustaría tener ese control y por qué medio te gustaría acceder a él. Por ejemplo, si preferirías revisar las garantías o el inventario a través de una página web.

**D:** ¿Tienes página web ahora mismo?

**A:** Eh... no, tampoco tengo.

**D:** Está bien, no hay problema, entonces podemos construir todo desde cero.

**D:** ¿Te parece bien que el control del sistema se pueda hacer también desde el celular?

**A:** Sí, sería lo mejor, pero no sé cuánto costaría algo así.

**D:** Como te comentaba antes, este es un proyecto universitario, así que el único costo que tendrías serían tus gastos operativos, como el alojamiento de la web o alguna suscripción a la plataforma donde esté tu inventario, nada más.

**A:** Ah, ok, me parece bien.

**D:** Última pregunta: de estos tres puntos —página web, control de inventario y control de garantías de clientes— ¿cuál dirías que es el más urgente para ti ahora mismo?

**A:** El control de garantías de clientes, sin duda. No puedo arriesgarme a tener otro problema por no saber quién es mi cliente o si su reparación todavía está en garantía o no.

**D:** Perfecto, con eso ya tengo una buena base. Voy a empezar a armar el diagrama y te lo paso para que le des un vistazo durante esta semana.

**A:** Me parece bien, gracias.

*(Fin de la entrevista)*

---

## Notas

- Necesidad prioritaria identificada: **control de garantías de clientes por reparación**.
- Necesidades secundarias: control de inventario de herramientas/piezas (hoy manejado en Excel, actualización manual) y, más adelante, una página web como canal de acceso.
- El sistema debería ser accesible también desde celular.
- Restricción principal: es un proyecto universitario, por lo que Alexis solo asumiría costos operativos (hosting, suscripciones), no de desarrollo.
