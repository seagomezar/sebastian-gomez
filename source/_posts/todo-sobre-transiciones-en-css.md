---
title: Todo sobre transiciones en CSS
tags:
  - Transiciones
  - CSS3
  - HTML5
  - Desarrollo Web
thumbnailImage: https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2018/01/css3.png
thumbnailImagePosition: left
url: 527.html
id: 527
categories:
  - CSS
date: 2018-01-09 16:31:03
---

Las transiciones hacen parte del conjunto de herramientas que poseemos como desarrolladores FrontEnd para mejorar la experiencia del usuario dentro de nuestra aplicación Web. Son útiles porque nos permiten animar el cambio de valores en las distintas propiedades de un elemento lo que puede hacerlo más llamativo al usuario e invitarlo a interactuar con él. En este post trataré de cubrir el extenso tema de transiciones con diversos ejemplos adaptados desde la especificación.

<!-- more -->

Empecemos con un ejemplo simple:

Tenemos un cuadrado simple:

{% tabbed_codeblock  %}
  <!-- tab html -->
    <div id="square1" class="square red"></div>
  <!-- endtab -->
{% endtabbed_codeblock %}

Y unos estilos asociados a dicho cuadrado:

{% tabbed_codeblock  %}
  <!-- tab css -->    
    .square {
      width: 50px;
      height: 50px;
      margin-bottom: 5px;
    }
    .red { background: red; }
  <!-- endtab -->
{% endtabbed_codeblock %}

Y tenemos una clase adicional que se la asignaremos al cuadrado en un momento x en el tiempo:

{% tabbed_codeblock  %}
  <!-- tab css -->
    .black {
      background: black;
    }
  <!-- endtab -->
{% endtabbed_codeblock %}

Sin embargo queremos que esto se haga de una manera suave, controlada y agradable al usuario. Por tanto es aquí donde necesitamos hacer uso de las transiciones. Esto lo podemos hacer añadiendo la propiedad transition dentro de la clase que queremos añadir:

{% tabbed_codeblock  %}
  <!-- tab css -->
    .black {
      background: black;
      transition: background 2s 0.25s;
    }
  <!-- endtab -->
{% endtabbed_codeblock %}

La propiedad transition como la hemos usado en el ejemplo anterior nos permite que el cambio de background de rojo a negro se haga durante 2 segundos (duración) en vez de hacer el cambio instantáneamente, también nos permite indicar que este cambio empiece a ocurrir 0.25 segundos después de que asigne la clase al elemento (delay).  
También existe otra sintaxis alternativa para esto que requiere unas cuantas líneas más, sin embargo es útil conocerla:

{% tabbed_codeblock  %}
  <!-- tab css -->
    .black {
      background: black;
      transition-property: background;
      transition-duration: 2s;
      transition-delay: 0.25s;
    }
  <!-- endtab -->
{% endtabbed_codeblock %}

Analogamente a los valores que hemos asignado en segundo a la duración y al retraso (delay) podríamos haberlo hecho en milisegundos, para lo cual bastaría con multiplicar por 1000 y añadir ms al final. Por ejemplo:

{% tabbed_codeblock  %}
  <!-- tab css -->
    ...
    transition-duration: 2000ms;
    transition-delay: 250ms;
    ...
  <!-- endtab -->
{% endtabbed_codeblock %}

Si queremos hacer transiciones sobre más de una propiedad, podemos usar all para indicar que la transición se aplica sobre todas las propiedades posibles:

{% tabbed_codeblock  %}
  <!-- tab css -->
    .black {
      background: black;
      color: white;
      transition: all 2s 0.25s;
    }
  <!-- endtab -->
{% endtabbed_codeblock %}

**Nota**: Transition all no es recomendable desde el punto de vista de desempeño (performance) altamente recomendamos no usar transition all a menos que definitivamente quieras aplicar transiciones sobre todo lo que pase con el elemento de la misma manera, por eso a continuación te explico como hacer transiciones específicamente con cada propiedad.

A veces no deseamos que se hagan transiciones sobre todas las propiedades de la misma manera, la propiedad transition además tiene la característica de permitir especificar la transición de cada propiedad simplemente separándolas por coma. Veamos un ejemplo:

{% tabbed_codeblock  %}
  <!-- tab css -->
    .black {
      background: black;
      color: white;
      transition: background 2s 0.25s,
                      color 1.5 3s;
    }
  <!-- endtab -->
{% endtabbed_codeblock %}

En el ejemplo anterior estamos cambiando el background y el color con distinta duración y distinto retraso (delay). Esto permite tener un control mas granular de exactamente lo que necesitamos animar en cada transición.

Como habrás notado hasta ahora las transiciones de las que hablamos ocurren de manera lineal, esto quiere decir que el cambio ocurre uniformemente durante el tiempo que dure la transición, sin embargo esta no es la única manera de hacerlo, por ejemplo podemos acelerar el cambio al comienzo y desacelerarlo al final lo que nos dará un tipo diferente de sensación al ver la transición. Para determinar como ocurrirá el cambio, tenemos la propiedad transition-timing-function que puede tomar los siguientes valores:

