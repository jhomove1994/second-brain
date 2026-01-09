---
id: Search
aliases:
  -  #search #SEARCH
tags:
  -  #cobol
  -  #sentencias
  -  #search
---

[[100-projects/Dominar Cobol]]

# SEARCH

Busca elementos dentro de una tabla [[Occurs]]. En lugar de escribir manualmente un bucle [[Perform]] que recorra la tabla uno por uno, el **SEARCH** lo hace de forma automática y mucho más eficiente.

Existen dos tipos fundamentales: el **SEARCH** (Lineal) y el **SEARCH ALL** (Binario).

1. **SEARCH Lineal (Simple)**

Este formato recorre la tabla desde la posición actual del índice hasta encontrar lo que se busca o llegar hasta el final.

**Requisitos**

- La tabla debe tener un índice (**INDEXED BY**)
- Se debe inicializar el índice con un [[Set]] antes de empezar.

```cobol
DATA DIVISION.
WORKING-STORAGE SECTION.
01 TABLA-PRODUCTOS.
  05 PRODUCTO OCCURS 100 TIMES INDEXED BY IND-PROD.
    10 CODIGO-PROD  PIC 9(05).
    10 NOMBRE-PROD  PIC X(20).

PROCEDURE DIVISION.
  *> 1. Siempre inicializar el índice.
  SET IND-PROD TO 1.

  *> 2. Ejecutar la busqueda.
  SEARCH PRODUCTO
    AT END
      DISPLAY "ERROR: EL PRODUCTO NO EXISTE"
    WHEN CODIGO-PROD(IND-PROD) = 12345
      DISPLAY "ENCONTRADO: " NOMBRE-PROD(IND-PROD)
  END-SEARCH.
```

2. SEARCH ALL (Búsqueda Binaria)

En lugar de mirar uno por uno, divide la tabla a la mitad sucesivamente. Es increiblemente rápido para tablas grandes.

**Requisitos estrictos:**

1. La tabla debe estar **ordenada** (ascendente o descendente).
2. Debes declarar ese orden en la definición de la tabla con **ASCENDING/DESCENDING KEY**.
3. **No** se necesita inicializar el índice con **SET** (**COBOL** lo maneja solo).

```cobol
01 TABLA-EMPLEADOS.
  05 EMPLEADO OCCURS 1000 TIMES
    ASCENDING KEY IS ID-EMP *> Obligatorio para SEARCH ALL
    INDEXED BY IND-EMP.
    10 ID-EMP    PIC 9(08).
    10 NOM-EMP   PIC X(30).

PROCEDURE DIVISION.
  SEARCH ALL EMPLEADO
    WHEN ID-EMP(IND-EMP) = 00123456
      DISPLAY "EMPLEADO HALLADO: " NOM-EMP(IND-EMP)
    END-SEARCH.
```

3. SEARCH con VARYING

Es un truco avanzado que sirve para sincronizar dos índices al mismo tiempo.

Normalmente, el **SEARCH** solo incrementa automáticamente el índice de la tabla que se esta recorriendo. Al agregar **VARYING**, le estas pidiendo a **COBOL** que **incremente un segundo índice (o una variable numérica)** al mismo ritmo que el índice principal.

- **¿Cuando se usa?:** Imagina que tienes dos tablas que "van de la mano", pero están separadas, o que quieres buscar algo en una tabla y saber en qué posición numérica exacta estás para usarla en otra parte.

**Ejemplo: Sincronizar dos tablas**

Tienes una tabla de **Códigos** y otra de **Precios**. Quieres buscar el código, pero necesitas que el índice de la tabla de precios se mueva igual.

```cobol
DATA DIVISION.
WORKING-STORAGE SECTION.
01 TABLA-CODIGOS.
  05 CODIGO     PIC 9(02) OCCURS 10 TIMES INDEXED BY IND-COD.
01 TABLA-PRECIOS.
  05 PRECIO     PIC 9(05) OCCURS 10 TIMES INDEXED BY IND-PRE.

PROCEDURE DIVISION.
  *> Inicializamos  AMBOS índices
  SET IND-COD TO 1.
  SET IND-PRE TO 1.

  SEARCH CODIGO
    VARYING IND-PRE *> Esto hace que IND-PRE crezca junto con IND-COD
    AT END
      DISPLAY "NO ENCONTRADO"
    WHEN CODIGO(IND-COD) = 55
      DISPLAY "PRECIO CORRESPONDIENTE: " PRECIO(IND-PRE)
  END-SEARCH.
```

- **Uso con variables numéricas:** A veces no se quiere mover otro índice, sino simplemente contar cuántos elementos se recorrio antes de encontrar lo que buscamos.

```cobol
  SET IND-COD TO 1.
  MOVE 0 TO CONTADOR-SALTOS.

  SEARCH CODIGO
    VARYING CONTADOR-SALTOS
    WHEN CODIGO(IND-COD) = 99
      DISPLAY "LO ENCONTRE DESPUES DE " CONTADOR-SALTOS " INTENTOS."
  END-SEARCH.
```

4. **Diferencias clave entre **SEARCH** y**SEARCH ALL\*\*

| Característica  | SEARCH                         | SEARCH ALL                               |
| :-------------- | :----------------------------- | :--------------------------------------- |
| **Velocidad**   | Lento en tablas grandes (O(n)) | Muy rápido (O(log n))                    |
| Orden en tabla  | No importa.                    | **Obligatorio** que esté ordenada.       |
| Índice          | Requiere **SET IND TO 1**      | Manejo automático                        |
| **Condiciones** | Puedes usar >, <, OR, etc.     | Solo permite **igualdad** (=) y **AND**. |
