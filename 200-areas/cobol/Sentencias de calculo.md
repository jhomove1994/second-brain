---
id: Sentencias de Calculo
aliases: []
tags:
  -  #cobol
  -  #sentencias
---

# Sentencias de Calculo

Cobol no incluye dentro de sus especificaciones verbos que nos pudieran ayudar a resolver calculos complejos como integrales, trigonometria, raices cuadradas, etc, sino simplemente las orientadas a los calculos basicos, suma, resta, multiplicacion y division que son las que vamos a ver a continuacion:

- add
- substract
- multiply
- divide
- compute
- inspect

## ADD

Se usa para realizar sumas y tiene los siguientes formatos:

1. ADD variable o literal ... TO (ROUNDED) (ON SIZE ERROR) instruccion
   Este se usa cuando quieres actualizar un contador o un saldo acumulado.

```cobol
 IDENTIFICATION DIVISION.
 PROGRAM-ID. EJEMPLO-ADD-TO.

 DATA DIVISION.
 WORKING-STORAGE SECTION.
 01 SALDO-CONTABLE     PIC 9(4)V99 VALUE 1500.25.
 01 INTERES-MENSUAL    PIC 9(2)V99 VALUE 10.88.

 PROCEDURE DIVISION.
 INICIO.
   *> Aqui definimos las variables y su tamanio (PIC)
   ADD INTERES-MENSUAL TO SALDO-CONTABLE
       ROUNDED
       ON SIZE ERROR
           DISPLAY "ERROR: EL SALDO EXCEDE LOS 4 DIGITOS ENTEROS"
 END-ADD.

 DISPLAY "NUEVO SALDO: " SALDO-CONTABLE.
 STOP RUN.
```

- ROUNDED: Si el resultado tiene mas decimales de los que SALDO puede guardar, COBOL redondea al mas cercano en lugar de simplemente truncar (cortar) el numero.
- ON SIZE ERROR: Se dispara si el resultado es mas grande que el PIC definido (por ejemplo, si el saldo intenta llegar a 100,000.00 pero solo definimos 5 digitos enteros).

2. ADD variable o literal ... GIVING variable (ROUNDED) (ON SIZE ERROR) instruccion
   Ideal para facturacion, donde quieres mantener el precio base intacto y calcular el total.

```cobol
  IDENTIFICATION DIVISION.
  PROGRAM-ID. EJEMPLO-GIVING.

  DATA DIVISION.
  WORKING-STORAGE SECTION.
  01 PRECIO-NETO      PIC 9(4)V99 VALUE 2000.00.
  01 IVA              PIC 9(4)V99 VALUE 0320.00.
  01 TOTAL-FACTURA    PIC 9(4)V99. *> Variable vacia para el resultado.

  PROCEDURE DIVISION.
  CALCULO-TOTAL.
      *> PRECIO-NETO e IVA se mantienen iguales.
      ADD PRECIO-NETO IVA GIVING TOTAL-FACTURA
          ON SIZE ERROR
              DISPLAY "ERROR DE CAPACIDAD DE FACTURA"
      END-ADD.
      DISPLAY "EL TOTAL A PAGAR ES: " TOTAL-FACTURA
      STOP RUN.
```

- Diferencia Clave: Se usa una tercera variable de destino. Es mas seguro si necesitas reutilizar los valores originales en otros calculos.

3. ADD CORR variable TO variable (ROUNDED) (ON SIZE ERROR)
   Suma estructural: Este es el mas avanzado. Se usa para mover datos entre grupos (como de una pantalla de WebLogic a un registro de Mainframe)

