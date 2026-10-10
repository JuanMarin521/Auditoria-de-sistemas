# Matriz inicial de riesgos
### Auditoría de Sistemas (ET0114) · Sesión 7 · Metro de Medellín — Área de Cables Aéreos

**Integrantes:**
- *Nombre completo* <!-- reemplazar -->
- *Nombre completo* <!-- reemplazar -->

---

## Caso y contexto actualizado

Se continúa el caso del Metro de Medellín, Área de Cables Aéreos, que gestiona el mantenimiento de telecabinas con SAP PM. Este taller se enfoca en un proceso concreto: **la confirmación y el cierre de órdenes de mantenimiento preventivo y correctivo en SAP PM**, incluyendo el registro del serial de componentes críticos (mordazas/pinzas), los tiempos de ejecución y el acceso de los técnicos al sistema desde los talleres. Los marcos elegidos en el primer taller (COBIT 2019, ISO/IEC 27001, ITIL 4) siguen vigentes y orientan qué controles se esperan en este proceso.

## Supuestos

- No se tuvo acceso directo a la parametrización de SAP PM ni a las políticas de red del Metro; los riesgos se formulan como situaciones posibles a partir de la evidencia disponible, no como fallas confirmadas.
- La muestra de 47 confirmaciones exportadas (`ordenes_SAP_metro.xlsx`, periodo 1–13 de marzo) se toma como indicio representativo del proceso, no como un hallazgo de auditoría cerrado.
- Se asume que el proceso de notificación actual exige transcripción manual desde papel hacia SAP, tal como se describió en el primer taller.
- Los valores de probabilidad e impacto se asignan con la escala sugerida (1 a 5) y se justifican con lo observado en la muestra.

## Activos o procesos identificados

1. **Serial de componentes críticos** (mordazas/pinzas) registrado al cerrar la orden en SAP PM.
2. **Proceso de confirmación y cierre de órdenes** de mantenimiento preventivo/correctivo en SAP PM.
3. **Registro de sub-operaciones ejecutadas** dentro de cada orden de mantenimiento.
4. **Red corporativa de los talleres** de Cables Aéreos (acceso físico y lógico a la red donde corre SAP).
5. **Credenciales de acceso a SAP PM** de los técnicos de mantenimiento.
6. **Registro de tiempos de inicio y fin** de las actividades (fecha y hora real de trabajo).

## Matriz de riesgos

### R-01 — Serial de componentes críticos

| Campo | Detalle |
|---|---|
| Activo o proceso | Serial de mordazas/pinzas registrado en la orden de SAP PM |
| Amenaza | Un técnico cierra la orden sin registrar el serial del componente instalado |
| Vulnerabilidad | El campo "Equipo/Serial" no está definido como obligatorio en la clase de orden |
| Riesgo | Una mordaza podría quedar en servicio más allá de su vida útil porque no es posible identificar cuál fue instalada, exponiendo a los usuarios de la telecabina |
| Probabilidad | 4 |
| Impacto | 5 |
| Nivel | 20 |
| Control actual o evidencia | En la muestra exportada, ninguna de las 47 confirmaciones tiene un serial registrado en campo estructurado; solo un texto libre menciona "seriales" sin mostrar el valor |
| Control propuesto | Volver obligatorio el campo serial en las clases de orden de componentes críticos y bloquear el cierre de la orden si el campo está vacío |
| Justificación | Es el riesgo con mayor impacto potencial (seguridad física de pasajeros) y hay evidencia directa en la muestra de que el campo no se está diligenciando |

### R-02 — Cierre de órdenes sin aprobación de supervisor

| Campo | Detalle |
|---|---|
| Activo o proceso | Proceso de confirmación y cierre de órdenes en SAP PM |
| Amenaza | Un técnico cierra una orden con datos incompletos o inconsistentes sin que nadie los revise |
| Vulnerabilidad | No existe un paso de aprobación (workflow) antes del cierre automático de la orden |
| Riesgo | Órdenes con información inconsistente quedan cerradas en firme y pierden la oportunidad de corregirse antes de afectar los indicadores de mantenimiento |
| Probabilidad | 4 |
| Impacto | 3 |
| Nivel | 12 |
| Control actual o evidencia | En la muestra, 28 de 47 confirmaciones tienen una fecha de inicio real incoherente con la fecha de fin (de 3 hasta 365 días de diferencia), y quedaron cerradas sin corrección |
| Control propuesto | Activar un workflow de aprobación del supervisor antes del cierre y una validación automática que impida fechas de inicio posteriores a la fecha de fin |
| Justificación | La inconsistencia se repite en más de la mitad de la muestra, lo que sugiere un problema sistemático de control, no casos aislados |

### R-03 — Acceso de equipos personales (BYOD) a la red de los talleres

| Campo | Detalle |
|---|---|
| Activo o proceso | Red corporativa de los talleres de Cables Aéreos |
| Amenaza | Un computador personal infectado, conectado a la red del taller, introduce malware o captura credenciales (keylogger) |
| Vulnerabilidad | Los switches del taller no cuentan con control de acceso a la red (802.1X o filtrado MAC) que limite la conexión a equipos del dominio |
| Riesgo | Credenciales de SAP PM podrían robarse o la red operativa podría infectarse con ransomware a través de un equipo personal no controlado |
| Probabilidad | 3 |
| Impacto | 5 |
| Nivel | 15 |
| Control actual o evidencia | Se confirmó en el primer taller que SAP exige contraseña, pero no hay evidencia de que la red rechace dispositivos ajenos al dominio corporativo |
| Control propuesto | Implementar control de acceso a la red (802.1X o filtrado MAC) y segmentar la red para aislar equipos invitados/personales de los servidores core |
| Justificación | El impacto es alto porque compromete tanto la integridad de SAP como la red operativa completa del área |

