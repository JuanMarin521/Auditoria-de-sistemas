# Informe de Auditoría de Sistemas y Plan de Acción

**Asignatura:** ET0114 Auditoría de Sistemas  
**Organización:** Empresa de Transporte Masivo del Valle de Aburrá (Metro de Medellín) — Área de Cables Aéreos  
**Sistema auditado:** ERP SAP — Módulo PM (Plant Maintenance)  
**Equipo auditor:** Juan Pablo Marin Espinosa, Diego Henao Camayo  

---

## 1. Identificación y Resumen Ejecutivo

- **Naturaleza de los datos:** proceso operativo real de mantenimiento de telecabinas. La prueba principal fue un **rastreo documental, físico y en sistema** de un caso real: Orden de trabajo `31531687` (mantenimiento preventivo semestral, cabina 02, pinza `07-468`), contrastando los seriales grabados en las piezas, la planilla de taller y lo registrado en SAP PM (equipo, clase, orden, notificación y puntos de medida). No se exponen datos personales ni credenciales; el nombre del técnico fue ocultado en la evidencia.
- **Riesgo principal evaluado:** **R-01 — Pérdida de trazabilidad de componentes críticos (mordazas y ejes)** (nivel en matriz: **20 — Crítico**).
- **Resultado principal:** el dato **existe en la pieza y en el papel**, y SAP PM **sí cuenta con estructura** para alojarlo (equipo `195185` con seis sub-equipos y la clase `PINZA_TELECABINAS`). Sin embargo, el proceso **no alimenta ni valida esa estructura al cerrar la orden**:
  1. La planilla registra **6 identificadores** (pinza `07-468`, mordaza fija, mordaza móvil, eje principal, eje bocado y eje J-H).
  2. Los sub-equipos de SAP están definidos **por posición de cabina**, no por serial físico, y los seriales se guardan como **texto libre** en la clase de la pinza.
  3. **3 de 5 componentes comparables** presentan valor distinto o ausente en SAP respecto de la planilla (eje bocado, eje principal, eje J-H); la mordaza móvil además aparece con el serial cortado.
  4. Las mediciones de desgaste se escribieron en el **texto explicativo** aunque existen **puntos de medida** para ellas.

---

## 2. Objetivo y Alcance

### Objetivo de auditoría
Verificar si la transcripción, notificación y cierre de una orden de mantenimiento de cabina en SAP PM garantiza la captura íntegra y estructurada de los seriales y mediciones críticos registrados en la planilla física de taller.

### Alcance funcional y temporal
- **Proceso:** componente físico (troquelado) → planilla física → registro y cierre en SAP PM.
- **Objeto auditado:** SAP Logon (Módulo PM), orden `31531687` y equipo `195185` "Pinza Cabina 02 LJ".
- **Población y muestra:** estudio de caso (*walkthrough*) sobre **1 orden**, **3 registros fotográficos** de piezas, la **planilla** y **7 capturas de SAP PM**: jerarquía del equipo, estructura, clase, operaciones de la orden, notificación, texto explicativo y puntos de medida.
- **Exclusiones:** liquidación financiera (Módulo CO), seguridad de red de talleres (R-03) y tickets de Mesa de Servicio (R-05).

---

## 3. Criterios y Metodología

### Criterios de evaluación
1. **Integridad en la captura y procesamiento de datos (COBIT 2019 — DSS06.03):** los controles de aplicación deben asegurar que los datos críticos provenientes de documentos fuente sean completos, estructurados y validados antes del cierre.
2. **Trazabilidad unitaria de componentes críticos (documentos de referencia DR-04, DR-12 y DR-15):** cada recambio debe dejar en el sistema el serial **desinstalado** e **instalado** por pieza.

### Papel de trabajo (ejecución de la prueba)

| Elemento | Documentación ejecutada |
| :--- | :--- |
| **¿Qué control revisamos?** | Control de entrada y obligatoriedad de datos en SAP PM al notificar y cerrar la orden de cabina. Responsables: Gerencia de TI (administradores SAP) y Jefatura de Cables Aéreos. |
| **¿Qué debería ocurrir?** | Los seriales retirados e instalados de la planilla deben quedar registrados de forma estructurada y validada en SAP (sub-equipos / clase) y las mediciones deben registrarse en los puntos de medida. |
| **¿Cómo lo revisamos?** | 1. Inspección física de piezas. 2. Extracción de seriales de la planilla. 3. Revisión en SAP de la jerarquía del equipo, la clase `PINZA_TELECABINAS`, la orden, la notificación y los puntos de medida. 4. Comparación planilla vs. SAP. |
| **¿Qué encontramos?** | SAP tiene estructura, pero los valores de la clase difieren de la planilla, el recambio no se refleja en los sub-equipos y las mediciones quedaron en texto libre. |

