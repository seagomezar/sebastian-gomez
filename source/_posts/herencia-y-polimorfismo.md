---
title: Herencia y Polimorfismo
tags:
  - Java
  - Programación orientada a objetos.
url: 232.html
id: 232
categories:
  - Java
date: 2015-10-22 14:54:31
---

Cuando hablamos de herencia, nos referimos a que una sub clase deriva de una clase superior adoptando todos los atributos y métodos  de la súper clase.
<!--more -->

**Herencia**

La herencia consiste en hacer uso de los atributos o métodos de una clase dentro de otra como si le perteneciera a este mismo. Esto se podría dar en un caso muy exclusivo para poder ahorrar proceso y código a implementar. Por ejemplo podría ser para una serie de empleados que ocupen diferentes cargos pero tienen atributos en común como el nombre, apellido, DNI, etc. Lo cual sería conveniente usar la herencia juntando los datos en común en una misma clase y distribuir clases independientes para los demás datos de los empleados.

Herencias es un pilar muy útil en java como podemos ver para hacértelo más simple te diré otro ejemplo más sutil:

Nuestros padres tienen sus vienes raíces que han construido con el paso del tiempo, claro es posible que ellos tenga sus hijos los cuales ellos serán los herederos de todo lo que tiene papá, adquiriendo así sus genes y dinero (en java heredaríamos sus atributos y métodos).

[![herencia1](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/herencia1-300x191.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/herencia1.png) [![herencia2](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/herencia2-300x225.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/herencia2.png) Ahora vamos a ver como es la sintaxis para definir las super clases (padre) y las sub clases (hija). Cuando vamos a definir  una sub clase  nuestro código sera así; [![herencia3](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/herencia3-300x51.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/herencia3.png)

Extends, es una de las palabras reservadas de java la cual sera la que siempre utilizaras para relacionar y decir cual sera la clase padre (super-clase) que quieres que tome o herede todos sus atributos  e métodos.

Al, hija extender de padre obtiene todos sus atributos y métodos y ten en cuenta que una sub clase no hereda sus constructores si quieres tenerlos tienes que hacer uso de esta palabra reservada en java  “Super” que se encarga de decir que vamos a utilizar el constructor de la super clase y también tiene como utilidad cuando vamos a redefinir un método, para mayor entendimiento te lo ilustrare.

Extends, es una de las palabras reservadas de java la cual sera la que siempre utilizaras para relacionar y decir cual sera la clase padre (super-clase) que quieres que tome o herede todos sus atributos  e métodos.

Al, hija extender de padre obtiene todos sus atributos y métodos y ten en cuenta que una sub clase no hereda sus constructores si quieres tenerlos tienes que hacer uso de esta palabra reservada en java  “Super” que se encarga de decir que vamos a utilizar el constructor de la super clase y también tiene como utilidad cuando vamos a redefinir un método, para mayor entendimiento te lo ilustrare.

[![herencia4](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/herencia4-300x108.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/herencia4.png)

[![herencia5](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/herencia5-300x113.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/herencia5.png)

[![herencia6](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/herencia6-300x90.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/herencia6.png)

Acá hemos evidenciado la funcionalidad de la palabra super.

Bueno ahora te contara algo que tiene java, es que desde que creamos un proyecto para nuestro aplicativo ya estamos presenciando lo de herencia, "como así!" pues es por que todo lo que realizamos en java y todas las super clase y sub clases que establezcamos nosotros como programadores, tenemos que saber que la clase principal (super clase), esta preestablecida por java es object, no la definimos nosotros ni nada pero ya esta incorporada y con todos sus métodos, es algo básico y útil conocer de ella.

Mira la herencia es una de las maravillas y fuerte de la programación orientada a objetos por su alta utilidad y beneficio que tiene para nuestro desarrollo como programadores, a continuación les enseñare otro ejemplo de herencia utilizando el super para que lo repasemos y lo tengamos mas claro (como llamar los construtores del padre).

  [![herencia7](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/herencia7-300x258.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/herencia7.png) [![herencia8](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/herencia8-300x165.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/herencia8.png)

Observas que la clase Taxi e Autobús heredan los atributos y métodos de la super-clase Vehículo, así que gracias a la super clase se ahorrara la necesidad de volver a implementar o copiar el código, solo sera necesario  implementar los atributos que complementan a las sub clases, como _numeroLinecia();_ de taxi y _numero Plazas(); ._

[![herencia9](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/herencia9-300x164.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/herencia9.png)

Observa  los modificadores de acceso para que hagas uso de ellos adecuadamente según las necesidades de la aplicación que estés realizando;

| Modificador     | Clase      | Package    | Subclase   | Todos      |
| --------------  | ---------- | ---------- | ---------- | ---------- |
| Public          | Sí         | Sí         | Sí         | Si         |
| Protected       | Sí         | Sí         | Sí         | No         |
| No especificado | Sí         | Sí         | No         | No         |
| Private         | Sí         | No         | No         | No         |


**Polimorfismo** El Polimorfismo es uno de los 4 pilares de la programación orientada a objetos (POO) junto con la Abstracción, Encapsulación y Herencia. Para entender que es el polimorfismo es muy importante que tengáis bastante claro el concepto de la Herencia. Consiste en la posibilidad de tener métodos con el mismo nombre en distintas clases. Al hablar de métodos en distintas clases nos estamos refiriendo a métodos distintos y por tanto con comportamientos distintos a pesar de que tengan el mismo nombre. El polimorfismo permite poder enviar un mismo mensaje (recordemos que un mensaje es una invocación a un método) a objetos de clases diferentes. Estos objetos recibirán el mismo mensaje pero responderán a él de formas diferentes. Por ejemplo, un mensaje “+” para un objeto entero significaría una suma, mientras que para un objeto String (cadena de caracteres) significaría la concatenación. 

Este tutorial ha sido realizado con la ayuda del estudiante Andres Felipe Vásquez. 

**Referencias** 
- [http://www.edukanda.es/mediatecaweb/data/zip/1305/page_14.htm](http://www.edukanda.es/mediatecaweb/data/zip/1305/page_14.htm) 
- [http://cursos.aiu.edu/Lenguajes%20de%20Programacion%20Orientados%20a%20Objetos/PDF/Tema%204b.pdf](http://cursos.aiu.edu/Lenguajes%20de%20Programacion%20Orientados%20a%20Objetos/PDF/Tema%204b.pdf) 
- [http://www.ifug.ugto.mx/~gerardo/programacion/4%20Java%20desde%20cero.pdf](http://www.ifug.ugto.mx/~gerardo/programacion/4%20Java%20desde%20cero.pdf)