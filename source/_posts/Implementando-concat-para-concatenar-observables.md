---
title: Implementando concat para concatenar observables
tags:
  - Desarrollo Web
  - Javascript
  - Programación Reactiva
url: 1001.html
thumbnailImage: https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2018/03/984368.png?w=400&ssl=1
thumbnailImagePosition: left
id: 1001
categories:
  - Javascript
date: 2018-05-31 12:16:12
---
En este post vamos a implementar una función que nos permita transformar distintos observables en uno solo concatenándolos.
<!-- excerpt -->

En los [post anteriores](https://www.sebastian-gomez.com/desarrollo-web/implementando-map-como-operador-en-observables/) tuvimos un acercamiento a la implementación del operador filter, map y of en observables, lo implementamos desde cero similarmente a como lo hace RX.js para entender un poco más la filosofía y el funcionamiento interno. En este post vamos a implementar una función que nos permita transformar distintos observables en uno solo concatenándolos. Esto puede resultar particularmente útil cuando quieres combinar datos que vienen desde una petición HTTP (FromEvent), con datos que tienes en arreglos o variables. Veamos un ejemplo:

{% tabbed_codeblock  %}
  <!-- tab js -->
    const sourceOne = Observable.fromEvent(/** Algo que nos dé 1, 2, 3 por ejemplo*/);
    const sourceTwo = of(4, 5, 6);
    const fuenteDeLaVerdad = Observable.concat(sourceOne, sourceTwo);
    fuenteDeLaVerdad.subscribe(/** lo que queramos hacer aquí*/);
  <!-- endtab -->
{% endtabbed_codeblock %}

En este ejemplo queremos combinar datos provenientes de una fuente dinámica con una fuente de datos estática, pero fácilmente podríamos combinar ambas fuentes estáticas o fuentes dinámicas, sin embargo el objetivo es claro, como podemos combinar dos observables. Veamos entonces, necesitamos que sea un método estático que me genere un nuevo observable a partir de los dos observables que reciba como parámetro. Por tanto un primer esqueleto podría ser algo similar a esto. (Recuerda que estamos bajo ES6 y una clase llamada Observable).

{% tabbed_codeblock  %}
    <!-- tab js -->
    class Observable {
        // ...
        static concat(...observables) {
            return new Observable(
                function subscribe(observer) {
                    // Aquí implementaremos la combinación
                }
            );
        }
        //...
    }
    <!-- endtab -->
{% endtabbed_codeblock %}

Bien, empecemos primero a desentrañar como estamos recibiendo los observables, como verás estamos usando spread operator para recibir tantos observables como queramos, (Acasó dije que solo se podían dos), esto …observables, nos permitirá poder combinar tantos como podamos y adicionalmente nos dará control sobre ellos. Lo primero entonces que necesitamos es poderlos dividir y tomar el primero de los observables, esto lo hacemos mediante dos funciones estándar para arreglos: 

{% tabbed_codeblock  %}
    <!-- tab js -->
    let myObservables = observables.slice();
    let currentObservable = myObservables.shift();

    const sourceOne = Observable.fromEvent(/** Algo que nos dé 1, 2, 3 por ejemplo*/);
    const sourceTwo = of(4, 5, 6);
    const fuenteDeLaVerdad = Observable.concat(sourceOne, sourceTwo);
    //...
    static concat(...observables) {
        return new Observable(
            function subscribe(observer) {
                let myObservables = observables.slice();
                let currentObservable = myObservables.shift();
                // En currentObservable tendriamos sourceOne
            }
        );
    }
    //...
    <!-- endtab -->
{% endtabbed_codeblock %}

Bien, ahora solo necesitamos iterar sobre todos los elementos de nuestro observable y posteriormente cuando se acaben los elementos de este observable precipitarnos con el siguiente hasta que ya no queden más observables. Vamos usar recursividad esta vez porque nos permitirá implementar la misma lógica a través de los observables.

{% tabbed_codeblock  %}
    <!-- tab js -->
    static concat(...observables) {
        return new Observable(function subscribe(observer) {
            let myObservables = observables.slice();
            let sub = null;
            let processObservable = () => {
                if (myObservables.length === 0) {
                    observer.complete();
                } else {
                    let observable = myObservables.shift(); //Take the next observable in the params.sub = observable.subscribe({
                        next(v) {
                            observer.next(v);
                        },
                        error(err) {
                            observer.error(err);
                            sub.unsubscribe();
                        },
                        complete() {
                            processObservable();
                        }
                    });
                }
            };
            processObservable();
            return {
                sub
            };
        });
    }
<!-- endtab -->
{% endtabbed_codeblock %}

Como verás solo existe una maneras de salir del primer observable al segundo, que se acaben los datos de dicho observable para lo cual volvemos a llamar la función processObservable y tomamos el siguiente observable de la lista. Si hay un error lo que hacemos inmediatamente es suspender todo el proceso de concatenación si bien en el bloque de código que te muestro arriba esta diseñado de esta manera, también podría haber una variación que podemos programar, para la cual simplemente podemos decirle que continue con el siguiente observable. Es decir:

{% tabbed_codeblock  %}
    <!-- tab js -->
    //...
    error(err) {
    observer.error(err);
    sub.unsubscribe();
    // Si falla algo con un observable pasamos al siguiente
    processObservable();
    },
    //...
    <!-- endtab -->
{% endtabbed_codeblock %}

Finalmente aquí está el ejemplo completo con la implementación de of y nuestro nuevo operador concat:

[https://codepen.io/seagomezar/pen/EpOyRj](https://codepen.io/seagomezar/pen/EpOyRj)

Eso es todo, espero que este post te sea de utilidad y lo puedas aplicar a algún proyecto que tengas en mente y que simplemente te haya ayudado a entender la naturaleza de los operadores sobre observables. déjame un comentario si lograste implementarlo, si quieres añadir alguna otra funcionalidad o si tienes alguna duda no dudes en dejarme un comentario en la parte de abajo, recuerda que si te gustó también puedes compartir usando los links a las redes sociales en la parte de abajo.