---

## 4. Resultados y Hallazgos

### A. Evidencia física y documental

| Componente | Evidencia física | Planilla (`31531687`) |
| :--- | :--- | :--- |
| Mordaza fija | `PFP037159` / N° 02 (pintura reduce legibilidad) | `4800048536-20B N° 16` (desinstalado) |
| Mordaza móvil | `PFP037160` / N° 08 | `N° 02A 4800054335-10` (desinstalado) → `PFP037160-01` (instalado) |
| Eje principal | `PF8322425-06` | `579643 / 579346 425-6 M16` (desinstalado) |
| Eje bocado | `4500434379 50` | `4500849988-20-A` (desinstalado) |
| Eje J-H | — | `MI19A` (desinstalado) |
| Pinza | — | `07-468` (desinstalado) |


### B. Estructura en SAP PM

El equipo `195185` "Pinza Cabina 02 LJ" tiene seis sub-equipos (`293007`–`293012`: mordaza fija, mordaza móvil, eje bocado, eje principal, muelle derecho y muelle izquierdo), cada uno ligado a un material (`5004197`–`5004202`). Están definidos **por posición de cabina**, no por serial físico.


### C. Seriales en la clase `PINZA_TELECABINAS` frente a la planilla

| Componente | Planilla | Clase SAP | Lectura |
| :--- | :--- | :--- | :--- |
| Mordaza fija | `4800048536-20B N° 16` | `N16 4800048536-20-B` | Coincide con el serial retirado |
| Mordaza móvil | `N°02A …4335-10` → `PFP037160-01` | `N02A 4800054335-10 PFP037160-0` | Mezcla serial viejo y nuevo; corta el sufijo `-01` |
| Eje principal | `579643 / 579346 425-6 M16` | `425 G M16 4800054335-10` | No coincide |
| Eje bocado | `4500849988-20-A` | `4500349988-20-A` | Difiere un dígito (8 vs. 3) |
| Eje J-H | `MI19A` | Sin característica | No existe en SAP |

Además, la clase **no tiene característica para el N° de pinza (`07-468`)**.


### D. Orden, notificación y mediciones

La orden `31531687` ("LJ Cab 02 Mtto preventivo semestral S47") incluye la operación `0700 — Controlar desgaste de la pinza`. La notificación `17129571` se registró el 19.11.2026 (06:50–07:00, 10 min, confirmación final). El texto breve se limita a 40 caracteres y la casilla *Existe txt.expl.* indica que el detalle está en un texto largo, que contiene la **puesta a tope D4 = 6,7 mm** y los **desgastes 1 a 6 entre 43,0 y 43,5 mm**. La pinza ya tiene puntos de medida: ciclaje (`101705`), desgaste 1–6 (`121785`–`121790`) y puesta a tope D4 (`121791`).


---

### Hallazgo H-01 (Riesgo R-01): seriales sin trazabilidad estructurada ni validada

- **Criterio:** COBIT 2019 (DSS06.03) y DR-04, DR-12 y DR-15: serial instalado y retirado, íntegro y validado al cerrar la orden.
- **Condición:** SAP modela la pinza y sus sub-equipos por posición y guarda los seriales como texto libre en la clase. El cierre de la orden no exige ni actualiza el serial. En la orden revisada, 3 de 5 componentes comparables difieren o no existen en SAP; la clase no tiene campo para el N° de pinza ni para el eje J-H.
- **Causa:** preliminar — no se utiliza el montaje/desmontaje de equipos en el recambio ni hay validación en el cierre. **Pendiente de confirmar** en `IW33`/`SPRO`.
- **Efecto o riesgo:** no es posible saber con certeza qué serial lleva cada cabina ni acumular su uso para prevenir fatiga mecánica.
- **Recomendación:** registrar el serial por sub-equipo, ejecutar montaje/desmontaje en cada recambio (p. ej. `IE4N`, a validar con el líder funcional) y bloquear el cierre sin serial.

### Hallazgo H-02 (Riesgos R-02 y R-04): doble digitación sin validación

