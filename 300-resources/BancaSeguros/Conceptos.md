---
id: Conceptos
aliases:
  -  #conceptos
tags:
  -  #bancaseguros
  -  #conceptos
---

## Micrositio

No es una pagina web embebida, es una pieza de software que es de la aseguradora pero esta alojada en la infra de banco. (como una libreria) todo esto se maneja en soluciones de oficina.

## WebView

Se integra un sitio web a la infra del banco, que esta alojado en los servidores de la aseguradora. Esta montado el 100% en cuentas y tarjetas. Como productos modulares, dimensiones financiera (empleados e independientes), salud y se esta trabajando en la dimension de vida.

> [!WARNING]
> para credito esta soluciones de oficina y aplicativos moviles.

> [!NOTE]
> Investigar las reglas de negocio para cada producto.

# Acrónimos

- GE: Multi-aplicación
- KL: Crédito
- ER: Canales socio
- KU: Cartera

## Acrónimos de archivos

- AVPW: AV = AVVILLAS - P = Environment(Puede ser D, T) - W = Work
  > [!NOTE] Estos se usan porque se pueden manipular y destruir
- AVPL: L = Línea
  > [!WARNING] Estos archivos son delicados Estos archivos son delicados
- AVPB: B = Batch

- E: entrada
- SS: salida

## Cardif

Es una intermediaria, que se encarga de los desarrollos de la aseguradora.

## GAW (Go Any Way)

- Carpeta de intercambio de archivos entre el banco y proveedores, en este caso (Cardif)

## Environments

PAUNA (desarrollo)
PAYA (QA)
CAREX (PRODUCCIÓN)

## ETL

Significa Extracción, Transformación y Carga, es un proceso fundamental para mover dataos de un lugar a otro (como de sistemas operativos a un almacén de datos).

## Estandar de nombres de programas

- K: Es un CICS
- F o Y: Copy
- J: JCL
- B: BATCH
- E: TWS Disparador
- S2324DAA: PARMLIB

Ejemplo de identificador: GEE727S
GE: Multi-aplicación
E: TWS Disparador (en este caso una ETL)
S: Significa que corre semanal
D: Significa que corre díario
M: Significa que corre mensual