{% tabbed_codeblock  %}
  <!-- tab css -->
    transition-timing-function: linear; // Este es el valor por defecto, no hace falta incluirlo
    transition-timing-function: ease-in; // Significa que al comienzo sea rápido el cambio y que después se ralentice.
    transition-timing-function: ease-out; // Significa que al comienzo sea lento el cambio y que después se acelere.
    transition-timing-function: ease-in-out; // Significa que al comienzo y al final sea rápido el cambio pero en la mitad sea lento
    transition-timing-function: cubic-bezier(0.21,0.3,0.1,0.23); // De acuerdo a los valores se acelera o desacelera en los distintos momentos en que ocurre la transición.
  <!-- endtab -->
{% endtabbed_codeblock %}

Pero esta no es la única manera de añadir esta propiedad a las transiciones. También es posible hacerlo directamente en la propiedad transition:

{% tabbed_codeblock  %}
  <!-- tab css -->
    .move {
      transform: translateX(500px);
      transition: transform 2s 0.25s ease-in-out;
    }
  <!-- endtab -->
{% endtabbed_codeblock %}

Incluso en cada transición sobre las propiedades:

{% tabbed_codeblock  %}
  <!-- tab css -->    
    .move-background {
        transform: translateX(500px);
        background: red;
        transition: transform 2s 0.25s ease-in-out,
                        background 1s 0.10 cubic-bezier(0.21,0.3,0.1,0.23);
    }
  <!-- endtab -->
{% endtabbed_codeblock %}

Miremos en detalle un poco más como funciona la propiedad transition-timing-function cuando toma el valor de cubic-bezier(). Para ello revisemos en que consiste la ecuación de la curva de bezier en la cual se basa esta función.

Las curvas de bezier son un sistema matemático que desarrollo pierre bezier para el trazado de dibujos de aeronaves y automóviles que se describe como una ecuación que toma cuatro valores para describir la curva:

[![](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2018/01/23242B8E-A920-4955-84E1-C7D2563CD969.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2018/01/23242B8E-A920-4955-84E1-C7D2563CD969.png) Con una ecuación matemática de la siguiente forma: [![](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2018/01/Screen-Shot-2018-01-09-at-4.13.37-PM-1024x53.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2018/01/Screen-Shot-2018-01-09-at-4.13.37-PM.png)

Pues bien estos cuatro valores (P0 a P3) son los que describen la transición del movimiento entre el punto inicial y el punto final y con estos se pueden definir completamente diversos tipos de transiciones:

cubic-bezier(P0, P1, P2, P3); En estos sitios web puedes jugar más con este tipo de transiciones donde puedes ajustar los valores para tener un mayor control en tu transición:

*   [http://cubic-bezier.com/](http://cubic-bezier.com/)
*   [http://easings.net/](http://easings.net/)

Debes tener en cuenta que hay propiedades que no son “transicionables” esto quiere decir que no puedes aplicar transiciones a estas propiedades. Para ver una lista de cuales propiedades son “transicionables" y cuales no puedes revisar este link:

[https://developer.mozilla.org/en-US/docs/Web/CSS/CSS\_animated\_properties](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_animated_properties)

En el siguiente ejemplo se muestra un conjunto de transiciones sobre cubos con distintas transition-timing-function y propiedades, puedes jugar con ellas para evidenciar sus diferencias:

https://codepen.io/seagomezar/pen/wPbYqe

Las herramientas para desarrolladores de los navegadores como chrome y firefox nos permiten ralentizar o acelerar las transformaciones para un mejor proceso de debug en ellas, para ello puedes abrir la pestaña animaciones:

[![](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2018/01/13B715EC-945A-47A7-A6E8-F6A79F24AE3D-1024x642.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2018/01/13B715EC-945A-47A7-A6E8-F6A79F24AE3D.png)

Puedes también usar Javascript para conocer el estado de una transición mediante los siguientes listeners:

*   transitionstart
*   transitionend

Finalmente algunas consideraciones respecto a las transiciones:

*   Transiciones alrededor de 100ms son instantáneas para los usuarios y difícilmente perceptibles.
*   Transiciones de máximo 1 segundo y mínimo 250ms son buenas y mantiene a los usuario conectados.
*   Mas de 2 segundos es definitivamente una mala idea para transformaciones en sitios web estándar ya que puede desconectar al usuario de lo que pasa.
*   De 250ms a 300ms es el tiempo estándar de la mayoría de animaciones.
*   Las transiciones en general te permite crear experiencias que pasan solo una vez.
*   Si el navegador no soporta transiciones en el peor de los casos siempre se cambia la propiedad.
*   Las transiciones son granulares porque te permiten animar una o dos o x propiedades.

Eso es todo, espero que este post te sea de utilidad y lo puedas aplicar a algún proyecto que tengas en mente y que simplemente te haya ayudado a entender la naturaleza de las transiciones en CSS. Déjame un comentario si lograste implementarlo, si quieres añadir alguna otra funcionalidad o si tienes alguna duda no dudes en dejarme un comentario en la parte de abajo, recuerda que si te gustó también puedes compartir usando los links a las redes sociales en la parte de abajo.