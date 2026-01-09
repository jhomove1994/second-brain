---
id: Estandares y Nombres de Catálogo Procesos Batch
aliases:
  -  #estandares-y-nombres-de-catalaogo-procesos-batch
tags:
  -  #Bancaseguros
---

## COMPILACIÓN PROGRAMAS BATCH

Actualmente el banco **AV VILLAS** cuenta con tres ambientes para hacer el desarrollo, pruebas e instalación de un requerimiento en donde cada uno tiene sus propios estándares para la codificación de nombres de catalogo.

**Donde AV** (AV VILLAS) **D,T,P** (Ambiente) **B** (Batch)

Existe un conjunto de implementaciones funcionales que pueden vincularse en distintos puntos del desarrollo o ejecución, estas son llamadas **librerías** las cuales tienen sus propios estándares según el ambiente a tratar:

- Para guardar los programas fuentes batch:
  DESA.AV01.BATCHLIB
  TEST.AV00.BATCHLIB
  PROD.AV00.BATCHLIB

En donde **DESA** (Ambiente), **AV00** (Consecutivo), **BATCHLIB** (Programas fuentes BATCH)

- Para guardar [[300-resources/JCL/jcl]]
  DESA.AV01.JCLLIB
  TEST.AV00.JCLLIB
  PROD.AV01.JCLLIB

- Para guardar parámetros:
  DESA.AV01.PARMLIB
  DESA.AV00.PARMLIB
  DESA.AV00.PARMLIB

- Para guardar Copies (Definición de estructura de archivos):
  DESA.AV01.COPYLIB
  TEST.AV00.COPYLIB
  PROD.AV00.COPYLIB

- Existen otros tipos de librerías que se utilizan para ejecutar procesos específicos:
  PROD.AV02.JCLLIB -> Procesos de ejecución Esporádico
  PROD.AV70.JCLLIB -> Procesos de ejecución Automáticos

## Nomenclatura Programas Batch

El nombre de los programas batch esta compuesto por:
AP -> Aplicativo
B -> Batch
nnn -> Consecutivo

Las aplicaciones se clasifican en:
AB: Ahorros
GE: Multiaplicativo (puede tener todos los productos)
KD: CDT's
KI: Cartera Individual
KO: Contabilidad

**EJ:** ABB317, GEB500, KDB007, KIB015, KOB130

- Para realizar la compilación de un programa se utiliza el comando **CATBATD** y para ver el spool con el resultado de la compilación se utilizan los comandos **SD.ST**.

- Estos programas no se catalogan.

## Nomenclatura Copys

Los copys son la definición de estructura de los archivos y su nomenclatura es la siguiente:

AP-> Aplicativo
F-> File
nnn-> Consecutivo

ej: **KIF010 (Cartera)**

- Para realizar la compilación de un copy se utiliza la instrucción **CATSOUD**, tampoco se cataloga.

## Nomenclatura JCL's

Los JCL's son declaraciones u órdenes con las que se indica al sistema operativo qué tareas debe realizar y en qué secuencia han de ejecutarse, su sintaxis:

AP -> Aplicativo
J -> Letra que significa JOB
nnn -> Consecutivo
P -> Periocidad

La periocidad puede estar representada en:
D: Día
E: Esporádico
S: Semanal
A: Anual
Q: Quicenal
Z: Semestral
M: Mensual
T: Trimestral

Ej: **ABJ105D, KDJ210M, KOJ165S, GEJ007E**

> [!WARNING] Para los procesos automáticos el consecutivo debe comenzar con la serie 600

- Como el JCL's ejecuta una serie de pasos para realizar una funcionalidad, si queremos catalogar un nombre de un paso dentro del JCL, se haría de la siguiente manera:

```JCL
//Nombre Externo archivo programa
//KD010KS
```

Donde K: Tipo de archivo indexado, S: Tipo de archivo Secuencial
E.S,M: Entrada, Salida, Ambos

## Nomenclatura Archivos

