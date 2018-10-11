---
title: Entendiendo la herencia con un ejemplo
tags:
  - Java
  - Programación orientada a objetos.
url: 250.html
id: 250
categories:
  - Java
date: 2015-10-26 18:45:00
---

En esta guía de ayuda nos disponemos a repasar temas fundamentales en la programación orientada a objetos como lo es la Herencia. Para tal fin, vamos a crear un programa en el entorno de desarrollo Netbeans. <!-- more -->

Para comenzar, vamos a definir que es la herencia, aquí voy a dar una definición coloquial, es decir que voy a definir esto con mis palabras.

-¿Qué es la herencia? à La herencia se utiliza con el fin de ahorrar líneas de código, si en una clase voy a utilizar lo mismo que utilice en otra clase, pues entonces en vez de copiarlo nuevamente, la extiendo y listo, eso es todo.

La estructura es muy sencilla, simplemente la clase a la cual vamos a copiar el código que ya está escrito en otra le agregamos la palabra **extend** y luego el nombre de la clase de la cual se extiende.

Ahora sí, vamos a pensar como programadores, supongamos que de la empresa EnlaceDieselTronic nos contrataron para realizar un software sencillo que en su primera etapa debe mostrarles el nombre de los empleados, la edad, el cargo y el horario de trabajo. Nosotros como programadores arriesgados, decidimos desarrollarlo en el lenguaje de programación Java, para desarrollarlo en ese lenguaje necesitamos un entorno de desarrollo apto para este, y nos decidimos por Netbeans.

Teniendo instalado Java y Netbeans en nuestro equipo, nos disponemos a comenzar el proyecto.

Lo primero que debemos hacer es crear un nuevo proyecto en **Netbeans**, para eso vamos a la pestaña **file** y luego en **New Project.**

[![therencia1](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/therencia1-244x300.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/therencia1.png)

En la pestaña **New project** seleccionamos la opción **Java application** y esta a su vez nos arrojará a una nueva ventana.

[![therencia2](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/therencia2-300x206.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/therencia2.png)

En esa nueva ventana ponemos el nombre con el cual queremos bautizar al proyecto, la ruta en donde queremos almacenarlo, y le damos **finish** para finalizar la creación.

[![therencia3](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/therencia3-300x206.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/therencia3.png)

Ahora, ya con el proyecto creado en Netbeans, es nuestra labor comenzar con el proyecto. Para eso crearemos 3 clases, 1 principal llamada **MAIN** que es la que va a ejecutar el proyecto, 2 secundarias llamadas **PERSONA** y **EMPLEADO.**

Para eso damos clic derecho en el paquete en donde está guardado el proyecto, luego en **New**, y por último en **Java Class.**

[![therencia4](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/therencia4-300x192.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/therencia4.png)

Aquí ya nos disponemos a nombrar las clases que vamos a crear, recordemos que son la clase Persona y la clase Empleado.

[![therencia5](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/therencia5-300x207.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/therencia5.png)

Lo primero que vamos a hacer es crearle atributos a la clase persona, que será la clase básica para el proyecto que le vamos a desarrollar a EnlaceDieselTronic.

La clase Persona será una clase padre, de esta heredará la clase Empleado. Por consiguiente, los atributos de la clase persona deberán ser usados en la clase Empleado.

[![therencia6](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/therencia6-300x126.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/therencia6.png)

Como ya declaramos los atributos Nombre y Edad que son los más generales para una clase padre, nos disponemos a generar el constructor para inicializar estos datos.

[![therencia7](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/therencia7-300x177.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/therencia7.png) Generado el constructor, vamos a retornar un mensaje con los atributos anteriormente generados. [![therencia8](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/therencia8-300x200.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/therencia8.png)

Ahora, nos disponemos a crear la clase empleado, en esta clase necesitamos el atributo **Nombre**, **Edad** y **Cargo**, aquí viene el sentido que tiene la Herencia, ¿Qué sentido tiene que en la clase empleado creemos el atributo **Nombre** y **Edad** sabiendo que en la clase básica ya los tiene declarados?, este es el sentido que tiene la Herencia, ahorrar líneas de código.

En la siguiente imagen mostramos cual es la estructura que se debe seguir para aplicar la herencia.

[![therencia9](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/therencia9-300x117.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/therencia9.png)

Valga la pena aclarar que el error que aquí aparece es debido a que todavía no hemos declarado el constructor.

Ya declarado el constructor y agregados los atributos Cargo y Horario que necesitamos añadir, debería quedar así:

[![therencia10](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/therencia10-300x193.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/therencia10.png)

Ahora nos falta el mensaje que vamos a retornar, debemos tener en cuenta que en la clase Persona ya teníamos un mensaje, y ese mismo debemos añadirlo al que vamos a crear aquí:

[![therencia11](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/therencia11-300x179.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/therencia11.png)

Aquí comenzamos con un concepto antiguo y un nuevo concepto, el **this** y el **super.** El **this** es un apuntador al dato mismo donde este. Por esto, cuando ponemos el **<this.Cargo = Cargo>** o el **<this.Horario = Horario>** estamos apuntando a los atributos **Cargo** y **Horario** en donde estén de la clase.

Ahora, el **Super** lo utilizamos cuando una clase herede de otra, y en la primera **(Clase hija)** defina un método que ya está definido en la segunda **(Clase madre).** El **Super** nos sirve para indicar que nos referimos al método de la clase Madre.

Ya entendido el **Super** y el **This**, y terminadas ambas clases, nos disponemos a crear el **Main**, lo más sencillo por desarrollar.

Aquí creamos el objeto Empleado, no tenemos necesidad de crear el Objeto para la **Clase Persona**, pues los parámetros que allí tenemos están incluidos en la **Clase Empleado.**

[![therencia12](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/therencia12-300x187.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/therencia12.png)

Aquí vemos que nos arroja un error, esto se debe a que no le estamos enviado parámetros.

[![therencia13](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/therencia13-300x192.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/therencia13.png)

Ya al enviarle parámetros el error desaparece.

Lo único que nos falta para finalizar el programa es imprimir el mensaje.

[![therencia14](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/therencia14-300x185.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/therencia14.png)

Aquí en la impresión del mensaje, lo que hacemos es poner la referencia de la clase Empleado que es donde está el mensaje completo (Vale la pena recordad que en la clase Persona también retornamos un mensaje, pero este lo estamos invocando desde la clase Empleado y lo estamos añadiendo al que allí pensamos retornar).

El resultado de la ejecución del programa es el siguiente:

[![therencia15](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/therencia15-300x72.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/therencia15.png)

Listo, hemos terminado nuestra aplicación orientada a objetos y aplicando correctamente el concepto de herencia.

Espero que este tutorial un poco más práctico les haya sido de mas ayuda, recuerda "la práctica hace la perfección", también si quieres aprender mas sobre el tema te invito a que veas [este post](http://www.sebastian-gomez.com/java/herencia-y-polimorfismo/).

Escríbeme si tienes alguna duda y no olvides si te ha gustado este tutorial compartelo!