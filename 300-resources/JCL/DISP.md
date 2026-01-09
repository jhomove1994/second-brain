---
id: DISP
aliases:
  -  #disp
tags:
  -  #DISP
  -  #JCL
---

## ¿Qué es DISP?

DISP (Disposition)

> Indica que pasa con el archivo antes, durante y después del STEP.

Forma más común:

```jcl
DISP=(ESTADO,FIN-OK,FIN-ERROR)
```

- DISP=SHR

  - El archivo ya existe
  - Se puede compartir
  - Normalmente es entrada

- DISP=OLD

  - El archivo existe
  - Exclusivo
  - Lectura/escritura

- DISP=(NEW,CATLG,DELETE)
  - NEW -> El archivo se crea
  - CATLG -> Si todo va bien, se guarda
  - DELETE -> Si falla, se borra
    > Usado para archivos de salida

## ¿Qué es un Dataset?

En banca, un dataset es:

> Un activo crítico, auditable, versionado y regulado que contiene información financiera.

Ejemplo reales:

- Movimientos díarios
- Pagos de primas
- Archivos contables
- Históricos legales (años)

> [!WARNING] Un dataset:
>
> - No se puede perder
> - No se puede sobreescribir sin control
> - Debe poder reprocesarse

## Tipos básicos de datasets

- Input (Entrada)

  - Ya existe
  - Solo se lee
  - Viene de otro sistema o proceso

  ```jcl
  DISP=SHR
  ```

- Output (Salida Nueva)
  - No existe
  - Se crea en el proceso
  - Si falla, se borra

```jcl
DISP=(NEW,CATLG,DELETE)
```

- Work/Temporal
  - Se usa solo durante el JOB
  - No debe quedar guardado

```jcl
DISP=(NEW,DELETE,DELETE)
```

## Errores de DISP que tumban producción

> [!ERROR] Usar DIPS=OLD en un archivo de entrada
> DISP=OLD
> ¿Qué pasa?
>
> - Bloquea el archivo
> - Otros procesos fallan
> - Cadena batch se cae

> [!ERROR] Usar DISP=SHR en salida
> DISP=SHR
> ¿Qué pasa?
>
> - Se escribe sobre archivo existente
> - Datos mezclados
> - Información corrupta

> [!ERROR] Olvidar DELETE en error
> DISP=(NEW,CATLG)
> ¿Qué pasa?
>
> - Archivo incompleto queda guardado
> - Siguiente proceso lo usa
> - Error cascada

> [!SUCCESS] Regla de oro

- Nunca escribas directamente sobre un histórico
- Siempre genera uno nuevo y luego consolida
