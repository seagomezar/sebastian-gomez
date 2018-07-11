---
title: Solución al problema de las torres de Hanoi usando PDDL
tags:
  - Inteligencia Artificial
  - PDDL.
  - Planificación en Inteligencia Artificial
url: 164.html
id: 164
categories:
  - Inteligencia Artificial
date: 2015-10-18 20:49:44
---

Las torres de hanoi ([Explicacion del juego](https://es.wikipedia.org/wiki/Torres_de_Han%C3%B3i)), es un juego-problema clásico muy conocido en las ciencias de la computación dada su complejidad. Una de las ramas en las que cobra interés dicho problema es la planificación en inteligencia artificial dada la complejidad de este problema, palabras más palabras menos la planificación en inteligencia artificial es "El proceso de búsqueda y articulación de una secuencia de acciones que permitan alcanzar un objetivo" (vea más [aquí](http://www.davidam.com/docu/aplic-ia/planific.html)). En este pequeño post pretendo ofrecer una representación de dicho problema en un lenguaje de planificación llamado [PDDL](https://en.wikipedia.org/wiki/Planning_Domain_Definition_Language) (**Planning Domain Definition Language**) y como esta representación nos ayuda a obtener soluciones a dicho problema usando esta representación en el planificador [LPG](https://courses.cs.washington.edu/courses/cse574/03sp/planners.html). <!-- more -->

### **Las Reglas del Juego**

Un disco de mayor tamaño no puede ir encima de un disco de menor tamaño. Los discos deben ser apilados de mayor a menos, comenzando desde la base de la torre. Los discos se deben pasar de la torre uno (1) a la torre tres (3). Este juego puede ser jugado en [aquí](http://www.juegosparalistos.es/Juegos_flash/hanoi.html).

**Planteamiento del Problema**
------------------------------

Una visión general del juego se puede ver de la siguiente manera: [![planning-1](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/planning-1-300x203.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/planning-1.png)  

### **Definición de Objects**

¿Cuáles son los objetos que usaremos en el ejercicio? Tres discos y Tres torres, es decir: **disco1, disco2, disco3, torre1, torre2, torre3** Se tendrá un brazo mecánico el cual manipulara los discos**.**

### **Estado inicial**

El estado inicial de donde parte el problema luce de la siguiente manera: [![planning-2](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/planning-2-300x114.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/planning-2.png)   Y dicho esto es hora de empezar a definir en lenguaje PDDL cada uno de los elementos que compondrán nuestro problema: (Problema en PDDL quiere decir la especificación de como esta el mundo antes de empezar estado inicial, y como queremos que quede luego de ejecutar el plan es decir estado final). Empezamos con la definición de nuestro problema: Se define la propiedad del objeto, es decir, el objeto “disco1”, se especifica que es un Disco, esto mismo se hace con los demás objetos [![planning-3](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/planning-3.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/planning-3.png) Se debe definir que hay discos de mayor tamaño que otros, con el fin de cumplir una de las reglas de juego. [![planning-4](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/planning-4.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/planning-4.png) Se establece el orden de los discos acorde a su tamaño [![planning-5](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/planning-5.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/planning-5.png) Se define que el disco de la parte superior quedo libre y que el brazo mecánico esta libre. [![planning-6](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/planning-6.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/planning-6.png) Se establece que las torres dos y tres están vacías. [![planning-7](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/planning-7.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/planning-7.png) Así nuestro estado inicial completo quedaría definido así:

```
(:domain torres)
(:objects disco1 disco2 disco3 torre1 torre2 torre3)
(:init 
(esdisco disco1)
(esdisco disco2)
(esdisco disco3)
(estorre torre1)
(estorre torre2)
(estorre torre3)
(mayor disco3 disco2)
(mayor disco3 disco1)
(mayor disco2 disco1)
(torrevacia torre2)
(torrevacia torre3)
(entorre disco3 torre1)
(encima disco2 disco3)
(encima disco1 disco2)
(libre disco1)
(brazolibre)
)
```

Es tiempo de definir nuestro objetivo o estado final, una representación gráfica de ese se muestra a continuación: [![planning-8](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/planning-8-300x115.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/planning-8.png) Se define el estado de los discos y en la torre que va a quedar, además definir que el disco superior queda libre y que el brazo queda libre.

```
(:goal (and
(entorre disco3 torre3)
(encima disco2 disco3)
(encima disco1 disco2)
(libre disco1)
(brazolibre)
)
```

Una vez representado nuestro estado inicial y estado final, esto es guardado en un archivo .pddl usualmente llamado problem.pddl (Porque usualmente esto es lo que se quiere resolver). Ahora procederemos a hablar de nuestro dominio o representacion de las acciones que componen el mundo del problema de las torres de hanoi:

**Planteamiento del dominio** Definición de los predicados que serán usados para representar el estado del mundo: **Encima**: recibe dos variables que sean discos. **Entorre**: recibe una variable que sea disco y otra que sea torre. **Libre**: Pone en estado libre la variable que se le asigne. **Sujeto**: Al usar el brazo mecánico, el objeto recibido queda sujeto. **Brazolibre**: El brazo mecánico queda en estado libre. **Mayor**: Define que la primera variable es mayor que la segunda variable. **Estorre**: Define que la variable que recibe es una torre. **Esdisco**: Define que la variable que recibe es un disco. **Torrevacia**: Define que la torre queda vacía.

```
(:predicates 
(encima ?d1 ?d2)
(entorre ?d ?t)
(libre ?d)
(sujeto ?d)
(brazolibre)
(mayor ?d1 ?d2)
(estorre ?t)
(esdisco ?d)
(torrevacia ?t)
)
```

Definición de las acciones que cambiaran el estado del mundo:

### Desapilar

**Parámetros:**

Recibe dos variables tipo disco.

**Precondiciones:**

Validar que las dos variables que se reciben **si sean discos.**

La segunda variable debe ser **mayor** a la primera variable.

La primera variable debe estar **encima** de la segunda variable.

La **primera Variable** debe estar **libre**.

El **brazo mecánico** debe estar **libre**.

**Efecto:**

La primera variable pasa a tener estado **sujeto**.

La primera variable **deja** de estar **encima** de la segunda variable.

La **primera Variable** no está **libre**.

El **brazo mecánico** no está **libre**.

```
(:action desapilar
:parameters (?d1 ?d2)
:precondition (and (esdisco ?d1)
(esdisco ?d2)
(mayor ?d2 ?d1)
(encima ?d1 ?d2)
(libre ?d1)
(brazolibre)
)
:effect (and (sujeto ?d1)
(not (encima ?d1 ?d2))
(not (brazolibre))
(not (libre ?d1))
(libre ?d2)
)
)
```

### Coger

**Parámetros:**

Recibe una variable tipo disco y una variable tipo torre.

**Precondiciones:**

**L**a primera variable debe ser **tipo disco**

La segunda variable debe ser **tipo Torre.**

La primera variable debe estar **libre**.

La primera variable debe estar **en la torre** definida en la segunda

El **brazo mecánico** debe estar **libre**.

La torre **no** puede estar **vacía**.

** Efecto:**

La primera variable pasa a tener estado **sujeto**.

La primera variable **deja** de estar **en la torre** de la segunda variable.

La **primera Variable** no está **libre**.

El **brazo mecánico** no está **libre**.

La torre de la segunda variable pasa a estar vacía.

```
(:action coger
:parameters (?d1 ?t1)
:precondition (and (esdisco ?d1)
(estorre ?t1)
(libre ?d1)
(entorre ?d1 ?t1)
(brazolibre)
(not (torrevacia ?t1))
)
:effect (and (sujeto ?d1)
(not (entorre ?d1 ?t1))
(not (brazolibre))
(not (libre ?d1))
(torrevacia ?t1)
)
)
```

### Apilar

**Parámetros:**

Recibe dos variables tipo disco.

**Precondiciones:**

Validar que las dos variables que se reciben **si sean discos.**

La segunda variable debe ser **mayor** a la primera variable.

La primera variable debe estar con estado

La **segunda Variable** debe estar **libre**.

La primera variable **no** debe estar **encima** de la segunda variable.

El **brazo mecánico** **no** está **libre**.

La **primera Variable** no está **libre**.

**Efecto:**

La primera Variable pasa a estar **libre**.

La primera variable **pasa a** estar **encima** de la segunda variable.

El **brazo mecánico** pasa a estar **libre**.

La primera variable pasa a tener estado **no sujeto**.

La **segunda Variable** no está **libre**.

```
(:action apilar
:parameters (?d1 ?d2)
:precondition (and (esdisco ?d1)
(esdisco ?d2)
(mayor ?d2 ?d1)
(sujeto ?d1)
(libre ?d2)
(not (encima ?d1 ?d2))
(not (brazolibre))
(not (libre ?d1))
)
:effect (and (libre ?d1)
(encima ?d1 ?d2)
(brazolibre)
(not (sujeto?d1))
(not (libre ?d2))
)
)
```

### Poner

**Parámetros:**

Recibe una variable tipo disco y una variable tipo torre.

**Precondiciones:**

**L**a primera variable debe ser **tipo disco**

La segunda variable debe ser **tipo Torre.**

La primera variable debe estar en estado **sujeto**.

La primera variable **no** debe estar **en la torre** definida en la segunda variable.

El **brazo mecánico** **no** debe estar **libre**.

La primera variable **no** debe estar **libre**.

La torre debe estar **vacía**.

**Efecto:**

La primera variable **pasa a** estar **en la torre** de la segunda variable.

La **primera Variable** pasa a estar **libre**.

El **brazo mecánico** pasa a estar **libre**.

La primera variable pasa a tener estado **no sujeto**.

La torre de la segunda variable pasa a estar **ocupada**.

```
(:action poner
:parameters (?d1 ?t1)
:precondition (and (esdisco ?d1)
(estorre ?t1)
(sujeto ?d1)
(not (entorre ?d1 ?t1))
(not (brazolibre))
(not (libre ?d1))
(torrevacia ?t1)
)
:effect (and (entorre ?d1 ?t1)
(libre ?d1)
(brazolibre)
(not (sujeto?d1))
(not (torrevacia ?t1)))
)
```

Hecho esto volcamos todo nuestro dominio a un archivo denominado domain.pddl. Ya con nuestros dos archivos domain.pddl y problem.pddl estamos listos para que nuestro planificador nos muestre posibles secuencias de acciones para llegar a la solución de nuestro problema, a continuación presento una de ellas: [![planning-15](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/planning-15-300x208.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/planning-15.png) Es posible también realizar algunas variantes para soportar el problema con 4, 8 o N discos solo es cuestión de jugar un poco con el problem.pddl. Para descargar los archivos [problem.pddl](http://www.sebastian-gomez.com/shared/torresprobl.pddl) o [domain.pddl](http://www.sebastian-gomez.com/shared/torresdom.pddl) haz click sobre sus titulos.