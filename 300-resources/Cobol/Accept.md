---
id: Accept
aliases:
  -  #Accept
  -  #ACCEPT
tags:
  -  #accept
  -  #resources
  -  #cobol
---

# ACCEPT

Es la instrucción que usaremos para la entrada de datos. Aunque su sintaxis principal nunca ha variado, ésta ha sido una de las instrucciones que mas cláusulas se le han ido añadiendo, incluso diferentes según el compilador, aquí vamos a explicar las comunes y principales.

- **ACCEPT para datos del sistema (Fecha y Hora):**
  Es el uso más común hoy en día. Sirve para que el prgrama sepa cuándo se está ejecutando y poder estampar esta fecha en los archivos o reportes.

```cobol
IDENTIFICATION DIVISION.
PROGRAM-ID. EJEMPLO-ACCEPT-SISTEMA.

DATA DIVISION.
WORKING-STORAGE SECTION.
01 FECHA-SISTEMA    PIC 9(06). *> Formato AAMMMDD
01 HORA-SISTEMA     PIC 9(08). *> Formato HHMMSScc (cc son centésimas)
01 DIA-DE-LA-SEMANA PIC 9(01). *> 1 para Lunes, 7 para Domingo

PROCEDURE DIVISION.
  *> Obtenemos la fecha actual del Mainframe
  ACCEPT FECHA-SISTEMA FROM DATE.
  ACCEPT HORA-SISTEMA FROM TIME.
  ACCEPT DIA-DE-LA-SEMANA FROM DAY-OF-WEEK.

  DISPLAY "FECHA (AAMMDD): "  FECHA-SISTEMA.
  DISPLAY "HORA (HHMMSS): "   HORA-SISTEMA.
  DISPLAY "DIA SEMANA: "      DIA-DE-LA-SEMANA.

  STOP RUN.
```

- **ACCEPT desde una fuente externa (SYSIN):**

Este formato se usa para leer datos que se pasan a través de **JCL** (el "script" que lanza el programa en el Mainframe). Es el ideal para pasarle al programa un "parámetro" sin tenr que modificar el código.

```cobol
IDENTIFICATION DIVISION.
PROGRAM-ID. EJEMPLO-ACCEPT-SYSIN.

DATA DIVISION.
WORKING-STORAGE SECTION.
01 PARAMETRO-ENTRADA  PIC X(10).

PROCEDURE DIVISION.
  DISPLAY "ESPERANDO DATO DESDE SYSIN...".

  *> Lee la primera línea disponible en el flujo de entrada SYSIN
  ACCEPT PARAMETRO-ENTRADA.

  DISPLAY "EL DATO RECIBIDO FUE: " PARAMETRO-ENTRADA.
  STOP RUN.
```

- ACCEPT Interacción en tiempo real (Modo Consola):

En este escenario, el programa se detiene completamente, muestra un cursor y espera a que una persona escriba algo y presion **ENTER**

```cobol
IDENTIFICATION DIVISION.
PROGRAM-ID. INTERACCION-REAL.

DATA DIVISION.
WORKING-STORAGE SECTION.
01 NOMBRE-USUARIO   PIC X(20).
01 CONFIRMACION     PIC X(01).

PROCEDURE DIVISION.
  DISPLAY "POR FAVOR, INGRESE SU NOMBRE: " WITH NO ADVANCING.
  *> El programa se detiene aquí hasta que el usuario escrib algo
  ACCEPT NOMBRE-USUARIO.

  DISPLAY "HOLA " NOMBRE-USUARIO ". ¿DESEA CONTINUAR? (S/N): "
      WITH NO ADVANCING.
  ACCEPT CONFIRMACION.

  IF CONFRIMACION = "S" OR "s"
      DISPLAY "PROCESO INICIADO..."
  ELSE
      DISPLAY "PROCESO CANCELADO."
  END-IF.

  STOP RUN.
```

**WITH NO ADVANCING:** Se utiliza con la instrucción **DISPLAY**, COBOL añade un carácter de "Nueva Línea" (Line Feed/Return). Si haces dos **DISPLAY** seguidos, verás dos lineas. Con **WITH NO ADVANCING**, logras que el siguiente elemento que se imprima (ya sea otro **DISPLAY** o un **ACCEPT**) aparezca **en la misma línea**.

**WARNING**:
En programas que corren bajo **#CICS** (transacciones en tiempo real), el comando **ACCEPT** estándar a veces está restrigido o se reemplaza por comandos propios de **CICS** (EXEC CICS ASKTIME). Sin embargo, en programas **#Batch**, el **ACCEPT** es el rey para obtener información del entorno.

- **DATE:** devuelve la fecha en formato **AAMMDD**, por lo que la variable debe de estar definida como PIC 9(6).
- **DAY:** Devuelve el año y el día del año en que estamos con el formato **AADDD**, siendo el valor 1, para el 1 de Enero y así sucesivamente. Debe de estar definida con PIC 9(5).
- **DAY-OF-WEEK:** Devuelve un dígito que indica el día de la semana siendo 1 el Lunes, 2 el Martes, ... Aquí, la variable debe de estar definida como PIC 9.
- **TIME:** Devuelve la hora con formato **HHMMSSMM**, la variable debe de estar como PIC 9(8)
