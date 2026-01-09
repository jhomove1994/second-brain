---
id: Perform
aliases:
  -  #perform
  -  #PERFORM
tags:
  -  #cobol
  -  #resources
  -  #perform
---

# PERFORM

Se utiliza para ejecutar repetidamente un conjunto de instrucciones un numero definido de veces o hasta que se presente una condición. Esta sentencia implica una bifurcación del flujo implícita y un retorno implícito a la siguiente sentencia despues del PERFORM. Su sintaxis es la siguiente:

```cobol
  PERFORM parrafo n times
  PERFORM parrafo UNTIL condicion
```

**PARRRAFO:** Serie de instrucciones agrupados bajo un nombre.

Formato 1: PERFORM simple

Es el uso mas basico: ejecutar un parrafo y volver.

```cobol
  PROCEDURE DIVISION.
    DISPLAY "INICIANDO PROCESO".
    PERFORM CONSULTAR-SALDO. *> Salta al parrafo
    DISPLAY "PROCESO TERMINADO". *> Vuelve aqui cuando el parrafo termina
    STOP RUN.
  CONSULTAR-SALDO.
    DISPLAY "CONECTANDO A BASE DE DATOS...".
    *> Al llegar al final de este bloque, COBOL "sabe" que debe volver.
```

**Formato 2:** PERFORM n TIMES (Repetición Fija)

Se usa cuando sabes exactamente cuantas veces quieres repetir algo.

```cobol
    PERFORM ENVIAR-ALERTA 3 TIMES.

  ENVIAR-ALERTA.
    DISPLAY "REINTENTANDO CONEXION CON ENTIREX...".
```

**Formato 3:** PERFORM UNTIL (Bucle condicional)
Es equivalente al **while** de otros lenguajes. Se ejecuta mientrras la condición sea falsa (Se detiene cuando sea verdadera).

```cobol
IDENTIFICATION DIVISION.
PROGRAM-ID. LIMPIEZA-DATOS.

DATA DIVISION.
WORKING-STORAGE SECTION.
01 CADENA-SUCIA    PIC X(20) VALUE "ABC*123*DEF*789".
01 CONT-ASTERISCOS PIC 9(02) VALUE 0.
01 FLAG-TERMINAR   PIC X(01) VALUE "N".

PROCEDURE DIVISION.
MAIN-LOGIC.
    PERFORM PROCESO-LIMPIEZA UNTIL FLAG-TERMINAR = "S".
    DISPLAY "CADENA LIMPIA: " CADENA-SUCIA.
    DISPLAY "TOTAL ASTERISCOS ELIMINADOS: " CONT-ASTERISCOS.
    STOP RUN.

PROCESO-LIMPIEZA.
    *> Contamos y reemplazamos en un solo paso
    INSPECT CADENA-SUCIA
        TALLYING CONT-ASTERISCOS FOR ALL "*"
        REPLACING ALL "*" BY "-".

    *> Cambiamos el FLAG para salir del PERFORM UNTIL
    MOVE "S" TO FLAG-TERMINAR.
```

**warning:** En #cobol tradicional, si la condición se cumple desde el principio, el bloque **no se ejecuta ni una vez**(como un while).

**Formato 4:** PERFORM VARYING (El bucle "For" completo)

Es el más potente. Controla una variable, le asigna un valor inicial, un incremento y una condición de salida en una sola línea. Es perfecto para recorrer las **TABLAS (OCCURS)**.

```cobol
IDENTIFICATION DIVISION.
PROGRAM-ID. RECORRER-TABLA.

DATA-DIVISION.
WORKING-STORAGE SECTION.
01 TABLA-MESES.
  05 MES-NOMBRE PIC X(10) OCCURS 12 TIMES INDEXED BY I.
01 CONTADOR     PIC 9(02).

PROCEDURE DIVISION.
  *> Inicializamos algunos datos manualmente
  MOVE "ENERO"      TO MES-NOMBRE(1).
  MOVE "FEBRERO"    TO MES-NOMBRE(2).
  MOVE "MARZO"      TO MES-NOMBRE(3).

  DISPLAY "--- INICIO DE LISTADO ---".

  *> Bucle para recorrer los primeros 3 meses
  PERFORM VARYING I FROM 1 BY 1 UNTIL I > 3
    DISPLAY "MES NUMERO " I ": " MES-NOMBRE(I)
  END-PERFORM.
```

**Formato 5:** PERFORM THRU
No ejecuta solo un parrafo, sino una secuencia de parrafos consecutivos.
El programa salta al primera párrafo mencionado, sigue de largo por todos los párrafos intermedios y **solo regresa** cuando termina de ejecutar el último párrafo del rango.

```cobol
DATA DIVISION.
WORKING-STORAGE SECTION.
01 ESTADO-PROCESO   PIC X(20) VALUE "PENDIENTE".

PROCEDURE DIVISION.
MAIN-LOGIC.
  DISPLAY "--- INICIO DE CIERRE ---".

  *> Ejecuta desde INICIO-CIERRE hasta FIN-CIERRE como un solo bloque
  PERFORM INICIO-CIERRE THRU FIN-CIERRE.

  DISPLAY "--- PROCESO COMPLETADO ---".
  DISPLAY "ESTADO FINAL: " ESTADO-PROCESO.
  STOP RUN.

INICIO-CIERRE.
  DISPLAY "PASO 1: BLOQUEANDO ARCHIVOS INDEXADOS...".
  MOVE "BLOQUEADO" TO ESTADO-PROCESO.

PROCESO-INTERMEDIO.
  DISPLAY "PASO 2: CALCULANDO SALDOS TOTALES...".
  MOVE "CALCULANDO" TO ESTADO-PROCESO.

FIN-CIERRE.
  DISPLAY "PASO 3: GENERANDO REPORTE SECUENCIAL...".
  MOVE "FINALIZADO" TO ESTADO-PROCESO.
  *> Aquí es donde el programa detecta que terminó el THRU  y regresa al MAIN-LOGIC.
```

**INFO:** Reglas de oro

1. **Orden físico:** Los parrafos deben estar escritos uno debajo del otro en el código. Si mueves un párrafo de lugar, el **THRU** podria ejecutar código que no querías o saltarse partes importantes.
2. **No hay retorno intermedio:** Si el **PERFORM** llama de A hasta C, y dentro de B hay un error, el programa seguirá hacia C a menos que uses un **GO TO** hacia el parrafo final.
3. **Encapsulamiento:** Útil cuando tienes una lógica larga que quieres dividir en pedazos pequeños para que sea legible, pero que siempre deben correr juntos.
