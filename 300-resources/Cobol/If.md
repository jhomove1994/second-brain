---
id: If
aliases:
  -  #if #IF
tags:
  -  #cobol
  -  #sentencias
  -  #if
---

[[100-projects/Dominar Cobol]]

# IF

Es la estructura de control fundamental para la toma de decisiones. Aunque parece sencillo, tiene una sintaxis muy rigurosa que ha evolucionado desde el **COBOL** antiguo hasta el moderno.

1. Estructura Básica y el END-IF

Existen dos formas de cerrar un **IF**: con **punto (.)** o con la sentencia **END-IF**.
**Recomendación:** En entornos modernos, se usa siempre END-IF. El punto es peligroso porque si se pone por error antes de tiempo, se termina toda la lógica del programa.

```cobol
  IF condicion
    instrucciones-si-es-verdadero
  ELSE
    instrucciones-si-es-falso
  END-IF.
```

2. Tipos de Condicionales

COBOL permite evaluar condiciones de forma muy natural

| Condición         | Operador Simbólico | Operador Palabra |
| :---------------- | :----------------- | :--------------- |
| Igual a           | =                  | EQUAL TO         |
| Mayor que         | >                  | GREATER THAN     |
| Menos que         | <                  | LESS THAN        |
| Mayor o igual que | >=                 | GREATER THAN     |
| Distinto de       | NOT =              | NOT EQUAL TO     |

3. Evaluaciones Especiales

A. Clase de Dato (NUMERIC / ALPHABETIC): Ideal para validar tramas. Se puede saber si un campo que debería ser número trae basura:

```cobol
  IF DATA-RECIBIDO IS NOT NUMERIC:
    DISPLAY "ERROR: WEBLOGIC ENVIO TEXTO EN UN CAMPO NUMERICO"
  END-IF.
```

B. Nombres de Condición (Nivel 88):
En lugar de preguntar por el valor, preguntas por el nombre lógico:

```cobol
  05 ESTADO-CIVIL PIC X VALUE "S".
    88 SOLTERO  VALUE "S".
    88 CASADO   VALUE "C".

  IF SOLTERO
    DISPLAY "EL CLIENTE ES SOLTERO"
  END-IF.
```

C. Signos

```cobol
  IF SALDO-CUENTA IS POSITIVE *> O NEGATIVE o ZERO
      PERFORM PROCESAR-PAGO
  END-IF.
```

4. IFs Anidados y Evaluaciones Compuestas

Se puede combinar condiciones usando **AND**, **OR** y paréntesis.

```cobol
    IF (EDAD > 18 AND CIUDAD = "BOGOTA") OR SOCIO-VIP
      MOVE "S" TO PERMISO-ACCESO
    ELSE
      MOVE "N" TO PERMISO-ACCESO
    END-IF.
```

5. El uso de NEXT SENTENCE

A veces quieres que, si se cumple una condición, el programa simplemente "salte" todo lo que queda del bloque **IF** y siga con la siguiente instrucción después del punto.

```cobol
  IF SALDO-NULO
    NEXT SENTENCE
  ELSE
    PERFORM ACTUALIZAR-SALDO
  END-IF.
  DISPLAY "PROCESO TERMINADO". *> Aquí salta el NEXT SENTENCE
```

**Un truco de legibilidad: El IF abreviado**
**COBOL** permite omitir el sujeto si es el mismo:

- IF VALOR > 10 AND 20 (Es lo mismo que IF VALOR > 10 AND VALOR < 20)
