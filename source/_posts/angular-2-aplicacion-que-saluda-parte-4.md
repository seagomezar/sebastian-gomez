---
title: Angular 2 aplicación que saluda Parte 4
tags:
  - Angular
  - Desarrollo Web
  - AngularJS
  - CSS
  - HTML5
  - Javascript
url: 354.html
thumbnailImage: https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/11/a1.png
thumbnailImagePosition: left
id: 354
categories:
  - Angular
date: 2015-11-10 15:34:31
---

En este post vamos a adentrarnos un poco más en las características nuevas de Angular 2, si aún no estas lo suficientemente familiarizado con Angular 2 y su funcionamiento básico, <!-- more --> te invito a que revises mi anterior post [Hola Mundo con Angular 2](http://www.sebastian-gomez.com/desarrollo-web/angular-2-con-javascript-hola-mundo-parte-3/). Vamos a construir una aplicación un paso más compleja que el hola mundo, esta aplicación saludará a alguien y guardará temporalmente en una lista a las personas que haya saludado. A continuación te muestro una imagen de como lucirá nuestra aplicación luego de haber saludado a tres personas.

[![app](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/11/app-300x111.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/11/app.png)

En primer lugar te mostraré la estructura del proyecto que construiremos:

[![estructura](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/11/estructura.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/11/estructura.png)

Bien entonces como verás respecto al tutorial anterior en el cual construimos un hola mundo básico, han cambiado varias cosas, en primer lugar hemos creado una carpeta views(vistas) donde tenemos dos archivos uno llamado cabecera.html y otro llamado saludador.html. En el archivo cabecera.html simplemente tenemos un texto básico donde se describe la aplicación:

{% tabbed_codeblock  %}
  <!-- tab html -->
    <h1>Esta aplicación obtiene un nombre del cuadro de texto y luego dice "Hola [Nombre]"</h1>
  <!-- endtab -->
{% endtabbed_codeblock %}

Como puedes ver no es nada complejo, es simplemente codigo html básico. Sin embargo en el archivo saludador.html si verás muchas cosas nuevas.

{% tabbed_codeblock  %}
  <!-- tab html -->
    <h1>Hola { {yourName}}</h1>
    <input #name placeholder="Pon tu nombre aquí"> <button (click)="setName(name)">Saludame</button>
    <h2> Hasta la fecha e saludado a: </h2>
    <ul><li *ng-for="#saludado of saludados">{ { saludado }} </li></ul>
  <!-- endtab -->
{% endtabbed_codeblock %}

Como verás la sintaxis para mostrar variables en Angular no ha cambiado mucho, simplemente { {yourName}} mostrará el contenido de la variable yourName en la vista. Ahora analicemos un poco la línea 2, verás que el input tiene algo extraño y nuevo verás un #name, básicamente esto es nuevo y reemplaza los ng-models de Angular en la versión 1, pero en que consiste?, simplemente Angular asocia este input con una variable en nuestra aplicación pero esta variable no es simplemente el texto sino el HTML object completo, es decir el input con todas las características asociadas a este ejemplo value, child, parent, etc... También verás que en esta misma línea creamos un botón y a la acción click  del botón denotada por (click) le asociamos una función setName donde mandamos el input #name completo.

En la línea 4 verás la primera directiva nueva de Angular, esta directiva es *ng-for , y básicamente funciona como un ng-repeat, es decir para cada valor en el array saludados pondrá el contenido actual en saludado y este podrá ser accedido y usado como una variable "normal" dentro de las llaves.

Pasemos ahora a nuestro archivo app.js, que es nuestro archivo principal donde manejaremos toda nuestra aplicación:

{% tabbed_codeblock  app.js %}
  <!-- tab js -->
    (function() {
      var AppTitle = ng.Component({
          selector: 'cabecera',
          templateUrl: 'views/cabecera.html'
        })
        .Class({
          constructor: function () { }
        });
      var Saludador = ng.Component({
            selector: 'saludador',
            templateUrl: 'views/saludador.html',
            directives: \[ng.NgFor\]
          })
          .Class({
            constructor: function () {
              this.yourName = "sebas";
              this.saludados = \["sebas"\];
            },
            setName: function(name){
              this.yourName = name.value;
              this.saludados.push(name.value);
              name.value = null;
            }
          });
      document.addEventListener('DOMContentLoaded', function() {
        ng.bootstrap(AppTitle);
        ng.bootstrap(Saludador);
      });
    })();
  <!-- endtab -->
{% endtabbed_codeblock %}

Línea 1: Simplemente estamos rodeando todo el código de una función IIFE, esto significa Inmediated Invoked Function Execution que como su nombre lo indica será ejecutada tan pronto el archivo sea cargado o invocado desde un archivo html.

Línea 2: Estamos declarando un componente de Angular, esta es la sintaxis para declarar componentes, los componentes son porciones de Javascript que opera sobre un template específico y que son utilizados desde los archivos html con una etiqueta o selector.

Línea 3: Será el nombre del selector o etiqueta que utilizaremos para incrustar nuestro componente en los archivos html en nuestro caso será la etiqueta cabecera que debemos incluirla donde llamemos a nuestra aplicación.

Línea 4: Es la ruta al archivo HTML que vamos a insertar en nuestra aplicación.

Línea 6: Es una clase, si, una clase, una de las nuevas caracteristicas que tiene Angular es que está Basado en el ECMA script 6 y este nuevo estandar de Javascript ya soporta la palabra Class, dentro de la Class del componente se definirá el código Javascript que quieras que controle la aplicación y como quieras que lo haga, puedes definir atributos, constructores, métodos, etc etc, si quieres puedes aplicar todo el paradigma de la programación orientada a objetos allí mismo. Para el caso de nuestra cabecera como ves, esta no tiene ninguna funcionalidad por tanto la podemos dejar vacía.

Línea 9, 10 y 11: Es nuestro segundo Angular Component llamado saludador, como verás la etiqueta con la cual podemos invocarlo se llama saludador y el templateUrl hace referencia a views/saludador.html que es el contenido HTML que vamos a insertar cuando invoquemos al selector.

Línea 12: Esta es la nueva manera de incluir directivas en Angular, es decir si vas a usar alguna directiva propia, o del core de Angular, deberás específicamente incluirla en este array, y solo la podrás usar en el componente en el que la incluiste, otras directivas son por ejemplo : NgForm,  NgFormControl,  NgFormModel,  NgIf,  NgModel,    NgSelectOption,  NgClass, entre otras.

Línea 14-18: Nuestra clase que controlará nuestro componente, nota que al interior tenemos un constructor que inicializará nuestras variables usadas, nota que la variable yourName es iniciada con el valor "Sebas" y nota que insertamos en el array de saludados también a "sebas", ahora todo tiene sentido no?. Recuerda estas variables solo existen al interior de este selector, estas variables no se pueden usar en otro selector.

Línea 19-22: Esta es la función principal de nuestra aplicación ya que es esta la que recibe el input enviado desde la vista, luego extrae su valor mediante .value, lo asigna a la variable yourName y lo inserta al array de saludados. Finalmente limpia el valor del input para que quede como nuevo.

Línea 25-28: Estamos añadiendo un listener al documento, es decir cuando se haya cargado todo el DOM (Document Object Model) se cargue nuestros componente AppTitle con su selector cabecera y Saludador con su selector saludador, esto estará disponible por si algún archivo .html lo invoca.

Hasta aquí estamos casi terminados pero falta ver nuestro archivo principal index.html para ver que ha cambiado.

{% tabbed_codeblock  %}
  <!-- tab html -->
    <html>
      <head>
        <title>Aprendiendo Angular 2</title>
        <script src="node_modules/angular2/bundles/angular2.sfx.dev.js"></script>
        <script src="app.js"></script>
      </head>
      <body>
        <cabecera></cabecera>
        <saludador></saludador>
      </body>
    </html>
  <!-- endtab -->
{% endtabbed_codeblock %}

Como verás simplemente hemos añadido nuestro componente saludador llamando su selector <saludador></saludador>, y esto es todo tenemos nuestra aplicación terminada.

Y eso es todo, te felicito, has creado tu segunda aplicación usando Angular 2, si te perdiste en algo te recomiendo que visites mi post anterior [Hola Mundo con Angular 2](http://www.sebastian-gomez.com/desarrollo-web/angular-2-con-javascript-hola-mundo-parte-3/) donde verás como instalar y organizar tu entorno para poder crear tus aplicaciones en Angular 2. Si tienes alguna duda no dudes en dejarme un comentario en la parte de abajo, recuerda que si te gustó también puedes compartir usando los links a las redes sociales en la parte de abajo. Este post hace parte de una serie de tutoriales sobre Angular 2, a continuación te dejo los links.

[AngularJS 2 Parte 0](http://www.sebastian-gomez.com/desarrollo-web/introduccion-a-angularjs-parte-0/)

[Qué es AngularJS? Parte 1](http://www.sebastian-gomez.com/desarrollo-web/angular-1-vs-angular-2-parte-2/)

[AngularJS 1 vs Angular 2? Parte 2](http://www.sebastian-gomez.com/desarrollo-web/angular-1-vs-angular-2-parte-2/)

[Hola Mundo con Angular 2 Parte 3](http://www.sebastian-gomez.com/desarrollo-web/angular-2-con-javascript-hola-mundo-parte-3/)

En el siguiente Post veremos algo de TypeScript. =)