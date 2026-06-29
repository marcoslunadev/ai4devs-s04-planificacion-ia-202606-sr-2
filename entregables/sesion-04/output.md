# Output — User Stories FlowSync MVP

> Output crudo generado por Claude Sonnet 4.6 a partir del prompt de descomposición. Sin retocar.

---

### Módulo: Autenticación y gestión de cuenta

**User Story 1**
Como visitante, quiero crear una cuenta con email y contraseña, para acceder a FlowSync como usuario registrado.

**Criterios de aceptación**

- Given que el visitante está en el formulario de registro, When introduce un email válido y una contraseña de al menos 8 caracteres y envía el formulario, Then se crea la cuenta y el usuario llega a la pantalla de bienvenida.
- Given que el visitante introduce un email ya registrado, When intenta crear la cuenta, Then el sistema muestra un mensaje indicando que el email existe y ofrece un enlace hacia el inicio de sesión.
- Given que la contraseña tiene menos de 8 caracteres, When el visitante intenta enviar el formulario, Then el sistema bloquea el envío y muestra el error de validación correspondiente.
- Given que el registro fue exitoso, When el usuario llega a la pantalla de bienvenida, Then ve una frase explicando qué hace FlowSync y un elemento de llamada a la acción para crear su primera tarea.

---

**User Story 2**
Como usuario registrado, quiero iniciar sesión con mi email y contraseña, para acceder a mi espacio de trabajo en FlowSync.

**Criterios de aceptación**

- Given que el usuario está en la pantalla de inicio de sesión, When introduce credenciales válidas, Then accede a la aplicación y ve su listado de tareas.
- Given que el usuario introduce una contraseña incorrecta, When intenta iniciar sesión, Then el sistema muestra un mensaje de error y no concede acceso.
- Given que el usuario deja campos obligatorios vacíos, When intenta enviar el formulario, Then el sistema bloquea el envío y señala los campos pendientes.

---

**User Story 3**
Como usuario autenticado, quiero cerrar sesión, para terminar mi sesión activa de forma segura.

**Criterios de aceptación**

- Given que el usuario está autenticado, When ejecuta la acción de cierre de sesión, Then el token de acceso queda invalidado **(asumido)** y el usuario es redirigido a la pantalla de inicio de sesión.
- Given que el usuario cierra sesión, When intenta acceder a una ruta protegida directamente, Then el sistema le redirige al inicio de sesión sin mostrar datos de la cuenta.

---

### Módulo: Gestión de tareas

**User Story 4**
Como usuario autenticado, quiero crear una tarea indicando al menos un título, para registrar un nuevo pendiente en mi lista.

**Criterios de aceptación**

- Given que el usuario está en la vista de tareas, When completa el campo título y guarda, Then la tarea se crea con estado `pending` y aparece en el listado.
- Given que el usuario no rellena el título, When intenta guardar, Then el sistema bloquea el envío y muestra el error de campo obligatorio.
- Given que el usuario añade descripción y fecha límite junto al título, When guarda la tarea, Then todos los campos introducidos quedan persistidos correctamente.
- Given que la tarea se crea sin fecha límite, When el usuario consulta el listado, Then la tarea aparece sin fecha asociada y sin generar ningún evento en Google Calendar.

---

**User Story 5**
Como usuario autenticado, quiero ver el listado de mis tareas, para tener una visión general de todos mis pendientes.

**Criterios de aceptación**

- Given que el usuario tiene tareas creadas, When accede al listado, Then ve todas sus tareas ordenadas de forma que las más relevantes para el día actual aparecen primero **(asumido: criterio exacto de ordenación a definir en refinamiento)**.
- Given que el usuario no tiene ninguna tarea o todas están archivadas, When accede al listado, Then ve un estado vacío con una invitación a crear la primera tarea.
- Given que el usuario tiene tareas de distintos estados, When consulta el listado sin ningún filtro activo, Then ve tareas de todos los estados mezcladas **(asumido)**.

---

**User Story 6**
Como usuario autenticado, quiero editar cualquier campo de una tarea existente, para mantener la información actualizada.

**Criterios de aceptación**

- Given que el usuario accede a una tarea existente, When modifica el título, la descripción, la fecha límite o el estado y guarda, Then los cambios quedan persistidos.
- Given que el usuario elimina el título de una tarea al editar, When intenta guardar, Then el sistema bloquea el guardado y muestra el error de campo obligatorio.
- Given que el usuario cambia la fecha límite de una tarea que ya tiene un evento en Google Calendar, When guarda, Then el evento en Google Calendar se actualiza con la nueva fecha.

---

**User Story 7**
Como usuario autenticado, quiero cambiar el estado de una tarea, para reflejar su progreso real.

**Criterios de aceptación**