La sintaxis de los archivos es la siguiente:
AAAA.CCC.SSNNCD.TTTTT.DDDDD
Donde:
AAAA -> Ambiente
CCC -> Simila a Catalogo
SS -> Aplicativo
NN -> Numero Secuencia
C -> Numero consecutivo llave alterna
D -> Tipo (Secuencial, indexado)
TTTTT -> Descripción Parte 1
DDDDD -> Descripción Parte 2

Para el Ambiente "AAAA" se define:

- Producción:
  AVPB -> Batch
  AVPW -> Trabajo

> [!NOTE] Para los ambientes Test y Desarrollo se cambia las letras T Y D respectivamente.

Para el parametro de catalogo tenemos los siguientes:

**CRE**: Credito
**ADM**: Administrativo
**AHL**: Ahorros
**MUL**: Multiaplicación
**CAP:** Capacitaciones
**RED:** Redes
**TR1:** Trabajo
**FTP:** FTP

Ej: **AVPB.CAP.AB010S.MSTRO.AHORR - AVTB.MUL.GE270K.CEDUL.PRODU - AVPW.TR!.KD010K,MSTRO.CARTE**

## Compilación de programas CICS

Los programas fuentes CICS se encuentran en las librerías:
DESA.AV01.CICSLIB
TEST.AV01.CICSLIB
PROD.AV01.CICSLIB

### Nomenclatura Programas CICS

AP -> Aplicativo
K ó L -> Objeto haciendo link ó Transacción
nnnn -> Consecutivo

Ej: **AVK00801**

- Para compilar n programa **CICS** se utiliza la instrucción **CATCICSD** y el spoool en donde se encuentra el resultado de la compilación también puede ver con los comando **SD.ST**.
- Si el retorno de la compilación es C0000 ó C0004 se debe realizar catalogación, esta se realiza en el ambiente **CICSDESA** con la instrucción **CEMT S PROG** (nombre del programa) **NE** (New Copy).

- Todos los programas **CICS** y archivos nuevos deben matricularse en alguno de los **CICS** disponibles:
  Desarrollo: CICSDESA
  Test: CICSTES1 -> Calidad
  CICSTES3 -> Formación
  Producción: CICSAPLI -> Solución de Aplicaciones (protegido, desprotegido)
  CICSANA -> Transacciones oficinas, canales electrónicos.

- Para matriculas de programa en desarrollo es necesario solicitarlo por medio de una OP (Orden de proceso), y para los ambientes **TEST** y **PRODUCCION** se debe solicitar por medio de la herramienta **Clear Quest** en la pantalla de matriculas.

## Nomenclatura Copies

AP -> Aplicativo
F ó Y -> File ó áreas de link entre varios CICS
nnn -> Consecutivo

Ej: **KIY010** (Cartera)

## Nomenclatura Archivos

La sintaxis de los archivos es muy parecida a la de los batch:

Donde:

AAAA -> Ambiente
CCC -> Simia a Catalogo
SS -> Aplicativo
NN -> Numero Secuencia
C -> Numero consecutivo llave externa
D -> Tipo (secuencial, indexado)
TTTTT -> Descripción Parte 1
DDDDD -> Descripción Parte 2

Para el Ambiente "AAAA" se define:

- Producción:
  AVPL -> Línea
  Ej: AVPL.CAP.AB010S.MSTRO.AHORR

## Recomendaciones

- Eliminar variables que no se necesiten.
- Para los niveles 88 es recomendable que el prefijo tenga el nombre de la variable y siempre se debe preguntar por el nivel 01 (agrupamiento) cuando se require hacer una funcionalidad.

- Se deben codificar sentencias con sus respectivas sentencias delimitadoras.

  Ej: PERFORM parrafo UNTIL parrafo-fin
  END-PERFORM.

- Existe una rutina llamada **SOC300** la cual es un servicio que genera errores de **abend**. Siempre se debe llamar al principio del programa, para que en caso de ocurrir un error se genere un archivo un correo con el error presentado.
- También existen rutinas de persistencia las cuales son servicios que generan código para realizar diferentes funcionalidades (Consulta, update, delete, etc), es recomendable realizar el llamado a estas rutinas en vez de realizar el read a archivos directamente.
