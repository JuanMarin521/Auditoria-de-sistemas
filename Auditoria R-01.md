# Taller: Del riesgo priorizado al plan de auditoría 
**Integrantes:** Juan Pablo Marin, Diego Henao Camayo  
**Caso:** Empresa de Transporte Masivo del Valle de Aburrá (Metro de Medellín) - Área de Cables Aéreos  

---

## 1. Contexto y Selección del Riesgo
El Área de Cables Aéreos del Metro de Medellín gestiona el mantenimiento de las telecabinas utilizando el módulo SAP PM. La preocupación central radica en la pérdida de trazabilidad de los componentes críticos (como las mordazas o pinzas), debido a que las órdenes de mantenimiento se transcriben de plantillas de papel al sistema sin controles que obliguen a registrar el identificador único de la pieza instalada o retirada.

**Riesgo Seleccionado (ID Matriz: R-01)**
*   **Activo o proceso afectado:** Proceso de notificación de mantenimiento y base de datos del módulo SAP PM.
*   **Amenaza:** Ingreso de información incompleta (omisión de seriales) por parte del personal durante la transcripción manual de papel al SAP.
*   **Vulnerabilidad:** Ausencia de controles de validación a nivel de aplicación en SAP PM (falta de obligatoriedad en el campo de *Equipo/Serial*).
*   **Consecuencia posible:** Componente crítico sin trazabilidad de serial. Esto deriva en la incapacidad de rastrear la vida útil o fatiga de una mordaza, exponiendo al sistema a fallas, perdida de trazabilidad y sanciones regulatorias.
*   **Nivel en Matriz:** Riesgo Crítico (Probabilidad: 4 / Impacto: 5 / Nivel: 20).
*   **Control propuesto:** Parametrización del campo *Equipo/Serial* como obligatorio (*mandatory*) en SAP PM para todas las clases de órdenes de mantenimiento de Cables Aéreos.
*   **Razón de auditoría:** Auditar este riesgo es prioritario porque impacta directamente la seguridad humana operacional. Confirmar la ausencia de este control técnico permite dimensionar la vulnerabilidad en el sistema y es motivo justificable del cambio inmediato en la parametrización de SAP.

---

## 2. Objetivo de Auditoría
Determinar si el proceso de notificación y cierre de órdenes de trabajo en el módulo SAP PM del área de Cables Aéreos garantiza el registro obligatorio y la trazabilidad del serial único de los componentes críticos (mordazas/pinzas), conforme a los documentos de referencia de mantenimiento.

---

## 3. Alcance Funcional
1.  **Proceso o función:** Notificación de la orden de trabajo y validación de campos de trazabilidad (ingreso de seriales).
2.  **Sistema o componente:** Aplicación cliente SAP Logon (módulo PM).
3.  **Población:** Totalidad de las órdenes de trabajo liquidadas para componentes críticos de Cables Aéreos.
4.  **Periodo:** Últimos 6 meses (Mayo - Octubre).
5.  **Áreas o responsables:** Gerencia de TI (Administradores SAP) y Jefes de Cables Aéreos.
6.  **Inclusiones y exclusiones:** Se excluye la auditoría de la liquidación de costos de mantenimiento (Módulo CO) y la revisión de tiempos de trabajo (R-04). *Razón:* Para garantizar la viabilidad del plan, el alcance se restringe exclusivamente al riesgo R-01 (trazabilidad física y seriales). Excluir lo financiero y los tiempos no limita la conclusión sobre la seguridad de las piezas críticas.

---

## 4. Criterios de Evaluación
1.  **Criterio 1 (Validación de Entrada en Sistema):** El sistema SAP PM debe requerir de manera obligatoria el registro de un "Equipo/Serial" válido para permitir el guardado y cierre de la orden de trabajo en componentes críticos.
    *   *Fuente:* Lineamientos de control de procesos de negocio de COBIT 2019-DSS06.03 (Gestionar los controles de los procesos de negocio) se enfoca en mantener la integridad de la información y la seguridad de los activos de información manejados dentro de los procesos de negocio de la empresa.
2.  **Criterio 2 (Registro Documental):** Todo cambio o mantenimiento de un componente crítico de telecabinas debe dejar registro del identificador único (serial) de la pieza instalada y/o retirada.
    *   *Fuente:* Documentos de Referencia de Mantenimiento del Metro de Medellín (DR-04, DR-12 y DR-15).

