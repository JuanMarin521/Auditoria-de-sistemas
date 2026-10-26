# Taller: Del riesgo priorizado al plan de auditoría
**Curso:** Auditoría de Sistemas (ET0114)  
**Integrantes:** Juan Pablo Marin, Diego Henao Camayo  
**Caso:** Empresa de Transporte Masivo del Valle de Aburrá (Metro de Medellín) - Área de Cables Aéreos  

---

## 1. Contexto y Selección del Riesgo
El Área de Cables Aéreos del Metro de Medellín gestiona el mantenimiento de las telecabinas utilizando el módulo SAP PM. La preocupación central radica en la pérdida de trazabilidad de los componentes críticos (como las mordazas o pinzas), debido a que las órdenes de mantenimiento se transcriben de plantillas de papel al sistema sin controles que obliguen a registrar el identificador único de la pieza instalada o retirada.

**Riesgo Seleccionado (ID Matriz: R-01)**
*   **Activo o proceso afectado:** Proceso de notificación de mantenimiento y base de datos del módulo SAP PM.
*   **Amenaza:** Ingreso de información incompleta (omisión de seriales) por parte del personal durante la transcripción manual de papel al ERP.
*   **Vulnerabilidad:** Ausencia de controles de validación a nivel de aplicación en SAP PM (falta de obligatoriedad en el campo de *Equipo/Serial*).
*   **Consecuencia posible:** Componente crítico sin trazabilidad de serial. Esto deriva en la incapacidad de rastrear la vida útil o fatiga de una mordaza, exponiendo al sistema a fallas catastróficas (riesgo de *safety*), pérdida de vidas y sanciones regulatorias.
*   **Nivel en Matriz:** Riesgo Crítico (Probabilidad: 4 / Impacto: 5 / Nivel: 20). Es el riesgo prioritario de la matriz.
*   **Control propuesto:** Parametrización del campo *Equipo/Serial* como obligatorio (*mandatory*) en SAP PM para todas las clases de órdenes de mantenimiento de Cables Aéreos.
*   **Razón de auditoría:** Auditar este riesgo es vital porque impacta directamente la seguridad humana operacional. Confirmar la ausencia de este control técnico permite dimensionar la vulnerabilidad en el sistema central del negocio y justificar el cambio inmediato en la parametrización de SAP.

---

## 2. Objetivo de Auditoría
Determinar si el proceso de notificación y cierre de órdenes de trabajo en el módulo SAP PM del área de Cables Aéreos garantiza el registro obligatorio y la trazabilidad del serial único de los componentes críticos (mordazas/pinzas), conforme a los documentos de referencia de mantenimiento.

---

## 3. Alcance Funcional
1.  **Proceso o función:** Notificación de la orden de trabajo y validación de campos de trazabilidad (ingreso de seriales).
2.  **Sistema o componente:** Aplicación cliente SAP Logon (módulo PM), específicamente las clases de actividad de cables (ej. A45MO, P28TO).
3.  **Población:** Totalidad de las órdenes de trabajo liquidadas para componentes críticos de Cables Aéreos.
4.  **Periodo:** Últimos 6 meses (Mayo 2026 - Octubre 2026), un periodo razonable para obtener una muestra representativa de la operación actual.
5.  **Áreas o responsables:** Gerencia de TI (Administradores SAP) y Jefatura de Cables Aéreos.
6.  **Inclusiones y exclusiones:** Se excluye la auditoría de la liquidación de costos de mantenimiento (Módulo CO) y la revisión de tiempos de trabajo (R-04). *Razón:* Para garantizar la viabilidad del plan, el alcance se restringe exclusivamente al riesgo R-01 (trazabilidad física y seriales). Excluir lo financiero y los tiempos no limita la conclusión sobre la seguridad de las piezas críticas.

---

## 4. Criterios de Evaluación
1.  **Criterio 1 (Validación de Entrada en Sistema):** El sistema SAP PM debe requerir de manera obligatoria el registro de un "Equipo/Serial" válido para permitir el guardado y cierre de la orden de trabajo en componentes críticos.
    *   *Fuente:* Lineamientos de control de procesos de negocio de COBIT 2019 (DSS06.03).
