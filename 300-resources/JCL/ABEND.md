---
id: Abend
aliases:
  - #Abend
tags:
  - #abend
  - #JCL
---

# ¿Qué es un Return Code (RC)?

Un Return Code es:

> Un número que indica cómo terminó un STEP.

Cada STEP devuelve un RC.

Ejemplo:

- RC=0 -> Todo OK
- RC>0 -> Algo pasó

Escala típica de RC:

| RC | Significado |
|:---- |:----|
|0| Éxito total |
|4| Advertencia |
|8| Error |
|12+| Error grave |

> [!WARNING]
> RC != ABEND

# ¿Qué es un ABEND?

ABEND = Abnormal End

> Una terminación anormal de un STEP o JOB
> Causas típicas:

- Archivo no existe
- DDNAME mal escrito
- Error lógico en COBOL
- Espacio insuficiente

-> En banca:

- Un ABEND detiene el proceso
- Se activa soporte
- Puede requerir reproceso

## Comparación RC vs ABEND

| Concepto     | RC        | ABEND               |
| :----------- | :-------- | :------------------ |
| Tipo         | Resultado | Fallo               |
| Número       | Sí        | No(código de ABEND) |
| Controlable  | Sí        | No directamente     |
| Continua JOB | Depende   | No                  |

## ¿Quién decide el RC?

Normalmente:

- El programa COBOL
- O la utilidad (SORT, etc)
  En COBOL:

```cobol
MOVE 8 TO RETURN-CODE.
```

-> El programador decide sí:

- Algo es advertencia
- Algo es error
- Algo es crítico

## Control de flujo con COND

```jcl
//STEP01 EXEC PGM=VALIDA
//STEP02 EXEC PGM=PROCESA,COND=(4,LT)
```

Significa:
- STEP02 se ejecuta solo si STEP01 tuvo RC < 4

### Como leer COND

| Operador | Significado |
| :-----| :----- |
| LT | Menor que |
| LE | Menos o igual |
| EQ | Igual |
| GT | Mayor que |
| GE | Mayor o igual |
