---
id: punteros
aliases:
  -  #punteros
tags:
  -  #cobol
  -  #punteros
---

# PUNTEROS

El manejo de punteros en #cobol es un tema de "nivel avanzado" porque rompe un poco con la filosofía tradicional del lenguaje de tener todo en estructuras fijas. Se introdujeron para permitir que #cobol interactúe con lenguajes como #C, #Java, o para manejar memoria dinámica y estructuras de datos complejas.

Son fundamentales para recibir tramas de longitud variable o para apuntar a bloques de memoria que el middleware llena a automáticamente.

1. **Conceptos Básicos: POINTER y ADDRESS OF**

Existen dos elementos clave para trabajar con direcciones de memoaria:

- USAGE IS POINTER: Es el tipo de dato. Define una variable que no guarda un nombre o un número, sino una **dirección de memoria** (normalmente 4 u 8 bytes dependiente de la arquitectura del Mainframe).

- **ADDRESS OF:** Es un registro especial que apunta a la ubicación física de una variable en la memoria.

2. Como se definen los Punteros.

Los punteros se definen en la **WORKING-STORAGE** o en la **LINKAGE SECTION**

```cobol
DATA DIVISION.
WORKING-STORAGE SECTION.
01 MI-PUNTERO     USAGE IS POINTER.
01 VARIABLE-REAL  PIC X(10) VALUE "Hola"

LINKAGE SECTION.
01 ESTRUCTURA-REMOTA.
  05 CAMPO-1      PIC 9(05).
  05 CAMPO-2      PIC X(20).
```

3. El comando **[[Set]]**: La clave de los Punteros

Para asignar o manipular punteros, **siempre** usamos **[[Set]]**. No se puede usar **[[Move]]** con variables de tipo puntero.

A. Asignar la dirección de una variable:

```cobol
*> Guardamos la dirección de VARIABLE-REAL en MI-PUNTERO
SET MI-PUNTERO TO ADDRESS OF VARIABLE-REAL.
```

B. Mapear la [[Divisiones/data-division/linkage-section]] (El uso más potente):

```cobol
*> Supongamos que recibimos una dirección de memoria desde otro programa
SET ADDRESS OF ESTRUCTURA-REMOTA TO MI-PUNTERO.

*> Ahora, si accedes a CAMPO-1, estarás leyendo directamente
*> lo que hay en la memoria a la que apunta MI-PUNTERO.
```

4. Paso de parámetros: BY REFERENCE vs BY VALUE.

Cuando haces un **CALL** entre programas, los punteros actúan tras bambalinas:

- **BY REFERENCE (Por defecto):** Le pasas al programa hijo el **puntero** de tu variable. Si el hijo modifica el dato, **tu variable original cambia**. Es lo más común en #Cobol [[100-projects/Dominar Cobol]]
- **BY VALUE:** Le pasas una copia del dato. El programa hijo no sabe dónde está tu variable original.
- **BY POINTER:** Le pasas explícitamente una variable que definiste como **USAGE IS POINTER**

5. Memoria dinámica: ALLOCATE y FREE

En versiones modernas de #Cobol se puede pedir memoria al sistema mientras el prgrama corre, igual que un **new** en #Java o **malloc** en #C

```cobol
*> Pedimos memoria para una estructura


*> Usamos la estructura...

*> Liberamos la memoria para evitar fugas (Memory Leaks)
FREE ADDRESS OF ESTRUCTURA-REMOTA.
```

6. ¿Por qué es vital?

- **Tramas de EntireX**: Muchas veces, el middleware no sabe qué tan grande es la respuesta que enviará [[#WebLogic]]. Envía un puntero al principio de la trama y el tamaño. Tu **COBOL** debe usar **SET ADDRESS OF** para poder leer esos datos.

# **Eficiencia:** Si tienes una tabla enorme de 100MB con movimientos bancarios, es mucho más rápido pasar un puntero (4 bytes) que copiar los 100MB de un programa a otro mediante un **CALL** [[CallYExit]]

# **Integración con C/Java:** Los objetos en [[#Java]] son esencialmente punteros. Cuando [[#EntireX]] hace el "mashalling" (traducción), los punteros en **Cobol** son el puente para entender dónde empiezan los datos que vienen de la [[#JVM]] de [[#WebLogic]]
