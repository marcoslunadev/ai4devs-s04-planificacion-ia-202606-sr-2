# Poke-holes — User Story 7: Cambiar el estado de una tarea

Story analizada: **US7** — Como usuario autenticado, quiero cambiar el estado de una tarea, para reflejar su progreso real.

---

## Hallazgo 1 — Acción sobre Google Calendar al completar es ambigua

El AC dice "el evento se elimina o se marca según corresponda" sin definir cuál de las dos. Son comportamientos distintos: un evento eliminado desaparece del calendario; uno marcado sigue visible. Esta decisión debe tomarse en refinamiento, no durante el desarrollo.

## Hallazgo 2 — Las transiciones de estado no están modeladas

Los AC solo cubren `pending → completed` y `* → archived`. No se define si es posible:
- Deshacer un completado (`completed → pending`).
- Desarchivar una tarea (`archived → pending` o `archived → completed`).

Si esas transiciones no existen, hay que decirlo. Si existen, faltan sus AC y sus implicaciones en Google Calendar.

## Hallazgo 3 — El estado de Google Calendar al archivar no está cubierto

El AC de archivado solo menciona que la tarea desaparece del listado. No dice qué ocurre con el evento en Google Calendar si la tarea tenía fecha límite: ¿se elimina, se mantiene? Es el mismo gap que en "completar", pero para `archived`.

## Hallazgo 4 — Fallo de sincronización tras cambio de estado no contemplado

La story no cubre el escenario en que el cambio de estado se guarda en FlowSync pero falla la actualización o eliminación del evento en Google Calendar. ¿El estado queda persistido igualmente? ¿Se muestra algún aviso? El PRD lo menciona explícitamente para la creación de eventos — debería cubrirse también aquí.

## Hallazgo 5 — "Cualquier estado → archived" incluye casos degenerados

Archivar una tarea ya archivada no está definido: ¿no-op, error silencioso o error visible? Además, archivar desde `completed` puede tener implicaciones distintas si el evento de Calendar ya fue eliminado al completar — el sistema necesita saber que no hay nada que eliminar para no fallar.
