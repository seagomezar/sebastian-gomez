---
title: Todo sobre Animaciones en CSS
tags:
  - Animaciones
  - CSS3
  - HTML5
  - Desarrollo Web
thumbnailImage: https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2018/01/css3.png
thumbnailImagePosition: left
url: 539.html
id: 539
categories:
  - CSS
date: 2018-01-24 12:13:17
---

Las animaciones en CSS son un conjunto de herramientas que nos permite mostrar contenido de una manera mas dinámica y llamativa para los usuarios de nuestro sitio web. Su principal ventaja versus las transiciones es que nos permite tener un más granular de los estados de la animación mediante los @keyframes. En este post daremos un vistazo general a animaciones en CSS y como implementarlas.
<!-- excerpt -->

Las animaciones en CSS son un conjunto de herramientas que nos permite mostrar contenido de una manera mas dinámica y llamativa para los usuarios de nuestro sitio web. Su principal ventaja versus las transiciones es que nos permite tener un más granular de los estados de la animación mediante los @keyframes. Veamos un ejemplo de una animación simple en la forma abreviada:

{% tabbed_codeblock  %}
  <!-- tab css -->
    .animated-thing {
      animation: black-to-white 1s linear 1;
    }
  <!-- endtab -->
{% endtabbed_codeblock %}

Ahora veamos la misma animación en la forma larga:

{% tabbed_codeblock  %}
  <!-- tab css -->
    .animated-thing {
      animation-name: black-to-white;
      animation-duration: 1s;
      animation-timing-function: linear;
      animation-iteration-count: 1;
      animation-delay: 0;
    }
  <!-- endtab -->
{% endtabbed_codeblock %}

El nombre de la animación hace referencia al keyframe respectivo. Los @keyframes en CSS los podemos definir como los puntos de trayectoria de una animación, es decir nos permite definir que valores deben alcanzar las propiedades que estamos animando a lo largo de la secuencia de animación. Esto nos da un control más específico sobre los pasos intermedios de la secuencia de animación.

{% tabbed_codeblock  %}
  <!-- tab css -->
    @keyframes black-to-white {
      0% {}
      25%, 35% { }
      100% {}
    }
  <!-- endtab -->
{% endtabbed_codeblock %}

También se pueden utilizar formas abreviadas:

{% tabbed_codeblock  %}
  <!-- tab css -->
    @keyframes black-to-white { 
      from{} 
      to{}
    }
  <!-- endtab -->
{% endtabbed_codeblock %}

También puedes usar mas de un keyframe a la vez separando por comas las diferentes animaciones:

{% tabbed_codeblock  %}
  <!-- tab css -->
    .animated-thing {
      animation:
        black-to-white 1s linear 1,
        black-to-red 2s ease-out infinite 2s;
    }
  <!-- endtab -->
{% endtabbed_codeblock %}

Una consideración a tener en cuentas es que los nombres de la animación no pueden empezar con números o caracteres especiales. Por ejemplo "1st-animation” no sería un nombre válido para la animación.

### Animación de Sprites usando CSS

La animación de sprites es una de las técnicas mas avanzadas que se puede tener con CSS, primero es necesario entender que es un sprite: Un sprite es una imagen gráfica única que se incorpora a una escena más grande para que parezca ser parte de la escena.

