---
id: Initialize
aliases:
  -  #initialize
  -  #INITIALIZE
tags:
  -  #initialize
  -  #sentencias
  -  #boda
---

## INITIALIZE

Se utiliza para inicializar variables según su descripción, es decir pondrá a ceros todas las variables numéricas o de edición y a espacios en blanco las alfabéticas y alfanuméricas. No funciona con campos definidos como #Filler, y puede ser muy útil para inicializar tablas completamente cuando nos referimos al nivel mas alto de la misma.

### Estructura de ejemplo para cada uno de los formatos

Usaremos este registro de cliente para todos los ejemplos:

```cobol
DATA DIVISION.
WORKING-STORAGE SECTION.
01 REGISTRO-CLIENTE.
    05 ID-CLIENTE     PIC 9(05)   VALUE 12345.
    05 NOMBRE-CLI     PIC X(20)   VALUE "JUAN PEREZ".
    05 FILLER         PIC X(05)   VALUE "XXXXX".
    05 SALDO-DEUDOR   PIC 9(5)V99 VALUE 500.50.
    05 CATEGORIA      PIC X(01)   VALUE "A".
```

**Formato 1:** INITIALIZE Simple (El estándar)

Inicializa todo el grupo siguiendo las reglas: Numéricos a 0 y Alfanuméricos a **SPACE**

```cobol
PROCEDURE DIVISION.
    INITIALIZE REGISTRO-CLIENTE.

    *> Resultado:
    *> ID-CLIENTE = 00000
    *> NOMBRE-CLI = "                    "
    *> FILLER     = "XXXXX"  No se modifica
    *> SALDO-DEUDOR = 00000.00
```

**Formato 2:** INITIALIZE con REPLACING (Filtrado por tipo)

Este formato es muy útil si solo quieres limpiar, por ejemplo, los montos de dinero pero dejar los nombres intactos.

```cobol
    INITIALIZE REGISTRO-CLIENTE
        REPLACING NUMERIC DATA BY ZEROES.

    *> Resultado:
    *> ID-CLIENTE y SALDO-DEUDOR  pasan a 0.
    *> NOMOBRE-CLI y CATEGORIA mantienen su valor original ("JUAN PEREZ", "A").
```

**Formato 3:** INITIALIZE con REPLACING ALPHANUMERIC

Puedes inicializar con un carácter específico en lugar de espacios.

```cobol
    INITIALIZE REGISTRO-CLIENTE
        REPLACING ALPHANUMERIC DATA BY ALL "-".

    *> Resultado:
    *> NOMBRE-CLI = "--------------------"
    *> CATEGORIA   = "-"
    *> Los números NO se tocan.
```
