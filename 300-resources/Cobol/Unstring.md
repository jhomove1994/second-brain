---
id: String
aliases:
  -  #String #STRING
tags:
  -  #cobol
  -  #sentencias
  -  #string
---

# UNSTRING

Sirve para desenglosar o separar una cadena de texto larga en viaras variables más pequeñas, basándose en un delimitador (como una coma, un espacio o un guión).

1. Estructura Básica
   El **UNSTRING** busca los delimitadores y "reparte" los trozos entre las variables que se le indiquen.

```cobol
   UNSTRING variable-origien
     DELIMITED BY "," or ";" OR ALL "*"
     INTO  variable-destino-1
           variable-destino-2
           variable-destino-3
     WITH POINTER posicion-inicial
     TALLYING IN  contador-campos
     ON OVERFLOW
       DISPLAY "ERROR: SOBRARON DATOS EN LA CADENA"
   END-UNSTRING.
```

2. Conceptos clave

- **DELIMITED BY:** Se puede definir varios delimitadores. El uso de **ALL** (ej. ALL "\*") es muy útil porque trata varios asteriscos seguidos como si fueran uno solo, evitando que queden campos vacíos entre ellos.
- **INTO:** Aquí se pone la lista de variables donde se quiere recibir los pedazos.
- **TALLYING IN:** Cuenta cuántos campos logró separar. Si esperas 3 datos y esta variable devuelve 2, sabes que la trama venía incompleta.
- **WITH POINTER:** Igual que en el [[String]], indica desde qué posición de la cadena de origen empezar a cortar.

3. Ejemplo completo: Procesando una trama.

Suponiendo que llega los datos de un empleado en una sola tira separada por comas: "101,JUAN PEREZ,DESARROLLADOR"

```cobol
IDENTIFICATION DIVISION.
PROGRAM-ID EJEMPLO-UNSTRING.

DATA DIVISION.
WORKING-STORAGE SECTION.
01 TRAMA-RECIBIDA   PIC X(40) VALUE "101,JUAN PEREZ,DESARROLLADOR".
01 ID-EMP           PIC 9(03).
01 NOMBRE-EMP       PIC X(15).
01 PUESTO-EMP       PIC X(15).
01 CONTADOR-CAMPOS  PIC 9(02) VALUE 0.

PROCEDURE DIVISION.
    UNSTRING TRAMA-RECIBIDA
      DELIMITED BY ","
      INTO  ID-EMP
            NOMBRE-EMP
            PUESTO-EMP
      TALLYING IN CONTADOR-CAMPOS
    END-UNSTRING.

  DISPLAY "CAMPOS PROCESADOS: " CONTADOR-CAMPOS.
  DISPLAY "ID:      " ID-EMP.
  DISPLAY "NOMBRE:  " NOMBRE-EMP.
  DISPLAY "PUESTO:  " PUESTO-EMP.
  STOP RUN.
```

4. Cláusulas Especiales Avanzadas

El **UNSTRING** permite un nivel de detalle impresionante con estas dos cláusulas que se ponen después de cada variable de destino:

- **DELIMITER IN:** Dice exactamente qué carácter se usó para separar ese campo específico (útil si usas varios delimitadores como **,** y **;**).
- **COUNT IN:** Te dice cuántos caracteres media el trozo que se acaba de extraer (antes de rellenar con espacios)

Ejemplo:

```cobol
UNSTRING TRAMA INTO VARIABLE-1 DELIMITER IN CUAL-DELIM COUNT IN CUANTOS-CARACT.
```

Es muy común que se necesite enviar de vuelta la longitud exacta de los campos o procesar cadenas donde los espacios al final son importantes.

- **Validaciones exactas:** Si un campo "ID" debe tener exactamente 8 caracteres, usas **COUNT IN** y luego preguntas **IF CUANTOS NOT = 8**. Es mucho más rápido que analizar el contenido del campo.
- **Optimización de base de datos:** Si se va a insertar ese dato en una tabla de **DB2** que tiene una columna **VARCHAR**, necesitas pasarle la longitud real para que no guarde espacios innecesarios.

- **Sub-stringing posterior:** Si después se necesita manipular solo una parte del dato extraido, ya se tiene el índice final gracias al valor guardado en la variable de **COUNT IN**.
