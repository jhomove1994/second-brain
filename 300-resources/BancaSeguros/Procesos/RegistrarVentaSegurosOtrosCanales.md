---
id: Registrar Venta de Seguros por otros Canales
aliases:
  -  #procesos #VentaSegurosOtrosCanales
tags:
  -  #bancaseguros
  -  #conceptos
---

# VENTA DE SEGUROS POR OTROS CANALES

[[300-resources/BancaSeguros/BancaSeguros]]

```mermaid
sequenceDiagram
  participant Cardif
  participant GAW
  participant CAREX5
  participant JCL
  Cardif->>GAW: Envía archivo 070C007PS660
  GAW->>CAREX5: Valida la estructura del archivo
  CAREX5->>CAREX5: Mapea y da estructura
  CAREX5->>JCL: Envía la data a GEJ727S
```