```COBOL
  IDENTIFICATION DIVISION.
  PROGRAM-ID. EJEMPLO-CORR.

  DATA DIVISION.
  WORKING-STORAGE SECTION.
  *> Grupo de origen: Datos de la sucursal A
  01 SUCURSAL-A.
    05 VENTAS     PIC 9(5) VALUE 10000.
    05 GASTOS     PIC 9(5) VALUE 02000.
    05 EXTRAS     PIC 9(5) VALUE 00500.

  *> Grupo de destino: Totales de la empresa
  01 TOTAL-EMPRESA.
    05 VENTAS     PIC 9(7) VALUE 500000.
    05 GASTOS     PIC 9(7) VALUE 100000.
    05 OTROS     PIC 9(7) VALUE 000000.

  PROCEDURE DIVISION
  CONSOLIDACION.
    *> COBOL suma VENTAS con VENTAS Y GASTOS cons GASTOS AUTOMATICAMENTE.
    ADD CORR SUCURSAL-A TO TOTAL-EMPRESA.

    DISPLAY "TOTAL VENTAS EMPRESA: " VENTAS OF TOTAL-EMPRESA.
    *> 'EXTRAS' no se sumo porque no existe en el destino.
    *> 'OTROS' quedo igual porque no existe en el origen.
    STOP RUN.
```

## SUBSTRACT

Se usa para realizar restas y tiene los siguientes formatos:

1. SUBSTRACT variable o literal ... FROM variable (ROUNDED) (ON SIZE ERROR) instruccion
2. SUBSTRACT variable o literal ... FROM variable o literal GIVING variable (ROUNDED) (ON SIZE ERROR) instruccion
3. SUBSTRACT CORR variable FROM variable (ROUNDED) (ON SIZE ERROR) insruccion.

## MULTIPLY

Se usa para realizar multiplicaciones y tiene los siguientes formatos:

1. MULTIPLY variable o literal BY variable (ROUNDED) (ON SIZE ERROR) instruccion
2. MULTIPLY variable o literal BY variable o literal GIVING variable (ROUNDED) (ON SIZE ERROR) instruccion.

## DIVIDE

Se usa para realizar divisiones y tiene los siguientes formatos:

1. DIVIDE variable o literal INTO variable (ROUNDED) (ON SIZE ERROR) instruccion
   Este formato toma el divisor y lo "mete" en el dividendo, reemplezando el valor original de este ultimo por el cociente.

   ```cobol
    IDENTIFICATION DIVISION.
    PROGRAM-ID. EJEMPLO-DIVIDE-INTO.

    DATA DIVISION.
    WORKING-STORAGE SECTION.
    01 TOTAL-DEUDA    PIC 9(6)V99 VALUE 1500.00
    01 DIVISOR-CUOTAS PIC 9(2)V99 VALUE 12.

    PROCEDURE DIVISION.
      *> TOTAL-DEUDA sera reemplazado por el resultado (1500 / 12)
      DIVIDE DIVISOR-CUOTAS INTO TOTAL-DEUDA
        ROUNDED
        ON SIZE ERROR
          DISPLAY "ERROR: EL COCIENTE NO CABE EN LA VARIABLE"
      END-DIVIDE.

      DISPLAY "VALOR POR CUOTA: " TOTAL-DEUDA. *> Imprime 125.00
      STOP RUN.
   ```

   **Diferencia clave:** Es una operacion "destructiva" para la segunda variable. El dividendo desaparece y se convierte en el resultado.

2. DIVIDE variable o literal (BY o INTO) variable o literal GIVING variable (ROUNDED) (REMAINDER) variable (ON SIZE ERROR) instruccion
   Este es el formato mas comun porque es el mas legible. Aqui se tiene las dos variantes:

- BY: Dividendo BY Divisor
- INTO: Divisor INTO Dividendo.

```cobol
 IDENTIFICATION DIVISION.
 PROGRAM-ID. EJEMPLO-DIVIDE-GIVING

 DATA DIVISION.
 WORKING-STORAGE SECTION.
 01 MONTO-TOTAL         PIC 9(5)V99 VALUE 5000.00.
 01 CANTIDAD-PERSONAS   PIC 9(2)    VALUE 03.
 01 RESULTADO-DIV       PIC 9(5)V99.
 01 SOBRANTE            PIC 9(5)V99.

 PROCEDURE DIVISION.
  *> Usando BY con GIVING y REMAINDER
  DIVIDE MONTO-TOTAL BY CANTIDAD-PERSONAS
      GIVING RESULTADO-DIV ROUNDED
      REMAINDER SOBRANTE
      ON SIZE ERROR
        DISPLAY "ERROR MATEMATICO"
  END-DIVIDE.

DISPLAY "CADA UNO PAGA: " RESUTALDO-DIV. *> 1666.67
DISPLAY "SOBRAN EN CENTAVOS: " SOBRANTE. *> Lo que no se pudo repartir
STOP RUN.
```

