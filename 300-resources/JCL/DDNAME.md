---
id: DBNAME
aliases:
  -  #DBNAME
tags:
  -  #JCL
---

## ¿Qué es DDNAME?

DDNAME(Data Definition Name):

> El nombre lógico que un programa ( COBOL, SORT, etc.) usa para referirse a un archivo.

Ejemplo:

```cobol
//PAGOS DD  DSN=BANCO.SEGUROS.PAGOS,DISP=SHR
```

- PAGOS -> DDNAME
- BANCO.SEGUROS.PAGOS -> nombre físico (DSN)

## ¿Qué es DSN?

DSN (Data Set Name):

> El nombre físico real del archivo en el mainframe.

Ejemplo:
BANCO.SEGUROS.PAGOS

Seria algo como:

- /data/banco/seguros/pagos.dat en Linux
