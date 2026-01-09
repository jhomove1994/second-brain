---
id: Fundamentos Programacion Batch
aliases:
  -  #fundamentos-programacion-batch
tags:
  -  #cobol
  -  #sentencias
  -  #fundamentos-programacion-batch
---

[[100-projects/Dominar Cobol]]

- Hay dos tipos de procesamiento en los main frames el procesamiento batch y el [[300-resources/Cobol/ProgramacionOnline/Fundamentos]].
- El procesamiento batch se usa principalmente para procesar grandes volúmenes de datos en una sola ejecución con el fin de obtener los siguientes beneficios:

  - Ejecutar el procesamiento cuando los recursos de computación estan menos ocupados.
  - Evitar que los recursos de los sistemas estén desaprovechados por intervenciones manuales ya que estos procesos corren sin supervisión del usuario.
  - Permitir al sistema usar diferentes prioridades para que se ejecuten los procesos en un determinado orden.
  - Correr los programas solamente una vez con muchas transacciones o funcionalidades.
  - En el procesamiento batch se saca provecho de la velocidad relativa de acceso a los registros secuencialmente dado que los registros son leídos y escritos uno a continuación del otro.

- Para ejecutar los procesos batch se utilizan una seria de sentencias para controlar el proceso denominados [[300-resources/JCL/jcl]] (Job Control Lenguage). El cual especifica el nombre del job, cada paso se identifica por un nombre que especifica el programa a ser ejecutado, el paso también especifica los archivos usados por el programa con los atributos de cada uno como nombre, unidad donde reside, tipo de unidad, longitud del registro y ablocamiento.

- El job finaliza cuando todos los pasos se han ejecutado.

- Algunas veces cuando un programa encuentra una condición de error el job finaliza con una terminación anormal (**ABEND**). Hay dos clases de **ABEND**:
  - Los generados por el sistema operacional (por ejemplo, intentar leer un archivo que no existe).
  - Los codificados dentro de los programas, todos los programas finalizan con un código de retorno que indica el resultado del procesamiento del paso cuando la terminación es normal el código de retorno es 0.
