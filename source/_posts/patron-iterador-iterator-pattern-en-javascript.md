---
title: Entendiendo el patrón iterador "Iterator pattern" en Javascript
tags:
  - Desarrollo Web
  - Javascript
  - Programación Reactiva
url: 546.html
id: 546
thumbnailImage: https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2018/02/javascript-736400_960_720.png?resize=150%2C150&ssl=1
thumbnailImagePosition: left
categories:
  - Javascript
date: 2018-02-07 09:22:45
---

En este post aprenderemos como implementar el patrón iterador en Javascript y en que situaciones lo podemos usar.
<!-- excerpt -->

El patrón iterador es uno de los patrones de diseño de software más usado en Javascript y también uno de los más sencillos. Es un patrón de comportamiento ya que define como se comunican objetos entre si. De él se extienden importantes aplicaciones que pueden ayudar a definir mejores arquitecturas en aplicaciones web, por lo que su uso y estudio es altamente recomendado. En este post aprenderemos como implementarlo en Javascript y en que situaciones lo podemos usar.

[![](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2018/02/3D9EF91B-93C7-4DB4-AC5D-73362F46D236.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2018/02/3D9EF91B-93C7-4DB4-AC5D-73362F46D236.png)

Empecemos por definir este patrón. Básicamente todos hemos usado arrays de esta forma \[1,2,3,4\] y usualmente si queremos recorrerlo nos inclinamos rápidamente por usar una estructura cíclica como "for" o “while” sin embargo esto hace que el código sea un poco mas imperativa y menos declarativa haciendo que no tengamos tanto control con cada dato unitario dentro del arreglo. Desde este punto es donde empieza a cobrar fuerza este patron iterador ya que básicamente lo que nos provee es una manera de recorrer el arreglo de una manera declarativa. El principal principio de este patrón es permitirnos recorrer colecciones de objetos de una manera que podamos decidir cuando queremos el siguiente objeto y cuando no. Para ello obligatoriamente deben existir tres métodos dentro del iterador:

*   first() --> Retorna siempre el primer objeto de la colección.
*   next() --> Retorna el siguiente objeto de la colección si existe.
*   current() --> Retorna el objeto actual de la colección sobre el estamos parado.

Empecemos entonces por definir una clase llamada iterator con estos tres métodos:

{% tabbed_codeblock  %}
  <!-- tab js -->
    class Iterator {
      constructor(collection) {
        this.index = 0;
        this.collection = collection;
      }
      first() {
        return this.collection\[0\];
      }
      next() {
        this.index += 1;
        return this.collection\[this.index\];
      }
      current() {
        return this.collection\[this.index\];
      }
    }
  <!-- endtab -->
{% endtabbed_codeblock %}

Como ves usando ES6 hemos definido una clase que permite hacer operaciones muy básicas sobre el array, pero siempre conservando cual es el índice sobre que esta la colección en todo momento. Esto nos permitirá fácilmente movernos sobre ella. Adicionalmente aunque no es obligatorio en la implementación de este patrón. Si es altamente recomendado añadir dos métodos utilitarios mas sobre la clase iterador. Estos son:

*   hasNext() —> Retorna true si hay mas items disponibles en la colección.
*   reset() —> Permite reiniciar el indice para iterar de nuevo sobre la colección.

Veamos entonces como sería la implementación:

{% tabbed_codeblock  %}
  <!-- tab js -->
    class Iterator {
      …
      reset() {
        this.index = 0;
      }

      hasNext() {
        return (this.collection.length > this.index +1);
      }
      …
    }
  <!-- endtab -->
{% endtabbed_codeblock %}

Super simple!, una vez que tu clase esté definida puedes usar prácticamente cualquier tipo de array para construir tu iterador y operar sobre él. Veamos un ejemplo de su uso práctico usando un ciclo while:

{% tabbed_codeblock  %}
  <!-- tab js -->
    // Usando un ciclo while
    const arr = \[1,2,3,4,5\];
    const arrayIterator = new Iterator(arr);
    console.log(arrayIterator.first());
    while (arrayIterator.hasNext()) {
      console.log(arrayIterator.next());
    }
  <!-- endtab -->
{% endtabbed_codeblock %}

De esta manera queda totalmente completo el patrón iterador. Como vez es muy fácil implementar el patrón iterador y su utilidad es casi inmediata. A continuación podrás observar todo el código completo de este ejemplo:

{% tabbed_codeblock  %}
  <!-- tab js -->
    class Iterator {
      
      constructor(collection) {
        this.index = 0;
        this.collection = collection;
      }
      first() {
        return this.collection\[0\];
      }

      next() {
        this.index += 1;
        return this.collection\[this.index\];
      }

      current() {
        return this.collection\[this.index\];
      }
      
      reset() {
        this.index = 0;
      }

      hasNext() {
          return (this.index + 1 < this.collection.length );
      }
    }

    const arr = \[1,2,3,4,5\];

    const arrayIterator = new Iterator(arr);

    console.log(arrayIterator.first());


    while (arrayIterator.hasNext()) {
      console.log(arrayIterator.next());
    }
  <!-- endtab -->
{% endtabbed_codeblock %}

Eso es todo, espero que este post te sea de utilidad y lo puedas aplicar a algún proyecto que tengas en mente y que simplemente te haya ayudado a entender la naturaleza del patrón iterator. déjame un comentario si lograste implementarlo, si quieres añadir alguna otra funcionalidad o si tienes alguna duda no dudes en dejarme un comentario en la parte de abajo, recuerda que si te gustó también puedes compartir usando los links a las redes sociales en la parte de abajo.