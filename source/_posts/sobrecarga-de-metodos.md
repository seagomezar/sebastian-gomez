---
title: Sobrecarga de métodos
tags:
  - Java
  - Programación orientada a objetos.
url: 285.html
id: 285
categories:
  - Java
date: 2015-10-28 14:45:21
---

La sobrecarga de métodos es una herramienta que nos da la programación orientada objetos, ya que nos permite modelar y definir el comportamiento de los objetos como queramos, es decir de acuerdo a unos parámetros de entrada ejecutar uno u otro método. Concretamente en Java es posible sobrecargar métodos, es decir, definir dos o más dentro de la misma clase, que compartan nombre pero que las declaraciones de sus parámetros sean diferentes; la sobre carga es una forma de polimorfismo ya que en determinado momento un objeto puede asumir uno u otro comportamiento. <!-- more -->

Cuando se llaman los métodos sobrecargados, el compilador determina cuál es el método invocado basándose en la cantidad y tipo de argumentos pasados; por consiguiente, los métodos sobrecargados deben diferir en números y tipo de parámetros. Cuando Java encuentra una llamada a un método sobrecargado, ejecuta la versión del que tiene parámetros (cantidad y tipo) que coinciden con los argumentos utilizados en la llamada.

Java diferencia los métodos sobrecargados con base en el número y tipo de argumentos que tiene el método y no por el tipo que devuelve.

También existe la sobrecarga de constructores: Cuando en una clase existen constructores múltiples, se dice que hay sobrecarga de constructores.

**Ejemplo**

Se define la clase Sobrecarga con tres métodos de nombre test sobrecargados, diferenciándose entre ellos por la cantidad/tipo de los parámetros main () llama a cada uno de ellos.

El método test () se sobrecargó tres veces; la primera versión no tiene parámetros; la segunda, un entero y la tercera dos enteros.

![sobrecarga1](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/sobrecarga1-300x222.png) ![sobrecarga2](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/sobrecarga2-300x190.png) 

La ejecución da lugar a esta salida: 
![sobrecarga3](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/sobrecarga3-300x55.png) 

Listo, hemos terminado nuestra aplicación orientada a objetos y aplicando correctamente el concepto de herencia. Espero que este tutorial les haya sido de mas ayuda, recuerda “la práctica hace la perfección”, también si quieres aprender mas sobre el tema te invito a que veas estos posts: [Herencia y polimorfismo.]

(http://www.sebastian-gomez.com/java/herencia-y-polimorfismo/) [Herencia con un ejemplo.](http://www.sebastian-gomez.com/java/entendiendo-la-herencia-con-un-ejemplo/) [Interfaces en Java.](http://www.sebastian-gomez.com/java/creacion-y-personalizacion-de-interfaces-graficas-usando-netbeans/) Escríbeme si tienes alguna duda y no olvides si te ha gustado este tutorial compartelo!