- **REMAINDER:** Es fundamental cuando el dinero no es divisible exactamente.
  - Importante: Si se usa **ROUNDED** junto con **REMAINDER**, el residuo se calcula antes de redondear el cociente. Esto garantiza que la suma de (Cociente \* Divisor) + Residuo siempre sea igual al valor original.
- **INTO vs BY:** Es facil confundirse, La regla nemotecnica es:
  - INTO: El primer numero es el que "divide al segundo"
  - BY: El primer numero es el que "es dividido por el segundo".
- **ON SIZE ERROR:** En division este error ocurre en dos casos:
  - Si el resultado es mas grande que el PIC del destino.
  - Division por CERO, Si el divisor es 0, COBOL no detiene el programa abruptamente si tienes definida esta clausula; ejecutara la instruccion que se ponga ahi.

## COMPUTE

Con esta orden podemos realizar todos los calculos aritmeticos posibles en una sola instruccion, utilizando los operadores +(suma), -(resta), \*(multiplicacion), /(division), \*\*(potenciacion), ademas de utilizar parentesis para especificar mejor la operacion a realizar.

1. COMPUTE variable (ROUNDED) = expresion aritmetica (ON SIZE ERROR) instruccion.

Debemos tener en cuenta que siempre tienen preferencia los operadores que vayan entre parentesis, a continuacion los de multiplicacion y division y por ultimo los de suma y resta y el orden en que va a ir realizando las operaciones es de izquierda a derecha.
Ejemplo:

```cobol
  COMPUTE  RESULTADO = 2 + 3 * 5.
```

Esta operacion daria como resultado 3\*5=15+2=17

```cobol
  COMPUTE RESULTADO = (2 + 3) * 5.
```

Esta operacion daria como resultado 2+3=5\*5=25

## INSPECT

Esta sentencia se utiliza para contar y reemplazar caracteres o grupos de caracteres dentro de un campo. Se puede contar las veces que aparece un caracter, o cambiar todos esos caracteres por otros, etc.

Esta instruccion tiene formatos diferentes segun lo que se desee hacer:

- **Formato 1**:
  Se usa para saber cuantos caracteres reales (incluyendo espacios) hay en un campo.

```cobol
  DATA DIVISION.
  WORKING-STORAGE SECTION.
  01 CAMPO-PRUEBA     PIC X(20) VALUE "HOLA MUNDO"
  01 CONTADOR         PIC 9(02) VALUE 0.

  PROCEDURE DIVISION
  *> Es VITAL inicializar el contador, porque INSPECT SUMA al valor actual MOVE ZERO TO CONTADOR.
  MOVE ZERO TO CONTADOR.
  INSPECT CAMPO-PRUEBA TALLYING CONTADOR FOR CHARACTERS.

  DISPLAY "VALOR: " CONTADOR. *> Imprimira 20 ( el tamanio total del PIC)
```

- CHARACTERS: Indica que cuente todos los caracteres del campo incluso los de espacios en blanco

- **Formato 2:** BEFORE INITIAL (Contar antes de...)
  Muy util para saber cuantos caracteres hay antes de un delimitador ( como una coma o un espacio )

  ```cobol
    DATA DIVISION.
    WORKING-STORAGE SECTION.
    01  CORREO      PIC X(30) VALUE "usuario@banco.com".
    01 POS-ARROBA   PIC 9(02) VALUE 0.

    PROCEDURE DIVISION.
      MOVE 0 TO POS-ARROBA.
      INSPECT CORREO TALLYING POS-ARROBA
        FOR CHARACTERS BEFORE INITIAL "@".

      DISPLAY "CARACTERES ANTES DEL @: " POS-ARROBA. *> Imprimira 07
  ```

  - BEFORE INITIAL: Busca solo hasta que aparezca la cadena especificada como Cadena1.

