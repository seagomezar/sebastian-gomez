---
title: Simple Login Con Angular 2 Parte 6
tags:
  - Angular
  - CSS
  - HTML5
  - Javascript
  - TypeScript
url: 368.html
thumbnailImage: https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/11/a1.png
thumbnailImagePosition: left
id: 368
categories:
  - Angular
date: 2015-11-26 21:22:49
---

En este post haremos un sistema de login básico con Angular 2: El sistema de login que desarrollaremos en este tutorial consiste en que solo mostraremos un contenido, si el usuario se encuentra logueado en nuestro sistema para ello utilizaremos un usuario por defecto que responde a las credenciales username: test password: test. 
<!-- more -->
Así funcionará nuestro sistema de login una vez desarrollado:

 ![http://www.sebastian-gomez.com/shared/LogginSampleApp.gif](http://www.sebastian-gomez.com/shared/LogginSampleApp.gif)

En primer lugar revisemos las dependencias que tendrá nuestra aplicación y como sera la estructura de nuestro proyecto:![LoginAppStructure](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/11/LoginAppStructure.png)

Como podrás notar respecto a nuestros anteriores proyectos, esta vez tenemos extensiones diferentes tales como .ts que hace referencia a typescript, y tsconfig.json donde estará la configuración de Typescript.

Recuerda que si no sabes que es typescript, como instalarlo y como funciona te invito a que visites mi [anterior post](http://www.sebastian-gomez.com/desarrollo-web/angular2-principios-de-typescript-parte-5/) donde hablo sobre lo esencial de typescript y como usarlo en tus proyectos (inclusive si son proyectos sin Angular). Voy a suponer que ya sabes sobre esto así que manos a la obra, en primer lugar te mostrare el contenido de los archivos principales de nuestra aplicación, estos son package.json donde definimos las librerías y los comandos de nodeJS que son útiles para nuestro proyecto:

{% tabbed_codeblock package.json %}
    <!-- tab js -->
      {
        "name": "Login Angular 2",
        "version": "1.0.0",
        "description": "",
        "main": "index.js",
        "dependencies": {
          "angular2": "2.0.0-alpha.44",
          "es6-shim": "^0.33.13",
          "systemjs": "0.19.2"
        },
        "devDependencies": {
          "live-server": "^0.8.2",
          "typescript": "^1.6.2"
        },
        "scripts": {
          "tsc": "tsc -p src -w",
          "start": "live-server --open=src"
        },
        "keywords": [],
        "author": "",
        "license": "ISC"
      }
    <!-- endtab -->
{% endtabbed_codeblock %}

tsconfig.json donde creamos y definimos nuestra configuración de typescript para nuestro proyecto:

{% tabbed_codeblock  tsconfig.json %}
    <!-- tab js -->
      {
        "compilerOptions": {
          "target": "ES5",
          "module": "commonjs",
          "sourceMap": true,
          "emitDecoratorMetadata": true,
          "experimentalDecorators": true,
          "removeComments": false,
          "noImplicitAny": false
        }
      }
    <!-- endtab -->
{% endtabbed_codeblock %}

Y finalmente  index.html donde incluimos nuestras dependencias e incluimos el componente principal de nuestra aplicación.

{% tabbed_codeblock index.html %}
    <!-- tab html -->
      <html>
        <head>
          <title>Angular 2 Login</title>
          <script src="../node_modules/es6-shim/es6-shim.js"></script>
          <script src="../node_modules/systemjs/dist/system.src.js"></script>
          <script src="../node_modules/angular2/bundles/angular2.dev.js"></script>
          <script>
            System.config({
              packages: {'app': {defaultExtension: 'js'}}
            });
            System.import('app/app');
          </script>
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
        </head>
        <body>
          <main>loading...</main>
        </body>
      </html>
    <!-- endtab -->
{% endtabbed_codeblock %}

No me detendré explicando estos tres archivos anteriores dado que lo he repetido en los tutoriales anteriores sin embargo notemos una particularidad en index.html en las lineas 8-12, allí estamos diciendo que incluya el archivo app.js a nuestra aplicación, sin embargo notaras que en ningún momento hemos hablado de un archivo app.js. Básicamente el archivo app.js sera un archivo creado automáticamente por typescript y estará ubicado dentro de la carpeta app, nosotros en ningún momento deberíamos preocuparnos por su contenido.

Es tiempo de definir nuestras vistas, como viste en la estructura del proyecto al inicio de este post te darás cuenta que tenemos una carpeta views y dentro de esta tendremos un archivo main.html y otro archivo llamado protected-component.html, a continuación te muestro su contenido:

{% tabbed_codeblock  main.html %}
    <!-- tab html -->
      <section class="login-component" *ng-if="!isLogged">
        <div class="control-group">
          <div>
            <label for="title">Username:</label>
          </div>
          <div>
            <input name="username" #username>
          </div>
        </div>
        <div class="control-group">
          <div>
            <label for="link">Password:</label>
          </div>
          <div>
            <input type="password" name="password" #password>
          </div>
        </div>
        <button class="" (click)="login(username, password)">Login</button>
      </section>
      <protected-component *ng-if="isLogged" ></protected-component>
      <section class="logout" *ng-if="isLogged"><button (click)="logout()">Logout</button>
    <!-- endtab -->
{% endtabbed_codeblock %}

{% tabbed_codeblock  protected-component.html %}
    <!-- tab html -->
      <h2> Este es un componente protegido</h2>
    <!-- endtab -->
{% endtabbed_codeblock %}

Verás que el archivo main.html tiene tres partes una primera sección donde tenemos un cuadro de login que se muestra únicamente si el usuario no esta logueado, luego una etiqueta llamada <protected-component> que sera el componente que únicamente es accesible para los usuarios logueados. Finalmente tenemos un div que nos permitirá desloguearnos y se mostrará únicamente cuando el usuario esta logueado. Por su parte el archivo protected-component.html no es mas que una simple línea de código donde ponemos un texto protegido.

Es tiempo entonces de crear nuestro archivo principal (Lo bueno usualmente viene de ultimo), nuestro archivo principal es el archivo app.ts y su contenido es el siguiente:

{% tabbed_codeblock app.ts %}
  <!-- tab js -->
    import {bootstrap, Component, View, NgIf} from 'angular2/angular2';
    //Protected-Content Component
    @Component({
      selector: 'protected-component'
    })
    @View({
      templateUrl: 'app/views/protected-component.html'
    })
    class ProtectedComponent{
    }

    //Main Component
    @Component({
      selector: 'main'
    })
    @View({
      templateUrl: 'app/views/main.html',
      directives:\[ProtectedComponent,NgIf\]
    })
    class Main{
      isLogged:boolean;
      constructor(){
        this.isLogged = false;
      }
      login(username, password){
        if(username.value =="test" && password.value=="test"){
          this.isLogged = true;
        }
      }
      logout(){
        this.isLogged = false;
      }
    }
    bootstrap(Main);
  <!-- endtab -->
{% endtabbed_codeblock %}

Al ser este nuestro archivo principal te lo explicare línea por línea:

Línea 1: Importamos los componentes de angular 2 que necesitamos para crear nuestra aplicación, estos suelen ser mas o menos estándar en nuestro proyectos, nota que hemos importado bootstrap que si te fijas la ultima linea es el que nos permitirá incrustar nuestro componente principal en nuestra aplicación, también hemos incluido components y vistas ya que por defecto es lo mínimo que necesitamos para una aplicación de angular 2 usualmente todos proyectos de angular 2 incluyen dichos elementos, finalmente tenemos la inclusión de la directiva NgIf que nos permitirá usar condicionales en las vistas para mostrar uno u otro contenido.

Línea 4 y 5: Es la definición de nuestro componente y la etiqueta por medio de la cual podremos invocamos a dicho componente desde el archivo principal index.html u otro componente.

Línea 7 y 8: Definimos la vista del componente, en este caso simplemente incluimos el archivo protected-component.html desde la ruta.

Línea 10 y 11: Es donde programamos la lógica de nuestro componente, pero en nuestro caso como es un simple texto no tiene ninguna lógica asociada.

Línea 14 y 15: Allí definimos nuestro segundo componente y la etiqueta, como verás en el archivo index.html allí si invocamos directamente nuestro componente desde su selector.

Línea 17 y 18: Nada expecial simplemente incluimos el archivo html o codigo html de nuestro componente desde la ruta especificada app/views/main.html.

Línea 19: Es donde incluimos componentes internos o externos de nuestra aplicación para usarlos en nuestro componente actual, es decir en esta línea estamos diciendo que nuestro componente main podría utilizar el componente <protected-component>.

Línea 21 a 31: Es donde definimos que atributos tiene nuestro componente, en nuestro caso particular tenemos un solo atributo boolean llamado 'isLogged' donde guardamos el estado de nuestro usuario en nuestra aplicación, como veras este atributo en el constructor es instanciado a falso, (Recuerda que el constructor es el código que se ejecuta al crear el componente), Luego tenemos dos funciones muy sencillas, una función llamada login que dado el caso que el usuario sea 'test' y la contraseña sea 'test' entonces cambiara el valor de la variable isLogged a verdadero, de lo contrario no pasa nada. La segunda función logout, que se ejecuta al pulsar el botón simplemente vuelve el valor de 'isLogged' a false.

Eso es todo ahora necesitamos poner a funcional nuestra aplicación. En primer lugar debemos instalar nuestras librerias de nodeJS y para eso usualmente usamos el comando **npm install**, luego en una terminal podremos ejecutar **npm start**. Sin embargo te darás cuenta que no funciona, y esto es porque como te dije al principio de este tutorial debemos generar nuestro archivo app.js a partir del archivo app.ts que hemos escrito, para ello simplemente escribes en el terminal **tsc -p src -w** y con esto se generara tu archivo app.js y ademas quedará esperando cambios en el archivo .ts dentro de tu aplicación. Hecho esto ya deberías tener en funcionamiento tu aplicación.

Y eso es todo, recuerda Este post hace parte de una serie de tutoriales sobre Angular2. A continuación te dejo los links de los anteriores posts, por si te has perdido alguno:

[AngularJS 2 Parte 0](http://www.sebastian-gomez.com/desarrollo-web/introduccion-a-angularjs-parte-0/)

[Qué es AngularJS? Parte 1](http://www.sebastian-gomez.com/desarrollo-web/angular-1-vs-angular-2-parte-2/)

[AngularJS 1 vs Angular 2? Parte 2](http://www.sebastian-gomez.com/desarrollo-web/angular-1-vs-angular-2-parte-2/)

[Hola Mundo con Angular 2 Parte 3](http://www.sebastian-gomez.com/desarrollo-web/angular-2-con-javascript-hola-mundo-parte-3/)

[Angular 2 aplicación que saluda Parte 4](http://www.sebastian-gomez.com/desarrollo-web/ angular-2-aplicacion-que-saluda-parte-4 /)

[Angular 2 Principios de TypeScript Parte 5](http://www.sebastian-gomez.com/desarrollo-web/angular2-principios-de-typescript-parte-5/)

Si tienes alguna duda no dudes en dejarme un comentario en la parte de abajo, recuerda que si te gustó también puedes compartir usando los links a las redes sociales en la parte de abajo.