---
id: Fundamentos Programacion Online
aliases:
  -  #fundamentos-programacion-online
tags:
  -  #cobol
  -  #fundamentos-programacion-online
---

[[100-projects/Dominar Cobol]]

- El procesamiento online a diferencia del [[300-resources/Cobol/ProgramacionBatch/Fundamentos]] se utiliza para procesar transacciones con poco volumen de información para obtener un resultado en tiempo real por ejemplo cuando un usuario se acerca a un cajero para solicitar un saldo.

- En el procesamiento online el acceso es al azar puesto que se requiere una respuesta inmediata y no seria eficiente tener que leer muchos registros para obtener la infomación que se esta solicitando.

- En el procesamiento online en el mainframe se utiliza el producto [[IBM CICS]], este producto es como un susbsistema operacional que controla todos los programas online, para que sea eficiente tiene la mas alta prioridad entre los procesos que se están ejecutando.

- El [[IBM CICS]] actúa como una interface entre el sistema operativo y los programas de aplicación online, los programa de aplicación online son disparados por el usuario entrando alguna información en los temrinales y necesitan ser diseñados para que respondan al usuario instántaneamente con los datos solicitados.

- El ciclo compuesto por la entrada de datos en la terminal, la ejecución del programa online y el retorno de los datos al usuario se llama una transacción.

- En la programación cobol online con [[IBM CICS]] las instrucciones de entrada/salida están bajo el control de **CICS** y no se codifican definiciones de archivos (ni select, ni FD) ni apertura y cierre de los mismos normalmente sino como instrucciones **CICS**, el compilador **COBOL** no contiene instrucciones **CICS**, por tal motivo los programas online deben pasar por un proceso de traducción de las instrucciones **CICS** a las intrucciones **COBOL** antes de pasar por el proceso de programación.

- Las instrucciones **CICS** se identifican porque comienzan con:

```cobol
  EXEC-CICS
  END-CICS.
```

- El área de comunicación que se especifica en los programas online **CICS** para llamados y paso de información entre programas tiene un nombre especifico **DFHCOMMAREA** y se codifica a nivel 01 en la [[300-resources/Cobol/Divisiones/data-division/linkage-section]].

- El llamado a otros programas no se realiza con la instrucción **CALL** sino con la sentencia **LINK** O **XCTL** y el retorno de los programas generalmente se hace con **GO BACK** a menos que no se quiera retornar a otro programa sino al **CICS** en cuyo caso se codifica la sentencia **EXEC CICS RETURN**

- En online la sentencia **START** se codifica como **START BROWSE** y se comporta de la misma manera que en el batch. En la instalación esta limitado a maximo **100** registros para hacer una lectura secuencial y se deben definir las condiciones de fin del start browse.
