Auditoría de Sistemas: Metro de Medellín - Área de Cables Aéreos

Juan Pablo Marin – Diego Henao

1.
En el metro de Medellín, específicamente en el área de cables aéreos, gestiona el mantenimiento de las telecabinas y procesos de mantenimiento mediante el ERP SAP, y la organización podría enfrentar riesgos operativos y regulatorios debido a la perdida de trazabilidad, ya que el sistema permite cerrar ordenes de componentes críticos o procedimientos sensibles, como el cerrar operaciones en ordenes de componentes como mordazas sin ingresar el serial único o con errores debido a la transcripción manual desde un papel.
Además, existe un riesgo adicional de ciberseguridad ya que se permite la conexión de computadores externos o personales a la red corporativa para acceder al aplicativo SAP, aunque este solicita credenciales.

2. Se aplicarían 3 marcos de referencia en el siguiente orden:
   
    -COBIT 2019 con enfoque en controles de procesos de negocio y calidad de datos.
    
    -ISO 27001 con enfoque en seguridad de la información y control de redes
    
    -ITIL 4 con enfoque en diseño del servicio y gestión de problemas

3.
Se inicia con COBIT 2019 se aplica principalmente por el riesgo mas inminente que es la falta de trazabilidad de los repuestos, rutinas o componentes críticos. Mediante el dominio DSS06 del COBIT que gestiona los controles de los procesos de negocio, permitiendo auditar directamente el problema que es la ausencia de controles a nivel de aplicación. Responde a la necesidad de averiguar por qué los implementadores SAP no han implementado campos obligatorios para los seriales y por qué no existe un flujo de trabajo que obligue a los supervisores (Gestores de calidad/auditores) de aprobar la orden antes de su cierre automático.

Se continua con ISO 27001 se eligió este marco para abordar el riesgo de ciberseguridad introducido por los computadores personales, aunque SAP Logon requiere de usuario y contraseña, un equipo personal infectado conectado a la red puede ingresar un ransomware o usar keyloggers para robar credenciales.

Para finalizar el ITIL 4, se aborda el factor humano y operativo, error en los tiempos y datos vacíos es un síntoma de mal diseño del servicio como la doble digitación que es del papel hacia el PC. ITIL 4 evaluará el diseño del servicio y la gestión del conocimiento, proponiendo buscar evoluciones del servicio como uso de tablets industriales.

4. Para la sustentación los hallazgos bajo cada marco se solicitará la siguiente evidencia:

COBIT:
  1. Demostración de que los parámetros de datos sensibles de piezas no están marcados como mandatory en los tipos de orden de mantenimiento preventivo y correctivo.
  2. Un muestreo de 50 ordenes de trabajo de telecabinas liquidadas en el ultimo año para verificar la tasa de campos vacíos o tiempos ilógicos cerrados automáticamente sin un workflow.
     
ISO:
  1.	El manual o normativa de TI respecto al uso de dispositivos personales en instalaciones operativas y tratar de conseguir evidencia de que existe una segmentación de red para aislar equipos invitados. Así también como solicitar el documento técnico o procedimiento para equipos de invitados a la red.
     
ITIL 4:
  1.	Documentación que muestre el procedimiento oficial que enseña a notificar y realizar gestiones en el SAP Logon.
  2.	Un reporte de errores en notificaciones o solicitudes de corrección de datos de mantenimiento.
