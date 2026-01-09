---
id: String
aliases:
  -  #String #STRING
tags:
  -  #cobol
  -  #sentencias
  -  #string
---

# STRING

Es el "pegamento" de [[100-projects/Dominar Cobol]]. Sirve para concatenar varias piezas de texto, variables o literales en una sola cadena de salida.

Es mucho más potente que un simple [[Move]] porque permite controlar exactamente hasta dónde copiar cada pieza y qué hacer si la cadena de destino se llena.

1. Estructura básica

A diferencia de otros lenguajes donde se usa el signo +, en **COBOL** se usa una estructura por bloques.

```cobol
      STRING variable-1 DELIMITED BY " "
             variable-2 DELIMITED BY SIZE
             " literal " DELIMITED BY SIZE
        INTO variable-destino
        WITH POINTER variable-puntero
        ON OVERFLOW
            DISPLAY "ERROR: LA CADENA SE DESBORDO"
      END-STRING.
```

2. Conceptos clave

A. DELIMITED BY (El limitador): Es lo que le dice a **COBOL** cuándo dejar de copiar de la variable de origen:

- DELIMITED BY SIZE: Copia **toda** la variable, incluyendo los espacios al final. - DELIMITED BY " " (o cualquier carácter) : Copia solo hasta que se encuentre el primer espacio o carácter especificado (muy útil para nombres).
- DELIMITED BY SIZE con literales: Se usa para asegurar que todo el texto entre comillas pase a la salida.

B. WITH POINTER (La posición de escritura): Es una variable numérica que le indica a **COBOL** en qué posición de la variable-destino debe empezar a escribir.

- Importante: Se debe inicializar en 1 antes del STRING.
- Al terminar el STRING, la variable queda apuntando a la siguiente posición libre, lo que permite ir "armando" la frase en pasos.

3. Ejemplo completo: Formateo de respuesta para WebLogic

Suponiendo que se recibe datos de un cliente y se necesita armar una sola trama de texto para enviarla de vuelta a mi servidor.

```cobol
IDENTIFICATION DIVISION.
PROGRAM-ID. EJEMPLO-STRING.

DATA DIVISION.
WORKING-STORAGE SECTION.
01 NOMBRE       PIC X(10) VALUE "JUAN      ".
01 APELLIDO     PIC X(10) VALUE "PEREZ     ".
01 ID-CLIENTE   PIC 9(05) VALUE 12345.
01 TRAMA-SALIDA PIC X(40) VALUE SPACES.
01 POSICION     PIC 9(02).

PROCEDURE DIVISION.
  MOVE 1 TO POSICION.

  STRING "CLI:"         DELIMITED BY SIZE
          ID-CLIENTE    DELIMITED BY SIZE
          "-"           DELIMITED BY SIZE
          NOMBRE        DELIMITED BY " "
          " "           DELIMITED BY SIZE
          APELLIDO      DELIMITED BY " "
    INTO TRAMA-SALIDA
    WITH POINTER POSICION
  END-STRING.

  DISPLAY "TRAMA ARMADA: [" TRAMA-SALIDA "]".
  *> Resultado: [CLI:12345-JUAN PEREZ                   ]
  STOP RUN.
```

4. Ejemplos de uso

- **Construcción de URLs o Paths:** Si el programa **COBOL** necesita llamar a un servicio externo o buscar un archivo cuyo nombre depende de la fecha, usa **STRING** para unir el prefijo, la fecha y la extensión.
- **Mensajería**: Cuando se devuelve un error, se suele usar **STRING** para unir el código de error con una descripción dinámica: "ERROR:" + COD-ERR + " EN FECHA " + FECHA.
- **Limpieza de datos:** Java suele preferir cadenas sin espacios de relleno (trailing spaces), el uso de DELIMITED BY " " es tu mejor aliado para que el String llegue "limpio" al otro lado.

5. Reglas de ORO del STRING

- **No limpia el destino:** El STRING no borra lo que ya había en la variable destino. Escribe encima, por eso es buena práctica hacer un MOVE SPACES TO DESTINO antes.

- **Truncamiento silencioso:** Si la cadena de salida es muy pequeña, **COBOL** corta el texto y sigue como si nada, a menos que uses la cláusula **ON OVERFLOW** para capturar el error.
- **Numéricos:** Para usar un número en un **STRING**, este debe ser **desempaquetado** (no puede ser **COMP-3**). Si es necesario, se mueve primero a una variable PIC X o PIC 9 normal.
