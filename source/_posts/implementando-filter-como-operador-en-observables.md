---
title: Implementando filter como operador en Observables
tags:
  - Desarrollo Web
  - Javascript
  - Programación Reactiva
url: 582.html
thumbnailImage: https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2018/03/984368.png?w=400&ssl=1
thumbnailImagePosition: left
id: 582
categories:
  - Javascript
date: 2018-04-10 12:16:12
---

En este post vamos a aumentar el número de operadores que podemos implementar y usar dentro de un Observable añadiendo el operador map.
<!-- excerpt -->

En el [post anterior](https://www.sebastian-gomez.com/desarrollo-web/implementando-map-como-operador-en-observables/) tuvimos un primer acercamiento a la implementación del operador map en observables, lo implementamos desde cero similarmente a como lo hace RX.js. En este post vamos a aumentar el número de operadores que podemos implementar y usar dentro de un Observable añadiendo el operador map. Primero recordemos cual es la definición más pura de un Observable con una función generadora de observables a partir de eventos:

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

Sin embargo es probable que necesitemos hacer más operaciones sobre la cadena de datos, imagínate si quizá requiramos solamente obtener sólo algunos datos del evento por ejemplo solo la posición del Mouse. O imagínate si sólo queremos escuchar por los clicks que se hagan en la parte izquierda del botón. Pues bien vamos a implementar dos funciones que quizá ya conozcas porque se usan principalmente para el procesamiento de arrays. Vamos a implementar la función filter sobre Observables. Pero antes veamos un ejemplo sobre como es su funcionamiento sobre Arrays en javascript para luego traerlo a nuestra definición de observables e implementarlo allí.

{% tabbed_codeblock  %}
  <!-- tab js -->
    [1,2,3,4].filter(x=> x > 3) 
    // retorna [4]
  <!-- endtab -->
{% endtabbed_codeblock %}

Como vez similarmente que map, filter recibe como parámetro una función, solo que en este casó es de validación es decir retorna “true” o “false”. Valiendose de esta función el operador filter retorna el elemento para el cual la función de validación es “true”. Revisemos como sería la implementación si sobre arrays para luego extenderla a Observables.

{% tabbed_codeblock  %}
  <!-- tab js -->
    filter(validationFunction) {
      const self = this;
      return new Observable(function subscribe(observer) {
        const subscription = self.subscribe({
          next(v) {
            if(validationFunction(v)) {
              observer.next(v);
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
  <!-- endtab -->
{% endtabbed_codeblock %}

¿Es más simple esta vez, no? En general los operadores sobre observables siguen la misma lógica. Todos (O al menos la mayoría) deben retornar un nuevo observable y las operaciones o filtros sobre los valores se deben hacer sobre la función next. Finalmente veamos como usar el operador filter sobre observables para filtrar los clicks que se hacen sobre un botón pero que cumplen el criterio de encontrarse sobre el lado izquierdo del mouse.

{% tabbed_codeblock  %}
  <!-- tab js -->
    const button = document.getElementById("button");
    const clicks = Observable.fromEvent(button, "click");
    const clicksSubscription = clicks.filter(ev => ev.offsetX < 40).map(ev => ev.offsetX ).subscribe({
      next(offSetX) {
        console.log("La posición del mouse respecto a X " + offSetX); // Solo imprime si haces click a la izquerda de "Me"
      }
    });
  <!-- endtab -->
{% endtabbed_codeblock %}

Finalmente aquí está el ejemplo completo con la implementación de filter y map:

[https://codepen.io/seagomezar/pen/WdVzxB](https://codepen.io/seagomezar/pen/WdVzxB)

Eso es todo, espero que este post te sea de utilidad y lo puedas aplicar a algún proyecto que tengas en mente y que simplemente te haya ayudado a entender la naturaleza de los operadores sobre observables. déjame un comentario si lograste implementarlo, si quieres añadir alguna otra funcionalidad o si tienes alguna duda no dudes en dejarme un comentario en la parte de abajo, recuerda que si te gustó también puedes compartir usando los links a las redes sociales en la parte de abajo.