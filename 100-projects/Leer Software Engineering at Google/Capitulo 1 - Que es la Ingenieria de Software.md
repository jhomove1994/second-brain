---
id: Capitulo 1 - Que es la Ingenieria de Software
aliases:
  - Capitulo 1 SE Google
  - What is Software Engineering?
tags:
  - software-engineering
  - lectura
  - capitulo
  - fundamentos
  - arquitectura
  - google
---

# Capítulo 1 – What is Software Engineering?

## 🔍 Idea principal

La ingenieria de software se diferencia de la programacion por su enfoque en el tiempo, la escala y los compromisos necesarios para mantener el software a traves del tiempo. No es solo escribir codigo, sino de poder evolucionar el software y mantenerse en el futuro, en equipo y con multiples versiones.

## 🧠 Conceptos clave

- **Diferecias entre programacion y SE**: el tiempo (duracion), la escala (numero de personas y lineas de codigo) y las contrapartidas (compromisos tecnicos que cambian con el tiempo).
- **Sostenibilidad**: El software es sostenible si se puede adaptar a cambios importantes (tecnologicos, de negocio, etc.) sin perder su calidad y mantenibilidad.)
- **Trabajo en equipo**: La programacion puede ser individual, pero la ingenieria de software es colaborativa por naturaleza.
- **Costo de escalado**: El rol de ingeniero es minimizar el costo de crecer el software y su equipo.
- **Ley de Hyrum**: "<<Con un numero suficiente de usuarios de una API, no importa lo que prometa en el contrato: todos los comportamientos observables de su sistema dependeran de alguien>>"

## 💬 Citas destacadas

> "Tres diferencias criticas entre la programacion y la ingenieria de software: el tiempo, la escala y las contrapartidas que entran en juego."
> "El proyecto es sostenible si, durante el tiempo de vida util previsto del software, somos capaces de reaccionar ante cualquier cambio importante que se produzca, ya sea por motivos tecnicos, o empresariales."
> "La diferencia entre la ingenieria de software y la programacion es una cuestion tanto de tiempo como de personas."
> "En que se diferencia la programacion a corto plazo de generacion de codigo con un tiempo de vida util esperado mucho mas largo?, Con el paso del tiempo, debemos ser mucho mas conscientes de la diferencia entre <<funciona>> y <<se puede mantener>>."
> "Para cualquier proyecto para el que no hemos planeado actualizaciones desde el principio, esa transicion es, probablemente, muy penosa por tres razones:

> > > - Llevamos a cabo una tarea que aun no se ha realizado para este proyecto; se han incorporado mas supuestos que estaban ocultos.
> > > - Es poco probable que los ingenieros que intentan realizar la actualizacion tengan experiencia en este tipo de tareas.
> > > - El tamanio de la actualizacion suele ser mayor de lo habitual, y se realizan de una sola vez actualizaciones correspondientes a varios anios, en lugar de llevar a cabo una actualizacion mas gradual. "
> > >   "La ley de Hyrum representa el conocimiento practico de que, incluso con las mejores intenciones, los mejores ingenieros y concienzudas practicas para la revision de codigo, no podemos asumir la adhesion perfecta a los contratos publicados o a las mejores practicas."
> > >   La base de codigo en si, tambien debe escalar. Si su sistema de compilacion o un sistema de control de versiones escala con el tiempo por encima de una funcion lineal, tal vez como resultado del crecimiento y de un historial de registros de cambios cada vez mayor, podria llegar a un punto en el que, sencillamente, no pueda continuar. Preguntas como <<Cuanto tiempo se tarda en hacer una compilacion completa?>>, <<Cuanto tiempo se tarda en obtener una copia nueva del repositorio?>> o <<Cuanto costara actualizar a una nueva version de lenguaje>> no se supervisan de forma activa y cambian con un ritmo lento.

> Se dice que es <<programacion si "inteligente" es un cumplido, pero es ingenieria de software si "inteligente" es una acusacion>>
> La base de codigo de su organizacion es sostenible cuando puede cambiar todas las cosas que debe cambiar, de forma segura, y puede hacerlo durante todo el tiempo de vida util de la base de codigo.
> Si el coste de computo para su cluster de prueba crece por encima de una funcion lineal, consumiendo mas recursos de computo por persona cada trimestre, esta en un camino insostenible y necesita hacer cambios rapidamente.

## 🔁 Conexiones

- Me recordo mi primer proyecto de software, enfocado en crearcion de encuestas dinamicas para el instituto de turismo de Bogota, debido a la baja experiencia, y a los tiempos cortos, el software se volvio insostenible a largo plazo, y no se pudo escalar para nuevos requerimientos.

## 🛠 Aplicaciones prácticas

- Pensar en cada modulo como algo que debe ser entendible y mantenible a largo plazo, no solo como una solucion rapida.
- Documentar decisiones tecnicas mas alla de lo funcional (ej. Por que usamos tal patron?)
- En revisiones de codigo, no solo mirar si "funciona" sino si sera facil de mantener.
- Establecer un checklist en mi flujo de trabajo que pregunte: ""

## ❓ Preguntas / dudas

- Que tipos de contrapartidas son mas comunes en proyectos reales?
- Como se gestionan eses compromisos en equipos pequenios?

## 🧭 Reflexión personal

- Muchas veces en proyectos que se desarrollan por la necesidad de entregar un MVP rapido, no se le da el tiempo necesario para analizar y planificar el software a largo plazo, complicando la escalabilidad del mismo a futuro, y terminando en Software ilegible y poco mantenible.
- Aveces se realizan desarrollos que pueden parecer modernos, con las ultimas caracteristicas de un framework, pero no se piensa a futuro, si es mantenible, si es entendible por alguien diferente al autor.
