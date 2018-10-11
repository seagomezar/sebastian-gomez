---
title: Implementando of como operador en Observables
tags:
  - Desarrollo Web
  - Javascript
  - Programación Reactiva
url: 588.html
id: 588
thumbnailImage: https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2018/03/984368.png?w=400&ssl=1
thumbnailImagePosition: left
clearReading: true
categories:
  - Javascript
date: 2018-05-16 09:01:33
---

En este post vamos a aumentar el número de operadores que podemos implementar y usar dentro de un Observable creando el operador “of" que básicamente será un operador de generación de observables de manera que podamos convertir datos en observables.
<!-- excerpt -->

En los [posts anteriores](https://www.sebastian-gomez.com/desarrollo-web/implementando-filter-como-operador-en-observables/) tuvimos un acercamiento a la implementación del operador filter y el operador map en observables, lo implementamos desde cero similarmente a como lo hace RX.js para entender un poco más la filosofía y el funcionamiento interno. En este post vamos a aumentar el número de operadores que podemos implementar y usar dentro de un Observable creando el operador “of" que básicamente será un operador de generación de observables de manera que podamos convertir datos en observables, veamos un ejemplo de lo que buscamos:

{% tabbed_codeblock  %}
  <!-- tab js -->
      const fancyObservable = Observable.of(5);

      const fancySubscription = fancyObservable.subscribe({
        next(e) {
          console.log(e); // “e” 
        }
      });
    <!-- endtab -->
{% endtabbed_codeblock %}

En este caso te estarás preguntando, ¿porqué es esto importante? ¿Que sentido tiene convertir una dato ya desenvuelto en un Observable? Bien, la respuesta más inmediata es porque a veces necesitas combinar datos “puros” con datos que provienen de Observables y más adelante veremos maneras de combinar observables más no tenemos manera de combinar datos “puros” con observables sin primero llevar los datos a Observables.

Antes de empezar la implementación recordemos cual es la definición más pura de un Observable con una función generadora de observables a partir de eventos:

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

Similarmente al operador fromEvent debemos crear el operador Of de manera estática es decir que esta función va estar ligada al prototipo (prototype) sin importar si existe o no una instancia de un Observable. Diferente a lo que pasaba con las funciones filter y map que definitivamente solo se invocaban si ya tenían un observable instancia.

{% tabbed_codeblock  %}
  <!-- tab js -->
      static of(value) {…}
  <!-- endtab -->
{% endtabbed_codeblock %}

Ahora revisando la definición de frontEvent nos damos cuenta que solo esta retornando un valor a la vez cada ves que ocurre un nuevo evento, en nuestro caso para el operador “of” solo habrá un evento ya que hay un único dato e inmediatamente debemos completar el observable:

{% tabbed_codeblock  %}
  <!-- tab js -->
      observer.next(value);
      observer.complete();
  <!-- endtab -->
{% endtabbed_codeblock %}

¿Super fácil no?, ahora pongamos todo junto y tendremos la definición completa de nuestro operador of:
{% tabbed_codeblock  %}
  <!-- tab js -->
      static of(value) {
    return new Observable(function subscribe(observer) {
      observer.next(value);
      observer.complete();
      return {
        unsubscribe() {}
      };
    });
  }
    <!-- endtab -->
{% endtabbed_codeblock %}

Finalmente aquí está el ejemplo completo con la implementación de filter, map y nuestro nuevo operador of:

[https://codepen.io/seagomezar/pen/rvZZev](https://codepen.io/seagomezar/pen/rvZZev?editors=0011)

Eso es todo, espero que este post te sea de utilidad y lo puedas aplicar a algún proyecto que tengas en mente y que simplemente te haya ayudado a entender la naturaleza de los operadores sobre observables. déjame un comentario si lograste implementarlo, si quieres añadir alguna otra funcionalidad o si tienes alguna duda no dudes en dejarme un comentario en la parte de abajo, recuerda que si te gustó también puedes compartir usando los links a las redes sociales en la parte de abajo.