---

## 5. Programa de Trabajo y Trazabilidad

| ID del riesgo | Control a evaluar | Criterio y fuente | Procedimiento | Población o muestra | Evidencia esperada |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **R-01** | **Configuración de obligatoriedad de campos en SAP PM** (Diseño del control) | El campo *Serial* debe ser obligatorio en el sistema (Fuente: COBIT DSS06 / Políticas Cables Aéreos). | **Revisión interna:** Ingresar al entorno de producción SAP (transacción IW33) y revisar la parametrización de campos para las clases de órdenes de cables. Excepción buscada: El campo "Equipo/Serial" figura como "Opcional". | 100% de las clases de órdenes parametrizadas para Cables Aéreos en el ambiente de mantenimiento en base del periodo establecido. | Capturas de pantalla de la parametrización de los campos en SAP PM. |
| **R-01** | **Integridad de los datos de seriales en órdenes cerradas** (Operación del control) | Las órdenes cerradas deben poseer la trazabilidad del componente segun los DR 04, DR 12, DR 15. | **Análisis de datos:** Filtrar en SAP órdenes liquidadas. Aplicar filtros para contabilizar valores nulos (vacíos) o textos genéricos en los campos de "Texto confirmación" donde debería constar el serial. | Filtros aplicados en SAP según fecha, orden o periodo contable . | Exporte de un informe indicando la cantidad/porcentaje de filas con excepciones (campos de trazabilidad vacíos). |

* 📊 **[Ver Programa de Auditoría (Matriz de Procedimientos en Excel)](./Programa_Auditoria_R01.xlsx)** 

---

## 6. Organización de la Ejecución (Cronograma y Roles)
Dada la naturaleza del equipo auditor, donde un integrante es empleado activo del Metro de Medellín con acceso a la red y sistemas corporativos, la ejecución se divide así:
*   **Fase 1: Preparación (Semana 1):** Recopilación de los Documentos de Referencia DR 04, 12, 15 y exporte de la base de datos de SAP. *Responsable: Juan Pablo Marin.*
*   **Fase 2: Revisión de Diseño (Semana 2):** Navegación en el cliente SAP Logon corporativo para verificar la parametrización real de las clases de órdenes. *Responsable: Juan Pablo Marin.*
*   **Fase 3: Revisión de Operación (Semana 3):** Análisis de la muestra de datos en Excel de forma externa, buscando campos de seriales omitidos o vacíos. *Responsable: Diego Henao Camayo.*
*   **Fase 4: Cierre (Semana 4):** Elaboración conjunta del informe de auditoría. *Recursos necesarios:* Equipo de trabajo corporativo , Python/Excel para análisis de datos.

---

## 7. Supuestos y Limitaciones
*   **Hecho comprobado:** Se evidenció que el sistema permite cierres de órdenes con campos descriptivos totalmente vacíos (nulos), materializando el riesgo R-01.
*   **Supuesto:** Se asume que en el entorno físico, los técnicos sí están anotando los seriales correctamente en las plantillas de papel y que las placas de los seriales en las pinzas son legibles, por lo que la pérdida de la información ocurre estrictamente en el proceso de transcripción o por la falta de obligatoriedad en SAP.
*   **Información por solicitar:** Verificación de si los componentes críticos (como las mordazas) están creados correctamente en la base de datos maestra de SAP como "Equipos" individualizados (que soportan número de serie) y no como repuestos de inventario genérico.
*   **Propuesta del equipo:** La parametrización estricta (*mandatory*) del campo de serial es un control propuesto por el equipo auditor para corregir la vulnerabilidad de diseño actual en la interfaz de usuario.
*   **Limitaciones:** El alcance de esta auditoría es de carácter lógico y transaccional en el sistema SAP. No se realizarán inspecciones físicas en terreno para corroborar si el serial registrado ocasionalmente en el sistema coincide con la pieza física real instalada en la telecabina.

---
**Anexo:** 
*   **Matriz de Riesgos Completa:** [Repositorio GitHub - Matriz de Riesgos Metro](https://github.com/JuanMarin521/Auditoria-de-sistemas/blob/main/Matriz_Riesgos_Metro.md)