- **Criterio:** los controles de entrada (COBIT 2019 — DSS06.03) deben prevenir errores de transcripción y exigir supervisión antes del cierre.
- **Condición:** transcripción manual papel → SAP sin validaciones ni revisión del supervisor. Evidencia: eje bocado `4500849988` (papel) vs. `4500349988` (SAP) y desfases de fechas de 1 mes o 1 año en el extracto de confirmaciones.
- **Causa:** proceso de doble digitación sin captura digital en el punto de trabajo ni reglas al guardar.
- **Efecto o riesgo:** pérdida de integridad del historial técnico y distorsión de los tiempos de mantenimiento.
- **Recomendación:** validaciones automáticas en la confirmación (`IW41`), aprobación del supervisor antes del cierre y evaluación de captura digital en taller.

### Hallazgo H-03 (asociado a R-01): mediciones críticas de desgaste en texto libre

- **Criterio:** los datos de condición deben quedar en campos estructurados que permitan análisis y alertas.
- **Condición:** existen puntos de medida `121785`–`121791` para desgaste 1–6 y puesta a tope D4, pero los valores se escribieron en el texto explicativo de la confirmación.
- **Causa:** el flujo de confirmación no obliga a registrar documentos de medición. **Pendiente de verificar** en documentos de medición (`IK18`) si se registró alguna lectura.
- **Efecto o riesgo:** sin histórico analizable, tendencias ni límites de alerta para anticipar la fatiga de las mordazas.
- **Recomendación:** registrar las mediciones como documentos de medición con límites superior e inferior y alerta.

---

## 5. Conclusión y Limitaciones

### Conclusión
SAP PM **tiene dónde guardar** los seriales y mediciones críticas (equipos, clase y puntos de medida), pero el proceso **no los captura de forma confiable**: el recambio no se refleja en los sub-equipos, la clase presenta diferencias con la planilla y las mediciones se escriben en texto libre. El control de trazabilidad es **inefectivo** y materializa el riesgo crítico **R-01**.

### Limitaciones
1. **Alcance del caso:** una orden, tres fotos y siete capturas; no permite cuantificar todas las órdenes históricas ni afirmar que exista una pieza vencida operando.
2. **Parametrización pendiente:** falta confirmar en `IW33`/`SPRO`, revisar documentos de medición (`IK18`) y los documentos `DR-04`, `DR-12` y `DR-15`.
3. **Valores de planilla:** transcritos desde imagen; requieren contraste con el original.
4. **Extracto de confirmaciones:** el truncamiento observado (p. ej. confirmación `15457307`, `CAB#13`) pudo corresponder solo al texto breve, que no incluye el texto explicativo.
5. **Legibilidad física:** la pintura reduce la visibilidad del troquelado en la mordaza fija (`PFP037159`).

---

## 6. Plan de Acción y Anexos

| ID | Acción propuesta | Responsable | Prioridad | Plazo | Evidencia de cierre |
| :--- | :--- | :--- | :---: | :---: | :--- |
| H-01 | Registrar serial por sub-equipo y usar montaje/desmontaje (p. ej. `IE4N`) en cada recambio; bloquear el cierre sin serial. | Líder funcional SAP PM y Jefe de Mantenimiento Cables Aéreos | Alta | 30 días | Parametrización y prueba de bloqueo. |
| H-01 | Conciliar la clase `PINZA_TELECABINAS` con las planillas; añadir N° de pinza y eje J-H; ampliar la longitud de los campos. | Líder funcional SAP PM y Supervisor | Alta | 30 días | Clase corregida y acta de conciliación. |
| H-01 | Limpiar el troquelado antes de leer y prohibir seriales en el texto breve. | Supervisor de Mantenimiento | Alta | 15 días | Instructivo y 5 órdenes con seriales completos. |
| H-03 | Registrar desgaste 1–6 y puesta a tope D4 como documentos de medición con límites de alerta. | Líder funcional SAP PM y Jefe de Cables | Media | 45 días | Mediciones graficables en SAP. |
| H-02 | Validaciones al guardar la confirmación (`IW41`) y revisión del supervisor antes del cierre. | Líder funcional SAP PM y Supervisor | Media | 45 días | Regla activa y órdenes sin desfases. |

### Anexos y trazabilidad del repositorio
- **Plan de auditoría (R-01):** [`Auditoria R-01.md`](./Auditoria%20R-01.md)
- **Matriz de riesgos priorizada:** [`Matriz_Riesgos_Metro.md`](./Matriz_Riesgos_Metro.md)
- **Programa de trabajo:** [`Programa_Auditoria_R01.xlsx`](./Programa_Auditoria_R01.xlsx)
- **Presentación:** [`Auditoria_Sap_Final_v2.pptx`](./Auditoria_Sap_Final_v2.pptx)
