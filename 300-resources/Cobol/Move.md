---
id: Move
aliases:
  -  #move
  -  #MOVE
tags:
  -  #move
  -  #cobol
  -  #sentencias
---

# MOVE

Es la instruccón que usaremos para enviar datos de una variable a otra u otras. Lo que en verdad hace es que la variable adquiera un valor determinado, ya sea procedente de otra variable o bien desde un valor fijo o constante.

Es importante entender que **MOVE** no mueve realmente (en el sentido de quitar de un lado para poner en otro), sino que **copia** y, si es necesario, **transforma** el dato para que encaje en el formato del destino.

**Formato 1:** Formato básico

Puedes mover un valor a una variable o a muchas al mismo tiempo.

```cobol
IDENTIFICATION DIVISION.
PROGRAM-ID. EJEMPLO-MOVE.

DATA DIVISION.
WORKING-STORAGE SECTION.
01  MONTO-1     PIC 9(03).
01  MONTO-2     PIC 9(03).
01  NOMBRE      PIC X(20).

PROCEDURE DIVISION.
  *> Move simple de un literal
  MOVE 100 T0 MONTO-1.

  *> Move de una variabla a otra
  MOVE MONTO-1 TO MONTO-2.

  *> Move múltiple (inicializa varias variables al mismo tiempo)
  MOVE "PENDIENTE" TO NOMBRE, ESTADO-PROCESO.
```

**Formato 2:** Formato MOVE CORRESPONDING (MOVE CORR)

Este es el formato "inteligente". AL igual que el **ADD CORR** que vimos, busca campos que se llamen igual en dos estructuras diferentes y los mueve automáticamente.

```cobol
01 REGISTRO-ENTRADA.
  05 CLIENTE-ID   PIC 9(05).
  05 FECHA        PIC X(10).
  05 CIUDAD       PIC X(20).

01 REGISTRO-SALIDA.
  05 FECHA        PIC X(10).
  05 CLIENTE-ID   PIC 9(05).
  05 TELEFONO     PIC X(10).

PROCEDURE DIVISION.
  *> Solo moverá CLIENTE-ID y FECHA. CIUDAD Y TELEFONO  se ignoran.
  MOVE CORR REGISTRO-ENTRADA TO REGISTRO-SALIDA.
```

## Reglas de ORO

El **MOVE** es el responsable de que los datos se adapten.

- **Alfanumérico a Alfanumérico (X a X):** Si el destino es más corto, se corta (truncamiento) por la **derecha**.
- **Numérico a Numérico (9 a 9):** Si el destino tiene más decimales, se rellena con **ceros**.
  - Si el destino tiene menos enteros, se corta por la **izquierda** (¡Peligro de pérdida de datos!)
  - Se alinean siempre por el punto decimal implícito (V)

**Formato 3:** Move con variables de Edición (Formateo)

Este es el uso vital para reportes o para enviar datos legibles a la Web.

```cobol
01 MONTO-NUMERICO     PIC 9(05)V99 VALUE 01250.50.
01 MONTO-EDITADO      PIC $Z(04).99.

PROCEDURE DIVISION.
  MOVE MONTO-NUMERICO TO MONTO-EDITADO.
  *> El resultado será: "$ 1250.50"
  *> El 'Z' suprime los ceros a la izquierda (hasta 4 veces). MONTO-EDITADO agrega un '$'
```