- Given que una tarea está en estado `pending`, When el usuario la marca como completada, Then el estado cambia a `completed`.
- Given que una tarea con fecha límite está en estado `pending`, When el usuario la marca como completada, Then el evento correspondiente en Google Calendar se elimina o se marca según corresponda.
- Given que una tarea está en cualquier estado, When el usuario la archiva, Then el estado cambia a `archived` y la tarea deja de aparecer en el listado por defecto **(asumido)**.

---

**User Story 8**
Como usuario autenticado, quiero borrar una tarea, para eliminar pendientes que ya no son relevantes.

**Criterios de aceptación**

- Given que el usuario selecciona la opción de borrado sobre una tarea, When confirma la acción **(asumido: confirmación explícita antes de borrar)**, Then la tarea se elimina del listado.
- Given que la tarea borrada tenía un evento asociado en Google Calendar, When se completa el borrado, Then el evento correspondiente se elimina de Google Calendar.
- Given que el usuario borra una tarea sin conexión Google activa, When se completa el borrado, Then la tarea desaparece del listado y no se intenta ninguna acción sobre Google Calendar.

---

### Módulo: Organización y filtrado

**User Story 9**
Como usuario autenticado, quiero filtrar mis tareas por estado, para enfocarme en un subconjunto específico de mis pendientes.

**Criterios de aceptación**

- Given que el usuario aplica el filtro "pendientes", When el listado se actualiza, Then solo muestra tareas con estado `pending`.
- Given que el usuario aplica el filtro "completadas", When el listado se actualiza, Then solo muestra tareas con estado `completed`.
- Given que el usuario aplica el filtro "archivadas", When el listado se actualiza, Then solo muestra tareas con estado `archived`.
- Given que no hay tareas que coincidan con el filtro activo, When el listado se actualiza, Then el usuario ve el estado vacío con invitación a crear una tarea **(asumido)**.

---

### Módulo: Exportación

**User Story 10**
Como usuario autenticado, quiero exportar mis tareas a un archivo CSV, para llevarme mis datos fuera de FlowSync.

**Criterios de aceptación**

- Given que el usuario solicita la exportación, When el sistema genera el archivo, Then el CSV descargado incluye al menos los campos título, descripción, estado y fecha límite de cada tarea.
- Given que el usuario tiene tareas sin descripción o sin fecha límite, When se exporta el CSV, Then esos campos aparecen vacíos en la fila correspondiente, sin omitir la fila.
- Given que el usuario no tiene ninguna tarea, When solicita la exportación, Then el sistema devuelve un CSV con solo la fila de cabeceras **(asumido)**.

---

### Módulo: Sincronización con Google Calendar

**User Story 11**
Como usuario autenticado, quiero conectar mi cuenta de Google a FlowSync, para habilitar la sincronización de mis tareas con Google Calendar.

**Criterios de aceptación**

- Given que el usuario no tiene cuenta de Google conectada, When inicia el flujo de autorización OAuth, Then es redirigido a Google para conceder permisos.
- Given que el usuario completa la autorización en Google, When regresa a FlowSync, Then su cuenta queda conectada y FlowSync puede escribir eventos en su Google Calendar.
- Given que el usuario ya tiene cuenta de Google conectada, When accede a la configuración de su cuenta **(asumido: existe una sección de configuración)**, Then ve el estado de la conexión como activa.

---

**User Story 12**
Como usuario autenticado con Google Calendar conectado, quiero que mis tareas con fecha límite aparezcan automáticamente como eventos en mi calendario, para ver mis pendientes sin gestionar dos herramientas por separado.

**Criterios de aceptación**

- Given que el usuario tiene Google Calendar conectado y crea una tarea con fecha límite, When la tarea se guarda, Then se crea un evento en su Google Calendar con el título de la tarea y la fecha correspondiente.
- Given que la tarea no tiene fecha límite, When se guarda, Then no se crea ningún evento en Google Calendar.
- Given que la API de Google no está disponible al intentar crear el evento, When la tarea se guarda, Then la tarea queda guardada en FlowSync y el sistema reintenta la sincronización más tarde.
- Given que el evento se crea correctamente, When el usuario abre Google Calendar, Then ve el evento con el nombre de la tarea en la fecha correspondiente.

---

**User Story 13**
Como usuario autenticado con Google Calendar conectado, quiero desconectar mi cuenta de Google, para dejar de sincronizar sin perder mis tareas.

**Criterios de aceptación**

- Given que el usuario tiene una cuenta de Google conectada, When ejecuta la acción de desconexión, Then FlowSync revoca el acceso y deja de sincronizar.
- Given que el usuario desconecta su cuenta de Google, When consulta su listado de tareas, Then todas las tareas siguen presentes tal como estaban antes de la desconexión.
- Given que la cuenta está desconectada, When el usuario crea o edita una tarea con fecha límite, Then no se realiza ningún intento de sincronización con Google Calendar.
