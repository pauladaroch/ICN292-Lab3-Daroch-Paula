# ICN292 - Lab 3: Triage de devoluciones con n8n

**Nombre:** Paula Daroch 
**RUT (sin DV):** 21326225 
**Fecha:** 14 de septiembre de 2026

En este repositorio están disponibles los workflows que se diseñaron para el Laboratorio 3 de ICN292, para automatizar el proceso de clasificación para solicitudes de devolución dentro de n8n.

## Workflows incluidos

- ICN292-Lab3-Daroch-Paula-triage: Recibe cada entrada vía Webhook, evalúa las reglas de clasificación fijadas, obtiene el valor de la UF, registra el resultado en una Data Table, notifica al cliente mediante Gmail y retorna la respuesta al emisor.

- ICN292-Lab3-Daroch-Paula-emisor: Permite despachar solicitudes de prueba dirigidas al Webhook del flujo de triage.

- ICN292-Lab3-Daroch-Paula-resumen: Genera un informe con las solicitudes procesadas a lo largo de la jornada y lo envía a través de Gmail.

- ICN292-Lab3-Daroch-Paula-error: Flujo secundario pensado para tomar las fallas del sistema y mandar un aviso por correo electrónico.

## Parámetros utilizados

- Umbral de monto: U = $55.000

- Plazo máximo: D = 14 días

## Reglas de clasificación

El criterio para ordenar las solicitudes opera de la siguiente forma:

- DATOS_INVALIDOS: Si falta el ID asociado a la solicitud o si el monto queda registrado en cero o un valor negativo.

- RECHAZO: Si el pedido pasa de los 14 días contados desde la compra o bien la mercadería llega marcada como `danado_por_uso`.

- REVISION: En caso de que el valor supere la barrera de $55.000 o si el ítem figura con la etiqueta `con_fallas`.

- APROBACION: Aplica a las situaciones restantes donde no calza ninguna de las condiciones anteriores.

## Resumen diario

El flujo encargado del resumen revisa las solicitudes almacenadas en el día para obtener el conteo y monto acumulado por tipo de ruta, la suma global de solicitudes recibidas, la cifra total correspondiente al monto procesado y el porcentaje final de la tasa de aprobación automática.

La corrida de este reporte quedó fijada para realizarse de forma automática a las 20:00 horas.

## Consideraciones de seguridad y configuración

Las versiones JSON cargadas son versiones sanitizadas derivadas de los workflows empleados a lo largo del desarrollo.

Antes de subir los archivos, se modificaron directamente ciertos parámetros pertenecientes al entorno de trabajo. Se eliminaron las referencias a credenciales de Gmail en los workflows que las utilizan, se reemplazaron las direcciones de correo personales por valores genéricos cuando correspondía y se cambió la URL real del Webhook en el workflow Emisor por una URL genérica.

Tales ajustes responden al resguardo de los datos personales y de la cuenta de n8n, sin alterar de modo alguno el comportamiento ni la lógica establecida.

Tanto las pruebas como las evidencias del informe se obtuvieron operando con los flujos de trabajo funcionales originales en n8n.

## Configuración necesaria al importar

Al momento de cargar estas rutinas en una plataforma de n8n distinta, se debe ingresar credenciales operativas de Gmail, modificar el texto `correo_destino@example.com` por una dirección de correo válida, ajustar la URL que le corresponde al Webhook dentro del workflow Emisor, generar la Data Table donde se van a registrar las devoluciones y vincular de nuevo el Error Workflow con la rutina de Triage cuando sea requerido.

## Cómo abrir o reproducir los archivos

Los archivos `.json` corresponden a workflows exportados desde n8n. Para utilizarlos, primero se deben importar dentro de una instancia de n8n mediante la opción de importar workflow desde archivo.

- **ICN292-Lab3-Daroch-Paula-triage.json:** Después de importarlo, se debe asociar la Data Table correspondiente, configurar una credencial de Gmail para el nodo Notificar Cliente y vincular el Error Workflow. Luego se puede activar el workflow para utilizar su Webhook.

- **ICN292-Lab3-Daroch-Paula-emisor.json:** Después de importarlo, se debe reemplazar la URL genérica por la URL de producción del Webhook generado por el workflow de Triage. Su ejecución se realiza manualmente desde n8n.

- **ICN292-Lab3-Daroch-Paula-resumen.json:** Al importarlo se debe seleccionar la Data Table utilizada y configurar las credenciales de Gmail junto con una dirección de correo válida. Después se puede activar para que el Schedule Trigger ejecute el resumen automáticamente.

- **ICN292-Lab3-Daroch-Paula-error.json:** Se debe importar en n8n, configurar las credenciales de Gmail y luego asociarlo como Error Workflow dentro de la configuración del workflow de Triage.

## Archivos

Los archivos quedaron guardados en formato JSON, por lo que se pueden importar hacia n8n una vez completados los pasos de configuración señalados en el apartado anterior.