### R-04 — Transcripción manual de tiempos (papel → SAP)

| Campo | Detalle |
|---|---|
| Activo o proceso | Registro de tiempos de inicio y fin de las actividades de mantenimiento |
| Amenaza | Ninguna amenaza externa; el riesgo proviene de un error humano recurrente en la digitación |
| Vulnerabilidad | El proceso de notificación exige doble digitación: el técnico anota en papel y luego transcribe a SAP, sin una herramienta que capture el dato en el momento |
| Riesgo | Los tiempos registrados en SAP no reflejan la duración real del trabajo, lo que distorsiona los indicadores de mantenimiento y puede ocultar demoras reales |
| Probabilidad | 5 |
| Impacto | 3 |
| Nivel | 15 |
| Control actual o evidencia | En 26 de 28 confirmaciones con fechas incoherentes, el "trabajo real" reportado sí coincide con la diferencia de horas del mismo día, lo que indica que el error está en la transcripción de la fecha, no en el trabajo ejecutado |
| Control propuesto | Capturar el tiempo desde tablets industriales o escáneres RFID en el punto de trabajo, eliminando la doble digitación |
| Justificación | Es el riesgo más frecuente en la muestra (probabilidad alta) y explica directamente el hallazgo de R-02 |

### R-05 — Sub-operaciones no registradas

| Campo | Detalle |
|---|---|
| Activo o proceso | Registro de sub-operaciones ejecutadas dentro de cada orden |
| Amenaza | Un técnico confirma la orden sin detallar qué sub-operaciones específicas ejecutó |
| Vulnerabilidad | El campo "Suboperación" no se diligencia en el flujo de confirmación, aunque el texto libre mencione sub-tareas realizadas |
| Riesgo | No es posible auditar el cumplimiento del plan de mantenimiento preventivo a nivel de detalle, lo que puede retrasar la detección de fallas repetidas en una sub-tarea específica |
| Probabilidad | 4 |
| Impacto | 3 |
| Nivel | 12 |
| Control actual o evidencia | La columna "Suboperación" está vacía en las 47 confirmaciones de la muestra, aunque 15 textos de confirmación mencionan explícitamente haber ejecutado sub-operaciones |
| Control propuesto | Obligar la selección de la sub-operación desde un catálogo estructurado antes de permitir la confirmación de la orden |
| Justificación | Es un patrón consistente (100 % de la muestra) y afecta directamente la calidad de los datos que soportan la planeación del mantenimiento |

### R-06 — Mesa de Servicio sin seguimiento de errores de usabilidad

| Campo | Detalle |
|---|---|
| Activo o proceso | Reportes de la Mesa de Servicio sobre el uso de SAP PM en campo |
| Amenaza | Los técnicos reportan de manera informal (de boca en boca) los problemas de digitación, sin dejar registro formal |
| Vulnerabilidad | No hay evidencia de que la Mesa de Servicio esté clasificando o dando seguimiento a tickets de usabilidad/errores de notificación en SAP |
| Riesgo | La organización no detecta a tiempo que el diseño actual del servicio (doble digitación) es la causa raíz de los errores de R-02, R-04 y R-05, y retrasa la mejora del proceso |
| Probabilidad | 3 |
| Impacto | 2 |
| Nivel | 6 |
| Control actual o evidencia | Aún no se ha solicitado ni recibido el reporte de tickets de la Mesa de Servicio del último año (evidencia pendiente del primer taller) |
| Control propuesto | Clasificar los tickets relacionados con usabilidad de SAP PM y revisar mensualmente su tendencia para alimentar la mejora del servicio |
| Justificación | Es el riesgo de menor nivel porque es un efecto de segundo orden, pero su control habilita la mejora sostenida de los demás riesgos |

## Resumen y priorización

| ID | Activo o proceso | Riesgo (resumen) | Prob. | Impacto | Nivel |
|---|---|---|---:|---:|---:|
| R-01 | Serial de mordazas/pinzas | Componente crítico sin trazabilidad de serial | 4 | 5 | **20** |
| R-03 | Red de los talleres | Equipo personal (BYOD) compromete la red/credenciales | 3 | 5 | **15** |
| R-04 | Registro de tiempos | Doble digitación genera tiempos/fechas erróneos | 5 | 3 | **15** |
| R-02 | Cierre de órdenes | Cierre sin aprobación de supervisor | 4 | 3 | **12** |
| R-05 | Sub-operaciones | Sub-operaciones ejecutadas no quedan registradas | 4 | 3 | **12** |
| R-06 | Mesa de Servicio | Sin seguimiento de errores de usabilidad | 3 | 2 | **6** |

El orden de prioridad coincide con el orden de los marcos elegidos en el primer taller: los riesgos de mayor nivel (R-01, R-02, R-04, R-05) corresponden a controles de proceso de negocio (COBIT 2019), seguidos del riesgo de red (R-03, ISO/IEC 27001) y, por último, el riesgo de diseño del servicio (R-06, ITIL 4).
