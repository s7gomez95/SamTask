# Roles y permisos de SamTask

Versión 2 · Borrador de diseño. Estos roles se programan en los módulos 5 a 7 (base de datos, backend y seguridad). Mientras tanto sirven como guía para diseñar las pantallas y los datos.

## Roles

| Rol | Qué hace |
|---|---|
| **Administrador / Gerente** | Gestiona usuarios y ve los reportes generales. |
| **Coordinador** | Crea, aprueba y asigna actividades. |
| **Técnico** | Ve solo sus actividades asignadas y las ejecuta en sitio. |
| **CEC** (Centro de Experiencia de Clientes) | Revisa y valida los informes, o los devuelve al técnico. |
| **Cliente** | Ve y crea actividades solo de su empresa. |

## Matriz de permisos

| Acción | Admin | Coordinador | Técnico | CEC | Cliente |
|---|---|---|---|---|---|
| Crear actividad | Sí | Sí | No | No | Sí (queda "Solicitada") |
| Aprobar y asignar | Sí | Sí | No | No | No |
| Ver actividades | Todas | Todas | Solo las suyas | Solo las que van a validar (provisional) | Solo las de su empresa |
| Llegué / Terminé, fotos e informe | No | No | Sí, en las suyas | No | No |
| Validar o devolver informe | Sí | No | No | **Sí** | No |
| Ver historial del vehículo | Todos | Todos | Los que atendió | Todos | Solo sus vehículos |
| Gestionar usuarios | Sí | No | No | No | No |
| Ver reportes generales | Sí | Parcial | No | No | No |

## Estados de una actividad

Ruta normal:

`Solicitada → Asignada → En sitio → Informe listo → Validada → Entregada al cliente`

Ruta alterna cuando el CEC encuentra algo incompleto:

`Informe listo → Devuelta → Informe listo`

### Quién puede cambiar cada estado

| Cambio de estado | Quién lo hace |
|---|---|
| Solicitada → Asignada | Coordinador (o Admin) |
| Asignada → En sitio | Técnico ("Llegué") |
| En sitio → Informe listo | Técnico ("Terminé") |
| Informe listo → Validada | CEC (o Admin) |
| Informe listo → Devuelta | CEC (o Admin), con una nota |
| Devuelta → Informe listo | Técnico |
| Validada → Entregada al cliente | Por definir |

## Principios de diseño

1. **Alcance, además de rol.** No basta con "es cliente": hay que saber de cuál empresa. Un cliente nunca debe ver datos de otro cliente. Es el riesgo más serio del sistema.
2. **Permisos por acción, no solo por rol.** Así, si mañana existe un "técnico senior" que valida, solo se cambia su lista de permisos.
3. **Cada estado lo cambia solo el rol autorizado.** Por ejemplo, el técnico no puede marcar "Validada".
4. **La evidencia no se borra ni se edita en silencio.** Después de "Informe listo", un cambio queda como corrección, con registro de quién, cuándo y qué cambió.
5. **Cada validación guarda quién la hizo** (nombre y hora), aunque el permiso sea del rol CEC, que es un grupo de personas.
6. **Un solo validador por actividad.** Si dos personas del CEC abren la misma actividad, solo una puede validarla. Se resuelve en el servidor.
7. **La seguridad vive en el servidor.** Ocultar un botón en la pantalla no protege nada. Cada petición se revisa en el servidor.

## Decisiones tomadas

- El informe del técnico es de **formato libre**. Se puede pedir un mínimo de datos que siempre convienen guardar (kilometraje, falla encontrada y solución), sin obligar un formato fijo.
- El **cliente también crea actividades** y ve el historial de cada una.
- **CEC** significa Centro de Experiencia de Clientes y valida los informes.

## Pendientes por definir

- [ ] ¿Qué otras tareas hace el CEC y qué información necesita ver? (Podría tener contacto directo con clientes.)
- [ ] ¿Quién y cómo marca "Entregada al cliente"?
- [ ] ¿Qué puede ver y hacer el coordinador en los reportes?
- [ ] ¿Qué hace exactamente la app que la empresa tiene en camino, y qué parte de estos roles ya cubre?
- [ ] Validar este diseño con la empresa antes de usar datos reales de clientes y fotos.

## Notas

- En el hackatón y en el prototipo se pueden mostrar pantallas distintas por rol, pero **eso no es seguridad real**: es solo diseño visual.
- No subir al repositorio datos reales (placas, fotos, nombres de clientes). Usar datos inventados.
