---
id: Switches
aliases:
  -  #switches
tags:
  -  #cobol
  -  #switches
---

En [[100-projects/Dominar Cobol]] son una reliquia técnica que sigue siendo extremadamente útil hoy en día, especialmente en el mundo del Mainframe. Sirven para **alterar el comportamiento de un programa desde el exterior** (normalmente desde el [[300-resources/JCL/jcl]] que lanza el proceso) sin tener que tocar el código ni recompilar.

Es como un mando a distancia con 8 botones que el operador del sistema puede pulsar antes de que el programa arranque.

1. ¿Dónde se definen? (SPECIAL-NAMES)

A diferencia de las variables normales, los switches se declaran en la **ENVIRONMENT DIVISION**. Aquí es donde se vincula un switch físico del sistema (llamado UPSI-0 hasta el UPSI-7) con el nombre que se elija.

**INFO** UPSI significa "User Programmable System Indicator"

```cobol
ENVIRONMENT DIVISION.
CONFIGURATION SECTION.
SPECIAL-NAMES.
    *> UPSI-0 es el primer interruptor del sistema
    UPSI-0  ON STATUS IS MODO-PRUEBA
      OFF STATUS IS MODO-PRODUCCION.
    *> UPSI-1 es el segundo
    UPSI-1  ON STATUS IS IMPRIMIR-LOGS-DETALLADOS.
```

2. ¿Cómo se usan en el código?

Los nombres que se definieron (MODO-PRUEBA, etc) se comportan como **nombres de condición** (parecidos al nivel 88)

- **Para consultar:** Se usa un [[If]]
- **Para modificar:** Se usa un [[Set]]

```cobol
PROCEDURE DIVISION.
  *> 1. Consultar el estado que viene de fuera
  IF MODO-PRUEBA
    DISPLAY "ATENCION: EJECUTANDO EN ENTORNO TEST"
    PERFORM CONECTAR-DB-TEST
  ELSE
    PERFORM CONECTAR-DB-PROD
  END-IF.

  *> 2. Cambiar el estado internamente
  IF ERROR-GRAVE
    SET  IMPRIMIR-LOGS-DETALLADOS TO ON
  END-IF.
```

3. Conexión con el JCL

Cuando se lanza el Job (proceso), el JCL tiene una sentencia llamada **UPSI**

```jcl
//PASO01  EXEC  PGM=TU_PROG,UPSI=10000000
```

- 10000000 significa: Switch 0 en **ON**, los demás en **OFF**
- 01000000 significa: Switch 1 en **ON**, los demás en **OFF**

4. Casos de uso específicos (¿Para qué sirven realmente?)

| Escenario              | Uso del Switch                                                                                                                                           |
| :--------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Mantenimiento nocturno | El JCL actica el Switch 0 para decirle al programa: "Solo lee datos, no permitas actualizaciones porque estamos en mantenimiento"                        |
| Depuración (Debug)     | Si una trama de EntireX está dando problemas, activas el Switch 1 en el JCL para que el COBOL empiece a imprimir todos los datos que recibe por consola. |
| Cierre de Ciclo        | Un switch puede indicar si es un "Cierre mensual" o un "Proceso diario", ejecutando cálculos distintos.                                                  |
