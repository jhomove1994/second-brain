---
id: Filler
aliases:
  -  #filler
  -  #FILLER
tags:
  -  #filler
  -  #sentencias
  -  #boda
---

# FILLER

Es un campo que no tiene nombre propio. Se utiliza para crear espacios o separaciones dentro de un registro que el programa no necesita manipular individualmente.

- **Propósito:** Rellenar espacios en blanco para que un registro tenga una longitud fija ( por ejemplo, que siempre mida 80 carácteres).

- **Restricción con #INITIALIZE:** Como el **#Initialize** busca los **nombres** de las variables para saber qué limpiar, y el **FILLER** técnicamente "no tiene nombre" ( es una palabra reservadad), el comando **lo ignora por completo**. Los datos que estén en un **FILLER** se quedarán tal cual estaban.

Cuando envías información a través de **#EntireX**, los datos no viajan como un objeto **JSON** (con etiquetas como {"nombre": "Juan"}). Viajan como una **trama plana**: una tira larga de caracteres donde cada dato se reconoce por su posición.

**El problema del "Alineamiento"**:

A veces, el sistema receptor (como una base de datos o un servicio en WebLogic) exige que ciertos datos empiecen en una posición específica (por ejemplo, que el Saldo siempre empiece en la columna 30).

Si tus datos actuales no llegan a la columna 30, usas un **FILLER** para "empujar" el siguiente campo.

**Ejemplo de una trama de respuesta:**

Imagina que WebLogic espera este formato exacto de 40 caracteres:

1. **ID**: 5 caracteres
2. **Espacio en blanco:** 5 caracteres (separador)
3. **Nombre:** 20 caracteres
4. **Saldo:** 10 caracteres

Así se define en COBOL:

```cobol
01 TRAMA-SALIDA.
  05 ID-CLIENTE     PIC 9(05).
  05 FILLER         PIC X(05) VALUE SPACES. *> Esto separa los campos.
  05 NOMBRE-CLI     PIC X(20).
  05 SALDO-CLI      PIC 9(08)V99.
```
