---
title: Angular con Javascript Hola Mundo Parte 3
tags:
  - Angular
  - Desarrollo web
  - AngularJS
  - CSS
  - HTML5
  - Javascript
url: 325.html
thumbnailImage: https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/11/a1.png
thumbnailImagePosition: left
id: 325
categories:
  - Angular
date: 2015-11-03 22:51:32
---

En este post construiremos una sencilla aplicación en Angular, que nos permita empezar a comprender el funcionamiento de Angular, y como usarlo. La aplicación consistirá simplemente en el típico hola mundo que es el proyecto insignia para iniciar el aprendizaje de un lenguaje de programación. <!-- more -->

[![Angular](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/11/Angular.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/11/Angular.png)

Recuerda que mucha de la documentación oficial ofrecida por el equipo de Angular en su versión 2 aún se encuentra en desarrollo. Sin embargo en cualquier momento puedes ir a: [Su página](https://angular.io/).

Bien, antes de empezar debemos tener instalado Node.js, ¿Porqué?, en primer lugar porque desde allí bajaremos los archivos de Angular2, y segundo porque también nos interesa obtener un live-server para desarrollar de una manera más cómoda. [Este es el link para desacargar e instalar Node.js](https://nodejs.org/en/).

Luego que tengas Node.js instalado, debemos crear la carpeta donde tendremos nuestro proyecto: en mi caso se llama hola-angular2, adentro de esta carpeta vamos a crear nuestra aplicación por tanto vamos inicializar nuestro proyecto, para ello vamos a abrir nuestra terminal, git bash o cmd y vamos a escribir:

{% tabbed_codeblock  %}
  <!-- tab js -->
    npm init -y
  <!-- endtab -->
{% endtabbed_codeblock %}

La línea anterior hace parte de la sintaxis de Node.js ya que npm significa Node Package Manager, este línea creará un archivo llamado package.json, en este archivo nosotros debemos decirle a Node.js como se llama nuestro proyecto, darle una descripción e incluir las librerías externas que necesitamos, como te sugiero que quede tu archivo es de la siguiente manera:

{% tabbed_codeblock  %}
  <!-- tab js -->
    {
      "name": "hola-angular2",
      "version": "1.0.0",
      "description": "Esta es una aplicación para enseñar angular 2",
      "main": "app.js",
      "scripts": {
        "start": "live-server"
      },
      "keywords": \[\],
      "author": "Sebastian Gomez",
      "license": "ISC",
      "dependencies": {
        "angular2": "2.0.0-alpha.44"
      },
      "devDependencies": {
        "live-server": "^0.8.2"
      }
    }
  <!-- endtab -->
{% endtabbed_codeblock %}

Analicemos con mas detalle este archivo, en la línea dos definimos el nombre de nuestro proyecto, luego una descripción, en la línea 5 definimos cual será el archivo principal de nuestra aplicación y vemos que he escrito app.js, esto quiere decir que este es el punto de entrada nuestra aplicación y luego deberemos crear este archivo, luego en la línea 6 y 7 definimos un comando de node llamado "start" eso quiere decir que luego podremos escribir npm start y node intentará ejecutar "live-server", pero que significa live server bueno como veras en las dev dependencies ahi escribi que quiero que node descargue un paquete llamado live-server, que básicamente lo que hará será escanear el contenido de la carpeta donde este el package.json y mostrar el contenido en el navegador, esto se quedará en ejecución en busca de cambios y si algo cambia refrescará el browser automáticamente, piénsalo como un pequeño servidor que se actualiza ante cualquier cambio que haya en los archivos al interior de nuestro proyecto.

Como vez e incluido como dependencias de nuestra aplicación linea 12 y 13, a Angujar 2 en su release (como sub-versión) 44, al estar en alpha quiere decir que aún no es la versión final pero que está lista para que los desarrolladores empiecen a aprender y a probar las funcionalidades. Una vez definamos esto en nuestro archivo package.json debemos ejecutar el siguiente mando para que NodeJs instale las dependencias:

{% tabbed_codeblock  %}
  <!-- tab js -->
    npm install
  <!-- endtab -->
{% endtabbed_codeblock %}

Y obtendremos un mensaje como el siguiente:

[![h3](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/11/h3.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/11/h3.png)

Bien hecho esto estamos listos para empezar a desarrollar nuestra aplicación. En primer lugar vamos a crear nuestro archivo app.js.

{% tabbed_codeblock app.js %}
  <!-- tab js -->
    (function() {
      var AppTitle = ng.Component({
          selector: 'cabecera',
          template: '<h1>Hola Mundo</h1>'
        })
        .Class({
          constructor: function () { }
        });
      document.addEventListener('DOMContentLoaded', function() {
        ng.bootstrap(AppTitle);
      });
    })();
  <!-- endtab -->
{% endtabbed_codeblock %}

Si alguna vez trabajaste con AngularJs en su versión 1 te darás cuenta que este archivo no es parecido en nada a lo que se trabajaba en dichar versión, por tanto debemos tomarnos con calma este archivo y tratar de entenderlo, asi que trataré de explicártelo lo mas específicamente posible línea por línea.

Línea 1: Simplemente estamos rodeando todo el código de una función IIFE, esto significa Inmediated Invoked Function Execution que como su nombre lo indica será ejecutada tan pronto el archivo sea cargado o invocado desde un archivo html.

Línea 2: Estamos declarando un componente de Angular 2, esta es la sintaxis para declarar componentes, los componentes son porciones de Javascript que opera sobre un template específico y que son utilizados desde los archivos html con una etiqueta o selector.

Línea 3: Será el nombre del selector o etiqueta que utilizaremos para incrustar nuestro componente en los archivos html.

Línea 4: Es el código html que incrustaremos en los archivos html que invoquen nuestro componente desde el selector.

Línea 6: Es una clase, si, una clase, una de las nuevas caracteristicas que tiene Angular 2 es que está Basado en el ECMA script 6 y este nuevo estandar de Javascript ya soporta la palabra Class, dentro de la Class del componente se definirá el código Javascript que quieras que controle la aplicación y como quieras que lo haga, puedes definir atributos, constructores, métodos, etc etc, si quieres puedes aplicar todo el paradigma de la programación orientada a objetos allí mismo.

Línea 7: Como te lo prometí en la linea anterior, dentro de la clase puede crear metodos, variables y constructores, esta es la sintaxis para definir el constructor, es decir código que quieres que se ejecute cuando se cree el componente, para nuestro ejemplo está vació pero puedes escribir algo de codigo allí.

Línea 9: Estamos añadiendo un listener al documento, es decir cuando se haya cargado todo el DOM (Document Object Model) se cargue nuestro componente, por si algun archivo .html lo invoca.

Línea 11: Finalmente cerramos nuestra función IIFE.

Si has entendido lo anterior, empiezas a entender Angular 2, porque esa es su filosofía, todo está basado en web components pero web components orientados a objetos, sin embargo aún nos falta algo más para tener nuestra primera versión de la aplicación funcionando y es algún html que llame nuestro componente, porque si no es así no podremos ver que es lo que hemos hecho. Para ello te invito a que crees un archivo llamado index.html que contenga lo siguiente:

{% tabbed_codeblock app.html %}
  <!-- tab html -->
    <html>
      <head>
        <title>Aprendiendo Angular 2</title>
        <script src="node_modules/angular2/bundles/angular2.sfx.dev.js"></script>
        <script src="app.js"></script>
      </head>
      <body>
        <cabecera></cabecera>
      </body>
    </html>
  <!-- endtab -->
{% endtabbed_codeblock %}

Nuevamente trataré de explicarte el código detalladamente. En la línea 4 estamos incluyendo la librería de Angular 2, si te das cuenta al haber hecho npm install, NodeJs descargó estos archivos en una carpeta node_modules y ahi encontrarás Angular2. En la línea 5 incluimos nuestro archivo app.js el que elaboramos anteriormente. Y por fin en la línea 8 hemos incluido nuestro angular component. Si guardas todo deberías tener una estructura como esta:

[![h1](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/11/h1.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/11/h1.png)

![](file:///C:/Users/Sebas/AppData/Local/Temp/enhtmlclip/Image.png) Si es así estas listo y puedes corres el comando:

{% tabbed_codeblock %}
  <!-- tab js -->
    npm start
  <!-- endtab -->
{% endtabbed_codeblock %}

Luego de un momento te debería cargar una ventana en tu browser como la siguiente:

[![h2](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/11/h2.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/11/h2.png)

Y eso es todo, te felicito, has creado tu primera aplicación usando Angular, si tienes alguna duda no dudes en dejarme un comentario en la parte de abajo, recuerda que si te gustó también puedes compartir usando los links a las redes sociales en la parte de abajo. Este post hace parte de una serie de tutoriales sobre Angular2, a continuación te dejo los links.

[Angular 2 Parte 0](http://www.sebastian-gomez.com/desarrollo-web/introduccion-a-angularjs-parte-0/)

[Qué es AngularJS? Parte 1](http://www.sebastian-gomez.com/desarrollo-web/angular-1-vs-angular-2-parte-2/)

[AngularJS vs Angular? Parte 2](http://www.sebastian-gomez.com/desarrollo-web/angular-1-vs-angular-2-parte-2/)