- Formato 3: AFTER INITIAL (Contador despues de...)
  Se usa para analizar la parte final de una cadena tras encontrar un caracter clave.

  ```cobol
  DATA DIVISION.
  WORKING-STORAGE SECTION.
  01  RUTA-ARCHIVO    PIC X(30) VALUE "REPORTES/ENERO/DOC1.TXT".
  01  CONT-DESPUES    PIC 9(02) VALUE 0.

  PROCEDURE DIVISION.
    MOVE 0 TO CONT-DESPUES.
    INSPECT RUTA-ARCHIVO TALLYING CONT-DESPUES
      FOR CHARACTERS AFTER INITIAL "/".

    DISPLAY "CARACTERES TRAS EL PRIMER /: " CONT-DESPUES.
  ```

  - AFTER INITIAL: empieza a buscar justo despues de la cadena especificada en Cadena1

- Formato 4: REPLACING
  A diferencia de TALLYING, REPLACING si modifica el contenido del campo. No cuenta nada, simplemente "busca y destruye" ( o cambia).

  ```cobol
    DATA DIVISION.
    WORKING-STORAGE SECTION.
    01  FECHA-MALA        PIC X(10) VALUE  "2024.05.20".
    01  NOMBRE-USUARIO    PIC X(20) VALUE  "  JUAN PEREZ  ".
    01  MONTO-TEXTO       PIC X(10) VALUE  "0000125000".

    PROCEDURE DIVISION.
    *> 1. ALL: Cambia todas las apariciones (Limpiar puntos de una fecha)
    INSPECT FECHA-MALA REPLACING ALL "." BY "/".
    *> RESULTADO: "2024/05/20"

    *> 2. LEADING: Cambia solo los caracteres que estan AL PRINCIPIO.
    *> Util para quitar ceros a la izquierda o espacios iniciales
    INSPECT MONTO-TEXTO REPLACING LEADING "0" BY SPACE.
    *> Resultado: "    125000"

    *> 3. FIRST: Cambia solo la primera vez que lo vea
    INSPECT NOMBRE-USUARIO REPLACING  FIRST " " BY "_"
    *> Resultado: "__ JUAN PEREZ   " (Solo cambio el primer espacio)
  ```

### TALLYING y REPLACING se pueden usar juntos

```cobol

    MOVE 0 TO CONT-ERRORES.
    INSPECT CAMPO-ENTRADA
        TALLYING CONT-ERRORES FOR ALL "*" *> Cuenta los asteriscos prohibidos
        REPLACING ALL "*" BY " "

    IF CONT-ERRORES > 0
        DISPLAY "SE ENCONTRARON Y LIMPIARON " CONT-ERRORES " ERRORES".
```

**INFO:**

- **¡Cuidado con el acumulador!:** El error mas comun en cobol es no hacer MOVE 0 TO variable1 antes del INSPECT. Si el contador ya tiene un 5 y el INSPECT encuentra 3 coincidencias, el resultado final sera 8.
- **Combinacion de filtros:** Puedes combinar varios en una sola linea. Por ejemplo: INSPECT campo TALLYING var1 FOR ALL "A" var2 FOR ALL "B"
- **Diferencia con REPLACING:** TALLYING solo cuenta, Si lo que quieres es cambiar un caracter por otro ( por ejemplo, cambiar comas por puntos), usarias INSPECT ... REPLACING.

## GO TO

Es una instruccion que ya esta obsoleta

## #PERFORM

## #CallYExit

## STOP RUN

Es la insstrucción final del programa fuente, indica al compilador que se puede iniciar la ejecución del programa.

## #INITIALIZE

## #ACCEPT

## #SET
