---
id: CallYExit
aliases:
  -  #CallYExit
  -  #CALLYEXIT
  -  #CALL
  -  #EXIT
tags:
  -  #call
  -  #CALL
  -  #EXIT
  -  #resources
  -  #exit
  -  #cobol
---

# CALL Y EXIT

Se utilizan cuando se hace un llamado a otro programa para ejecución. El programa que llama ejecuta la sentencia call para darle control de ejecución al programa que es llamado y la sentencia exit es ejecutada en el programa llamado para retornarle el control al programa llamador.

CALL programa, nombre - programa USING area

- **PROGRAMA:** Es el nombre del programa llamado a ejecución, este tipo de llamado se conoce como llamado estático, para ejecutar este tipo de sentencia el programa llamado debe ser compilado previamente. Y cada vez que se modififque dicho programa se debe recompilar el programa llamador dado que el compilador genera un solo modulo de carga con el programa llamado incluido dentro del programa llamador.

- **NOMBRE PROGRAMA:** Es una variable definida en el programa llamador que contiene el nombre del programa a ser llamado, este tipo de llamado se conoce como **call dinámico** si el programa llamado se codifica no es necesario recompilar el programa llamador porque las intrucciones del programa llamado están en un modulo de carga diferente al del programa llamador y se integran solamente en el momento de la ejecución.

- **ÁREA:** Son las variables que se definen en la **linkage-section** del programa llamado para recibir datos del programa llamador que están definidos en la **working storage**

- **PROGRAMA LLAMADOR:** El que ejecuta la sentencia CALL.
- **PROGRAMA LLAMADO:** El que recibe el control, ejecuta su lógica y retorna el control.
- **USING:** Es la cláusula para pasar "argumentos" o variables entre ambos.

```cobol
IDENTIFICATION DIVISION.
PROGRAM-ID. PROG01.

DATA-DIVISION.
WORKING-STORAGE SECTION.
01 MONTO        PIC 9(5)V99 VALUE 100.00.
01 RESULTADO    PIC 9(5)V99.

PROCEDURE DIVISION.
    DISPLAY "PROG01: LLAMANDO AL CALCULADOR DE IMPUESTOS".

    *> Llamamos al subprograma pasando las variables
    CALL "PROG02" USING MONTO, RESULTADO.

    DISPLAY "PROG01: EL RESULTADO DEVUELTO ES: " RESULTADO.
    STOP RUN.
```

SUBPROGRAMA

```cobol
IDENTIFICATION DIVISION.
PROGRAM-ID. PRO602.

LINKAGE SECTION. *> IMPORTANTE: Aquí se definen los datos que vienen de fuera
01 LNK-MONTO      PIC 9(5)V99.
01 LNK-RESULTADO  PIC 9(5)V99.

PROCEDURE DIVISION USING LNK-MONTO LNK-RESULTADO.

  *> Calculamos un impuesto del 15%
  COMPUTE LNK-RESULTADO = LNK-MONTO * 1.15.

  *> En lugar de STOP RUN,  usamos EXIT PROGRAM
  EXIT PROGRAM.

```

# EXIT vs EXIT PROGRAM vs STOP RUN

Es común confundirlos, pero en #cobol cada uno tiene un "alcance" distinto:

- **EXIT:** Es solo un punto de referencia. No hace nada por si solo; se usa para marcar el final de un **PERFORM** ... **THRU**
- EXIT PROGRAM: Termina la ejecución del subprograma y **devuelve el control** al programa que lo llamó (al **CALL** original). Se usa en los programas "hijos".
- **STOP RUN:** Mata todos los procesos activos y devuelve el control al Sistema Operativo (o al CICS/JCL). Se usa solo en el programa "padre".

# CALL STATICO

El nombre del subprograma es un un literal (un texto fijo entre comillas). Durante la compilación, el sistema "pega" ambos códigos en un solo archivo ejecutable.

Se usa principalmente para rutinas matemáticas o de validación que nunca cambian (ej. calcular un dígito verificador o un IVA).

```cobol
IDENTIRIFICATION DIVISION.
PROGRAM-ID. PADRE-ESTATICO.

PROCEDURE DIVISION.
  DISPLAY "SOY EL PADRE Y LLAMO A MI HIJO DE FORMA ESTÁTICA".

  *> Sintaxis: Literal entre comillas
  CALL "HIJO-ESTATICO"

  DISPLAY "EL HIJO TERMINO Y VOLVI AL PADRE".
  STOP RUN.
```

- **En el Mainframe:** Para que esto funcione, en el paso de "Link-edit" del **JCL** se debe incluir ambos módulos. Si el HIJO-ESTATICO no existe en ese momento, la compilación falla.

# CALL DINAMICO

El nombre del programa está guardado en una variable. El programa no sabe a quién va a llamar hasta que llega a esa línea de código en tiempo de ejecución.

Se usa principalmente para lógica de negocio, (ej: "Consultar Saldo", "Transferir"). Esto permite que el equipo de **Mainframe** pueda arreglar un error en la lógica de transferencias y subir el parche.

```cobol
IDENTIFICATION DIVISION.
PROGRAM-ID. PADRE-DINAMICO.

DATA DIVISION.
WORKING-STORAGE SECTION.
*> Definimos una variable para guardar el nombre del programa
01 PROGRAMA-A-LLAMAR    PIC  X(08)  VALUE SPACES.

PROCEDURE DIVISION.

 DISPLAY "SOY EL PADRE Y BUSCARE AL HIJO EN EL DISCO...".

 *> Podemos cambiar el nombre del prgrama según una condición
 MOVE "HIJO-DINAMICO" TO PROGRAMA-A-LLAMAR.

 *> Sintaxis: Se usa el nombre de la VARIABLE, sin comillas
 CALL PROGRAMA-A-LLAMAR
   ON EXCEPTION
     DISPLAY "ERROR: NO SE ENCONTRO EL PROGRAMA EN LA LIBRERIA".
 END-CALL.

 DISPLAY "HIJO DINAMICO EJECUTADO CON EXITO".
 STOP RUN.
```

- **En el Mainframe:** El programa PADRE se compila solo. Cuando el servidor hace la petición, el Mainframe busca el archivo **"HIJO-DINAMICO"** en las librerias cargadas (STEPLIB).