2.  **Criterio 2 (Registro Documental):** Todo cambio o mantenimiento de un componente crítico de telecabinas debe dejar registro del identificador único (serial) de la pieza instalada y/o retirada.
    *   *Fuente:* Documentos de Referencia de Mantenimiento del Metro de Medellín (DR 04, DR 12 y DR 15).

---

## 5. Programa de Trabajo y Trazabilidad

| ID del riesgo | Control a evaluar | Criterio y fuente | Procedimiento | Población o muestra | Evidencia esperada |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **R-01** | **Configuración de obligatoriedad de campos en SAP PM** (Diseño del control) | El campo *Serial* debe ser obligatorio en el sistema (Fuente: COBIT DSS06 / Políticas TI). | **Revisión interna:** Ingresar al entorno de producción SAP (transacción SPRO) y revisar la parametrización de campos para las clases de órdenes de cables. Excepción buscada: El campo "Equipo/Serial" figura como "Opcional". | 100% de las clases de órdenes parametrizadas para Cables Aéreos en el ambiente de producción. | Capturas de pantalla de la parametrización de los campos en SAP PM. |
| **R-01** | **Integridad de los datos de seriales en órdenes cerradas** (Operación del control) | Las órdenes cerradas deben poseer la trazabilidad del componente (Fuente: DR 04, DR 12, DR 15). | **Análisis de datos:** Exportar un reporte `.xlsx` de órdenes liquidadas. Aplicar filtros para contabilizar valores nulos (vacíos) o textos genéricos en los campos de "Suboperación" o "Texto confirmación" donde debería constar el serial. | Muestra extraída de 55 órdenes de trabajo recientes (ej. archivo *ordenes SAP metro.xlsx*). | Exporte de la base de datos de SAP indicando la cantidad/porcentaje de filas con excepciones (campos de trazabilidad vacíos). |

---

## 6. Organización de la Ejecución (Cronograma y Roles)
Dada la naturaleza del equipo auditor, donde un integrante es empleado activo del Metro de Medellín con acceso a la red y sistemas corporativos, la ejecución se divide así:
*   **Fase 1: Preparación (Semana 1):** Recopilación de los Documentos de Referencia (DR 04, 12, 15) y exporte de la base de datos de SAP. *Responsable: Juan Pablo Marin (acceso interno).*
*   **Fase 2: Revisión de Diseño (Semana 2):** Navegación en el cliente SAP Logon corporativo para verificar la parametrización real de las clases de órdenes. *Responsable: Juan Pablo Marin.*
*   **Fase 3: Revisión de Operación (Semana 3):** Análisis de la muestra de datos en Excel (`ordenes SAP metro.xlsx`) de forma externa, buscando campos de seriales omitidos o vacíos. *Responsable: Diego Henao Camayo (auditor externo).*
*   **Fase 4: Cierre (Semana 4):** Elaboración conjunta del informe de auditoría. *Recursos necesarios:* Equipo de cómputo corporativo (para Fase 1 y 2), Python/Excel para análisis de datos (Fase 3).

---

## 7. Supuestos y Limitaciones
*   **Hecho comprobado:** Tras una exploración preliminar de la muestra de datos (`ordenes SAP metro.xlsx`), se evidenció que SAP permite cierres con campos descriptivos totalmente vacíos (nulos), lo que materializa el riesgo R-01.
*   **Supuesto:** Se asume que la muestra de Excel exportada por el empleado interno refleja con exactitud la base de datos de producción y no presenta alteraciones en su estructura.
*   **Información por solicitar:** Verificación de si existe algún otro sistema paralelo (ej. SCADA) que lleve el control de los seriales fuera de SAP, aunque operativamente se sabe que la dependencia actual recae en el ERP.
*   **Propuesta del equipo:** La parametrización estricta (*mandatory*) del campo de serial es un control propuesto por el equipo auditor para corregir la vulnerabilidad de diseño actual.
*   **Limitaciones:** El auditor externo (Diego) no cuenta con credenciales VPN ni SAP Logon, por lo que su capacidad de auditar se limita estrictamente a la fiabilidad del exporte de datos (`.xlsx`) y capturas de pantalla suministradas por el integrante interno.

---
**Anexo:** 
*   **Matriz de Riesgos Completa:** [Repositorio GitHub - Matriz de Riesgos Metro](https://github.com/JuanMarin521/Auditoria-de-sistemas/blob/main/Matriz_Riesgos_Metro.md)
