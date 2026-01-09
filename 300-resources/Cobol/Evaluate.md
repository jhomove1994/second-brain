---
id: Evaluate
aliases:
  -  #Evaluate #EVALUATE
tags:
  -  #cobol
  -  #sentencias
  -  #evaluate
---

[[100-projects/Dominar Cobol]]

# EVALUATE

El comando **EVALUATE** es el equivalente al **switch** de otros lenguajes, pero mucho más potente. En **COBOL**, se le conoce como el "Case estructural" y es la mejor forma de evitar los famosos "IF anidados" (esos códigos donde el **IF** se va desplazando hacia la derecha y se vuelve ilegible).

1. Estructura Básica

```cobol
  EVALUATE VARIABLE-O-EXPRESION
    WHEN valor-1
      instrucciones-1
    WHEN valor-2
      instrucciones-2
    WHEN ANY
      instrucciones-si-cualquier-valor
    WHEN OTHER
      instrucciones-si-no-es-ninguno-de-los-anteriores
  END-EVALUATE..
```

2. Formatos Avanzados

A diferencia de Java, el **EVALUATE** permite evaluar **rangos, múltiples variables** y hasta **condiciones lógicas** simultáneamente.

A. Rangos y Listas

```cobol
  EVALUATE NOTA-EXAMEN
    WHEN 90 THRU 100
      DISPLAY "EXCELENTE"
    WHEN 75 THRU 89
      DISPLAY "BUENO"
    WHEN 60 THRU 74
      DISPLAY "SUFICIENTE"
    WHEN 0 THRU 59
      DISPLAY "INSUFICIENTE"
    WHEN OTHER
      DISPLAY "NOTA INVALIDA"
  END-EVALUATE.
```

B. Evaluación de TRUE/FALSE

Se puede evaluar si ciertas condiciones se cumplen sin necesidad de una variable central.

```cobol
  EVALUATE TRUE
    WHEN SALDO < 0
      DISPLAY "MOROSO"
    WHEN SALDO = 0
      DISPLAY "AL DIA"
    WHEN SALDO > 0
      DISPLAY  "CON SALDO A FAVOR"
  END-EVALUATE.
```

3. Evaluación de Múltiples Sujetos (El Formato ALSO)

Se puede evaluar dos o más variables al mismo tiempo, como una "matriz de decisión".

```cobol
  EVALUATE TIPO-CLIENTE ALSO ESTADO-CUENTA
    WHEN "VIP"      ALSO "ACTIVA"
      PERFORM PROCESO-PRIORITARIO
    WHEN "ESTANDAR" ALSO "ACTIVA"
      PERFORM PROCESO-NORMAL
    WHEN ANY        ALSO "BLOQUEADA"
      PERFORM PROCESO-ERROR
  END-EVALUATE.
```

**Reglas de Oro del EVALUATE**

- **Sin "Break":** A diferencia de Java, **COBOL** **no necesita un break**. Una vez que encuentra un **WHEN** que coincide, ejecuta sus instrucciones y salta directamente al **END-EVALUATE**. No sigue ejecutando los siguientes **WHEN**.

- **WHEN OTHER:** Es muy recomendable ponerlo siempre al final para capturar valores inesperados que podrían venir de la red o de la base de datos.
- **Orden:** **COBOL** evalúa los **WHEN** de arriba hacia abajo. El primero que sea verdadero es el que se queda el control.
