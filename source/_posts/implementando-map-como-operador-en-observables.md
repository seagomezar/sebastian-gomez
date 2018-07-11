---
title: Implementando map como operador en Observables
tags:
  - Desarrollo Web
  - Javascript
  - Programación Reactiva
url: 576.html
id: 576
thumbnailImage: https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2018/03/984368.png?w=400&ssl=1
thumbnailImagePosition: left
categories:
  - Javascript
date: 2018-03-20 11:47:44
---

En este post vamos a valernos de esta definición para aumentar el número de operadores que podemos implementar y usar dentro de un Observable.
<!-- excerpt -->

En el [post anterior](https://www.sebastian-gomez.com/desarrollo-web/implementando-observables-a-partir-de-eventos-en-js/) tuvimos un primer acercamiento a la definición más pura de lo que es un Observable y lo implementamos desde cero similarmente a como lo hace RX.js. En este post vamos a valernos de esta definición para aumentar el número de operadores que podemos implementar y usar dentro de un Observable. Primero recordemos cual es la definición más pura de un observable con una función generadora de observables a partir de eventos:

{% tabbed_codeblock  %}
  <!-- tab js -->
    class Observable {  
      constructor(subscribe) {
        this._subscribe = subscribe;
      }
      subscribe(observer) {
        return this._subscribe(observer);
      }
      static fromEvent(domElement, eventName) {
        return new Observable(function subscribe(observer) {
          const handler = ev => { observer.next(ev) };
          domElement.addEventListener(eventName, handler);
          return {
            unsubscribe() {
              domElement.removeEventListener(eventName, handler);
            }
          }
        });
      }
    }
  <!-- endtab -->
{% endtabbed_codeblock %}

De esta manera tu puedes crear Observables que te devolverán información del evento de la siguiente manera:
{% tabbed_codeblock  %}
  <!-- tab js -->
    const button = document.getElementById("button");
    const clicks = Observable.fromEvent(button, "click");
    const clicksSubscription = clicks.subscribe({
      next(e) {
        console.log("Click in the button”, e); // “e” contiene toda la información del evento
      }
    });

    clicksSubscription.unsubscribe(); // De esta manera te puedes des-suscribir
  <!-- endtab -->
{% endtabbed_codeblock %}

Sin embargo es probable que necesitemos hacer más operaciones sobre la cadena de datos, imagínate si quizá requiramos solamente obtener sólo algunos datos del evento por ejemplo solo la posición del Mouse. O imagínate si sólo queremos escuchar por los clicks que se hagan en la parte izquierda del botón. Pues bien vamos a implementar dos funciones que quizá ya conozcas porque se usan principalmente para el procesamiento de arrays. Vamos a implementar la función “map” sobre observables. Pero antes veamos un ejemplo sobre como es su funcionamiento sobre Arrays en javascript para luego traerlo a nuestra definición de observables e implementarlo allí.

Empecemos con la función map. La función map es una función pura que itera sobre cada elemento de un array y “mapea” cada item de un array en otro. Veamos un ejemplo:

{% tabbed_codeblock  %}
  <!-- tab js -->
    [1, 2, 3\].map( elemento => elemento * 2); 
    // Retorna [1, 4, 6]
  <!-- endtab -->
{% endtabbed_codeblock %}

Ahora bien ¿Como hace internamente esta función para retornar este resultado? Básicamente la función map recibe como parámetro una función de proyección o transformación y aplica dicha función a cada uno de los elementos que pasan como parámetro. Esto hace que sin afectar el array original pueda devolver en nuevo arreglo con la función de proyección aplicada a cada item dentro del array. Veamos como sería la implementación de la función map para arrays:

{% tabbed_codeblock  %}
  <!-- tab js -->
    Array.prototype.map = function(projectionFunction) {
      const results = [];
      this.forEach(function(itemInArray) {
        results.push(projectionFunction(itemInArray));
      });
      return results;
    };
  <!-- endtab -->
{% endtabbed_codeblock %}

¿Hermosa no? Ahora que te parece si nos arriesgamos a escribir lo mismo para observables. Similarmente que para arreglos, vamos a necesitar como parámetro una función de proyección que nos permita transformar cada valor que retorna el observable. Adicionalmente queremos retornar un observable también al que nos podamos subscribir. Empecemos por solo esas dos cosas que te acabó de decir:

{% tabbed_codeblock  %}
  <!-- tab js -->
    class Observable {  
      …
      map(projectionFunction) {
        const self = this;
        return new Observable(function subscribe(observer) {
          const subscription = self.subscribe({
            next(v) {
            },
            error (e) {
            },
            complete () {
            }
          });
          return subscription;
        });
      }
      …
    }
  <!-- endtab -->
{% endtabbed_codeblock %}

Simple, ¿no? Como verás hemos definido la función map que ya no es “static” ya que no se puede aplicar sobre la definición abstracta de Observable sino sobre instancias de dicha clase. Adicionalmente recibimos como parámetro una función de proyección que es la que transformará cada valor que retorne la función interna next, y finalmente retornamos la subscripción para que sea posible suscribirnos al observable que justo acabamos de crear. Veamos entonces como podemos aplicar la función de proyección a cada elemento que retornemos dentro de la función next.

{% tabbed_codeblock  %}
  <!-- tab js -->
    class Observable {  
      …
      map(projectionFunction) {
        const self = this;
        return new Observable(function subscribe(observer) {
          const subscription = self.subscribe({
            next(v) {
                const value = projectionFunction(v);
                observer.next(value);
            },
            error (e) {
            },
            complete () {
            }
          });
          return subscription;
        });
      }
      …
    }
  <!-- endtab -->
{% endtabbed_codeblock %}

Como ves no hicimos otra cosa que usar la función de transformación sobre cada valor que queremos enviar al observador. Eso es todo!, esta definida nuestra función map dentro de la definición que tenemos de Observable. Ajustemos un poco más esta definición previniendo posibles errores inesperados:

{% tabbed_codeblock  %}
  <!-- tab js -->
    class Observable {  
      …
      map(projection) {
        const self = this;
        return new Observable(function subscribe(observer) {
          const subscription = self.subscribe({
            next(v) {
              let value;
              try {
                value = projection(v);
                observer.next(value);
              }
              catch(e) {
                observer.error(e);
                subscription.unsubscribe();
              }
            },
            error (e) {
              observer.error(e);
            },
            complete () {
              observer.complete();
            }
          });
          return subscription;
        });
      }
      …
    }
  <!-- endtab -->
{% endtabbed_codeblock %}

Con lo anterior queda completa nuestra función map en la definición de observable. Veamos ahora como podemos usarla para transformar los datos dentro de nuestro observable, obteniendo solamente la posición del mouse en el momento que hizo click sobre el botón.

{% tabbed_codeblock  %}
  <!-- tab js -->
    const button = document.getElementById("button");
    const clicks = Observable.fromEvent(button, "click");
    const clicksSubscription = clicks.map(ev =>ev.offsetX).subscribe({
      next(offSetX) {
        console.log(offSetX);
      }
    });
  <!-- endtab -->
{% endtabbed_codeblock %}

En el siguiente Codepen podrás revisar la definición completa:

*   [https://codepen.io/seagomezar/pen/zpgROb](https://codepen.io/seagomezar/pen/zpgROb?editors=1011)

Eso es todo, espero que este post te sea de utilidad y lo puedas aplicar a algún proyecto que tengas en mente y que simplemente te haya ayudado a entender la naturaleza del operador map sobre observables. déjame un comentario si lograste implementarlo, si quieres añadir alguna otra funcionalidad o si tienes alguna duda no dudes en dejarme un comentario en la parte de abajo, recuerda que si te gustó también puedes compartir usando los links a las redes sociales en la parte de abajo.