[![](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2018/01/700A020D-5636-4097-898A-FD3B6C6EB0A5.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2018/01/700A020D-5636-4097-898A-FD3B6C6EB0A5.png)

Aquí se ve un ejemplo de una hoja de sprites del capitán america. Cada secuencia de movimiento ocupa el mismo espacio que las demás y poniéndolas juntas a determinada velocidad nos permite “percibir” que hay movimiento en el personaje.

Bien para hacer una efectiva animación de un sprite en CSS no nos basta con las animation-timing-function: linear, ease-in, ease-out, ease-in-out o cubic-bezier; ya que en cualquier caso se vería el movimiento de los frames de un lado a otro. Allí es donde podemos usar la función steps(x) donde x es el número de sprites que tendría tu hoja de sprites. steps(x) divide un bloque de keyframes en pasos iguales x luego salta entre ellos. Veamos un ejemplo de como animar el capitán américa del sprite anterior:

{% tabbed_codeblock  %}
  <!-- tab css -->
    .waking-front {
      background: url(http://untamed.wild-refuge.net/images/rpgxp/avengers/captainamerica_shield.png) 0 0 no-repeat;
      animation: waking-front 1s steps(4) infinite;
      height: 48px;
      width: 32px;
      margin: 50px auto 0;
    }

    @keyframes waking-front {
      0% { 
        background-position: 0 0;
      }  
      100% { 
        background-position: -128px 0;
      }
    }
  <!-- endtab -->
{% endtabbed_codeblock %}

Como buena práctica se sugiere no poner el 0% si el estado inicial es el mismo de la imagen por lo que una versión reducida podría ser de esta manera:

{% tabbed_codeblock  %}
  <!-- tab css -->
    @keyframes waking-front {
      100% { 
        background-position: -128px 0;
      }
    }
  <!-- endtab -->
{% endtabbed_codeblock %}

Hay también tres propiedades importantes que deberías conocer cuando estas creando animaciones en CSS, estas propiedades van directamente dentro del @keyfram. Estas son:

animation-fill-mode: forwards o backwards;  // Permite controlar cual es el estado final de la aplicación si quieres que al finalizar la aplicación se preserven los estilos con los que terminó la aplicación.

animation-play-state: paused  o running; // Te permite controlar el estado de la animación, si esta corriendo o esta pausada.

animation-direction: reverse o alternate o alternate-reverse; // Te permite controlar si la animación empieza o termina en el valor del 100%, si es una animación que esta ejecutando infinitamente puedes usar alternate para que se devuelva e inicie indefinidamente.

Puedes controlar el estado de una animación con Javascript usando los siguientes listeners:

*   animationstart
*   animationend
*   animationiteration

En los siguientes ejemplos se muestran un conjunto de animaciones con sprites con distintas animaciones y propiedades, puedes jugar con ellas para evidenciar sus diferencias:

* https://codepen.io/seagomezar/pen/RjzMvQ https://codepen.io/seagomezar/pen/KyOKKe 
* https://codepen.io/seagomezar/pen/vWoBYM https://codepen.io/seagomezar/pen/RjzXmO

Personalmente sigo a Rachel Nabors ([@](https://twitter.com/rachelnabors)[rachelnabors](https://twitter.com/rachelnabors)) una de mis grandes heroínas en animación [http://rachelnabors.com/](http://rachelnabors.com/).

Finalmente algunas consideraciones sobre las animaciones:

*   Las animaciones pueden hacer loop infinitamente.
*   Poseen auto inicio, no se requiere un disparador como las transiciones.
*   Se pueden repetir infinitamente.
*   Se pueden alternar entre end y start.
*   Se pueden agrupar distintas propiedades.
*   Usa las animaciones para indicar que un elemento cambia de dirección, de estado de solidez y de momentum.
*   “Animations in” o animaciones al comienzo son mas fáciles que las animaciones al final ( animation out). Por eso verás miles de sitios con un animaciones al comienzo que una vez que terminan difícilmente regresan de una buena manera al estado original.
*   Existe tres tipos de animaciones:
*   *   Supplemental Animations: Aquellas no están relacionadas con la información inicial.
    *   Decorative Animations: Solo aportan decoración y nunca deberías tener mas de una.
    *   Stateful Animations: El core de tu animación y animaciones sobre contenido importante o call to action, resalta detalles que el usuario no debería pasar por alto.
*   Puedes usar la propiedad translateZ(0) que hará que tu animación sea ejecutada por el hardware en vez de tu sitio navegador, usa esto si estas teniendo problemas de performance con las animaciones o si simplemente quieres optimizar tu flujo.

Eso es todo, espero que este post te sea de utilidad y lo puedas aplicar a algún proyecto que tengas en mente y que simplemente te haya ayudado a entender la naturaleza de las animaciones en CSS. Déjame un comentario si lograste implementarlo, si quieres añadir alguna otra funcionalidad o si tienes alguna duda no dudes en dejarme un comentario en la parte de abajo, recuerda que si te gustó también puedes compartir usando los links a las redes sociales en la parte de abajo.