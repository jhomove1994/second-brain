---
id: SET
aliases:
  -  #SET #set
tags:
  -  #cobol
  -  #sentencias
  -  #set
---

# SET

La sentencia **SET** establece puntos de referencias para operaciones de manejos de tablas y switches.
**SET** se utiliza para manipular elementos de control: **Índices de tablas, Nombres de condición (88) y Direccionamiento de memoria (Punteros)**.

**Formato 1:** SET para índices (Tablas con INDEXED BY)

Cuando una tabla tiene un índice (en lugar de un subíndice numérico normal), no puedes usar **[[Move]]**. Debes usar **SET** porque los índices internamente no son números simples, sino desplazamientos de memoria (offsets).

```cobol
DATA DIVISION.
WORKING-STORAGE SECTION.
01 TABLA-MESES.
  05 MES-NOMBRE PIC X(10) OCCURS 12 TIMES INDEXED BY IND-MES.

PROCEDURE DIVISION.
  *> Formato A: Asignación directa.
  SET IND-MES TO 1.

  *> Formato B: Incremento/Decremento (Muy útil en bucles)
  SET IND-MES UP BY 1. *> Suma 1 al índice.
  SET IND-MES DOWN BY 1. *> Resta 1 al índice.
```

**Formato 2:** SET para Nombres de Condición (88)

Este es el formato más elegante. En lugar de mover un valor a una variable para cambiar su estado, "activas" la condición.

```cobol
DATA DIVISION.
WORKING-STORAGE SECTION.
01 ESTADO-TX      PIC X(01).
  88 TX-EXITOSA   VALUE "S".
  88 TX-FALLIDA   VALUE "N".
  88 TX-PENDIENTE VALUE "P".

PROCEDURE DIVISION.
  *> En lugar de: MOVE  "S" TO ESTADO-TX.
  SET TX-EXITOSA TO TRUE.

  IF TX-EXITOSA
    DISPLAY "LA TRANSACCIÓN LLEGO BIEN"
```

**Ventaja:** Hace que el código sea mucho más legible y menos propenso a errores de "dedazo" al escribir el valor literal.

**Formato 3** SET para [[Punteros]] (Direccionamiento)

Este formato se usa para comunicaciones avanzadas o cuando se trabaja con la **LINKAGE SECTION** para manipular direcciones de memoria directamente.

```cobol
DATA DIVISION.
01 PTR-CLIENTE    USAGE IS POINTER.
01 REG-DATOS      PIC X(100).

PROCEDURE DIVISION.
  *> Asigna la dirección de memoria de una variable a un puntero
  SET PTR-CLIENTE TO ADDRESS OF REG-DATOS.
```

**Formato 4:** SET para [[Switches]]

Aunque el estado inicial del switch viene del [[300-resources/JCL/jcl]], se puede **cambiar su valor** durante la ejecución del programa usando **SET**.

```cobol
IDENTIFICATION DIVISION.
PROGRAM-ID. PROG-SWITCHES.

ENVIRONMENT DIVISION.
CONFIGURATION SECTION.
SPECIAL-NAMES.
  UPSI-1 ON STATUS IS DEBUG-ACTIVADO.

PROCEDURE DIVISION.
  *> Lógica de negocio...

  IF DEBUG-ACTIVADO
    DISPLAY "DEBUG: DATOS RECIBIDOS CORRECTANMENTE."
  END-IF.

  *> Si por alguna razón queremos apagar el debug en medio del proceso:
  IF ALGO-FALLO:
    SET DEBUG-ACTIVADO TO OFF
  END-IF.

  STOP RUN.
```

**Como funciona:** Para que esto funcione con tus servidores, el equipo de Operaciones del Mainframe configura el **[[jcl/JCL]]** de la siguiente manera:

```jcl
//PASO01    EXEC    PGM=PROG-SWITCHES,UPSI=10000000
```

- El 1 en la primera posición activa el **UPSI-0**.
- El 0 lo desactiva.
