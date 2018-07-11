---
title: Implementando Observables a partir de Eventos en JS
tags:
  - Desarrollo Web
  - Javascript
  - Programación Reactiva
url: 565.html
id: 565
thumbnailImage: https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2018/03/984368.png?w=400&ssl=1
thumbnailImagePosition: left
categories:
  - Javascript
date: 2018-03-07 14:19:28
---

En éste post vamos a tratar de implementar la definición más pura de observable y la función generadora de observables más común como ya lo tiene implementado RX.js con el fin de entender a profundidad el concepto.
<!-- excerpt -->

Nos encontramos en la ola de la programación reactiva y funcional, y a pesar de todas la ventajas que ofrece aún es desconocida para la mayoría de desarrolladores sobre todo aquellos desarrolladores jóvenes. En éste post vamos a tratar de implementar la definición más pura de observable y la función generadora de observables más común como ya lo tiene implementado RX.js con el fin de entender a profundidad el concepto.

La manera más natural de acercarse a los observables es entendiendo que es  la combinación de dos patrones de diseño en Javascript: Uno es el [patrón iterador](https://www.sebastian-gomez.com/desarrollo-web/patron-iterador-iterator-pattern-en-javascript/) que lo explico en este post, y el segundo es el [patrón observer](https://www.sebastian-gomez.com/desarrollo-web/entendiendo-el-patron-observador-observer-pattern-en-javascript/) que también lo explico en este post, sin embargo no es la manera más fácil. Una vez que hayas leido estos dos patrones y al menos tengas una idea en la cabeza de lo que son y significan un observable no es más que una función en Javascript con otra función en su interior que retorna un Objeto (JSON) con tres funciones en su interior. onNext, onComplete y onError. En primer lugar escribamos en Javascript lo que te acabo de narrar:

{% tabbed_codeblock  %}
  <!-- tab js -->
    // ES5 & 6 (Antiguo estándar)
    function Observable(forEach) {
      this._forEach = forEach;
    }

    Observable.prototype = {
      forEach: function(onNext, onError, onCompleted) {
        if (typeof onNext === "function") {
          /* la función se invocó explicitamente con 
          * (onNext: ()=> ...., onError: () => ....., onComplete: () => ....) */
          return this._forEach({
            onNext: onNext,
            onError: onError || function(){},
            onCompleted: onCompleted || function(){}
          })
        }
        else {
          /* la función se invocó explicitamente con un objeto en lugar de tres funciones
          * {onNext: ()=> ...., onError: () => ....., onComplete: () => ....} */
          return this._forEach(onNext);
        }
      }
    }
  <!-- endtab -->
{% endtabbed_codeblock %}

Como te dije es una función llamada Observable que recibe como parámetro una función que en este caso la llamamos forEach, ¡Y Sí! Tiene todo que ver con la función ForEach que usamos con los arreglos en Javascript. Como verás esta función la implementamos de manera interna en el Observable para hacerla propia de la definición del Observable. Como verás en la invocación de la función forEach, asumimos que la van a invocar enviándole tres parámetros, onNext, onError y onComplete y que lo único que haremos será detectar si han enviado cada función como parámetro o si han enviado las tres funciones dentro un objeto JSON, y en cualquier caso devolvemos recursivamente la invocación del forEach nuevamente con la declaración en su interior.

Wait what? …

Sí, tal vez esto hasta aquí te estalló un poco la cabeza, la programación con observables suele cambiar el paradigma con el cual escribimos código, trataré de mostrarte un ejemplo para que sirve lo que hemos hecho escrito allí arriba, pero antes añadamos una función al enredado bloque de código de arriba. Imagínate que quieres mostrar un console.log(“Hizo click”); cada vez que un usuario hace click en un botón de tu sitio web. Para ello vamos a crear una función estática dentro de la definición de observable que creamos arriba que convertirá un evento del DOM en un observable:

{% tabbed_codeblock  %}
  <!-- tab js -->
    Observable.fromEvent = function(dom, eventName) {
      return new Observable(function forEach(observer) {
        var handler = (e) => observer.onNext(e);
        dom.addEventListener(eventName, handler);
        return {
          dispose: () => {
          dom.removeEventListener(eventName, handler)
        }
      }
      });
    }
  <!-- endtab -->
{% endtabbed_codeblock %}

Como verás la función fromEvent recibe un elemento del DOM y el nombre del evento sobre el que queremos hacer seguimiento y retorna un observable sobre que nos retorna el evento cada vez que se ejecuta la acción dentro de la función onNext. Adicionalmente un Observable siempre debe retornar un objeto que llamaremos subscripción que tiene la responsabilidad de proveer una manera de dessuscribirnos al observable en caso de que no deseemos recibir mas información esta es la responsabilidad de la función dispose. Ahora veamos como usamos los dos bloques de código juntos para capturar los eventos cada vez que hagamos click sobre un botón.

{% tabbed_codeblock  %}
  <!-- tab js -->
    const button = document.getElementById("button");
    const clicks = Observable.fromEvent(button, "click");
    const clicksSubscription = clicks.forEach( event => {
        console.log("El usuario hizo click", event);
    });
  <!-- endtab -->
{% endtabbed_codeblock %}

Bien veamos ahora como podemos hacer lo mismo usando completamente ES7, lo primero que tenemos que tener presente es que la función forEach por convención es reemplazada por la función subscribe. Y nos valdremos de las palabras reservadas class y static para crear nuestra implementación de observable. Veamos entonces en primer lugar como implementamos la definición pura de observable:

{% tabbed_codeblock  %}
  <!-- tab js -->
    class Observable {  
      constructor(subscribe) {
        this._subscribe = subscribe;
      }
      subscribe(observer) {
        return this._subscribe(observer);
      }
    }
  <!-- endtab -->
{% endtabbed_codeblock %}

Como verás aprovechándonos de la definición de clase y constructor se hace mas corta la implementación del observable. Ademas en esta nueva definición no nos importa en absoluto como el observador (observer) envía las funciones onNext, onComplete y onError (Que por cierto por convención se cambian a next, error y complete). Veamos entonces como implementamos la función fromEvent para crear observables a partir de eventos.

{% tabbed_codeblock  %}
  <!-- tab js -->
    class Observable {
      …
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

Análogamente la ejecución del observable se realiza de manera similar a la ejecución del observable en el antiguo estándar con la diferencia que en lugar de usar la función forEach, usamos la función subscribe:

{% tabbed_codeblock  %}
  <!-- tab js -->
    const button = document.getElementById("button");
    const clicks = Observable.fromEvent(button, "click");
    const clicksSubscription = clicks.subscribe({
      next(e) {
        console.log("Click in the button");
      }
    });
  <!-- endtab -->
{% endtabbed_codeblock %}

Como verás en los dos bloques de código anteriores notaras que la diferencia entre el estándar viejo o antiguo para crear la definición pura más simple de un observable es significativamente mas larga y más descriptiva que el estándar nuevo y esto se debe a que usando el estándar nuevo dejamos por completo la implementación de las funciones onNext, onComplete y onError al observador que en adelante serán llamadas next, complete y error (sin el “on” al comienzo) verás que la invocación y ejecución del observable es muy similar usando ambos estándares.

Como notarás un observable no se ejecuta a menos que tengas un subscriptor que este escuchando los eventos mediante el subscribe o el forEach respectivamente y esto se debe a que por naturaleza los observables son fríos (Cold or lazy observables) es decir que no hacen nada a menos que tengan a alguien que los escuche.

Finalmente para dessuscribirte de un Observable puedes usar el método dispose que o unsubscribe que viene junto con la subscripción de la siguiente manera:

{% tabbed_codeblock  %}
  <!-- tab js -->
    // Estandar antiguo
    clicksSubscription.dispose();

    // Estándar nuevo
    clicksSubscription.unsubscribe();
  <!-- endtab -->
{% endtabbed_codeblock %}

Sin embargo recuerda que si te dessuscribes antes de escuchar por el observable no obtendrás ningún evento, es decir una implementación como la siguiente solo nos permitiría obtener únicamente el primer click sobre el botón:

{% tabbed_codeblock  %}
  <!-- tab js -->
    const clicksSubscription = clicks.subscribe({
      next(e) {
        console.log("Click in the button");
        clicksSubscription.unsubscribe();
      };
    });
  <!-- endtab -->
{% endtabbed_codeblock %}

En los siguientes links encontrarás la implementación: 
* [Usando el estándar Antiguo (forEach)](https://codepen.io/seagomezar/pen/aEVZmv) 
* [Usando el estándar Nuevo (Subscribe)](https://codepen.io/seagomezar/pen/MrOeyd)

Eso es todo, espero que este post te sea de utilidad y lo puedas aplicar a algún proyecto que tengas en mente y que simplemente te haya ayudado a entender la naturaleza de los observables. déjame un comentario si lograste implementarlo, si quieres añadir alguna otra funcionalidad o si tienes alguna duda no dudes en dejarme un comentario en la parte de abajo, recuerda que si te gustó también puedes compartir usando los links a las redes sociales en la parte de abajo.