---
title: AngularJS vs Angular Parte 2
tags:
  - Angular
  - AngularJS
  - CSS
  - HTML5
  - Javascript
  - Desarrollo Web
url: 316.html
thumbnailImage: https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/11/a1.png
thumbnailImagePosition: left
id: 316
categories:
  - Angular
date: 2015-10-31 21:51:18
---

AngularJS en su versión 1 se define a si mismo como un conjunto de librerías de Javascript para la creación de aplicaciones web, mientras que Angular 2 se define a si mismo como una plataforma para creación de aplicaciones web y aplicaciones móviles. Estas son las respectivas definiciones que nos encontramos en la documentación oficial de dichas plataformas:

<!-- more -->

Documentación oficial [AngularJS](https://angularjs.org/)

Documentación oficial [Angular 2](https://angular.io/)

Pero analicemos un poco con más detalle en que consiste este cambio en las definiciones sobre que es Angular, en primer lugar se puede notar que en el sitio web de Angular 2 (angular.io)  no tiene el sufijo JS, esto significa que dejaremos de llamar a AngularJS como es conocido en su version 1.x por simplemente Angular, esto se debe básicamente a que han cambiado un poco su filosofía tratando de llevar Angular a un nivel superior ya que permite soportar de entrada TypeScript, Javascript y Dart, es decir en cualquiera de estos tres lenguajes, es posible trabajar con Angular de manera similar y trasparente. Segundo la definición de AngularJS (es decir la versión 1 de Angular) no incluye por ningún lado la frase "Mobile Applications" mientras que en la versión dos incluye explícitamente esta frase, esto significa que estarán enfocados en dar mas soporte a la construcción de aplicaciones móviles híbridas mejorando en general el desempeño y el uso de Angular en los dispositivos móviles, en temas como la memoria, el uso de red, el material design, entre otras cosas. Sin embargo también se espera que se mejore la interacción con Ionic (El popular framework para la creación de aplicaciones móviles híbridas basado en AngularJS), oficialmente en la página de Ionic, anuncian la versión dos que va de la mano con Angular.

#### Historia

Hace un tiempo atrás el equipo de angular tomo la decisión de cambiar drásticamente a AngularJS, Google y en general los developers de angular querían un mejor framework con una curva de aprendizaje menor y una serie de mejoras de desempeño abismales, esto lo anunciaron en el ng-conf 2015. Desde entonces el team de Angular ha hecho al rededor de 44 releases sobre esta nueva versión de angular llamada Angular 2 en adelante Angular.

Pero cuales son las tan anheladas mejoras o diferencias respecto a AngularJS que se han tratado de materializar en Angular:

Component-Based UI, Este concepto es familiar para aquellos desarrolladores que trabajan en ReactJS, sin embargo si nunca has trabajado con ReactJS no te preocupes yo te lo explicaré fácilmente, Angular no posee controladores ni directivas como si lo tiene AngularJS para comunicarse con la vista, en vez de eso Angular utilizará un concepto llamado componentes y dentro de esos componentes tu encontraras un selector es decir un elemento dentro del Document Object Model, que será el elemento a manipular por el componente.

Así se hacía en AngularJS

{% tabbed_codeblock  %}
  <!-- tab js -->
    angular.module(‘example’)
      .controller(‘ExampleCtrl’, function() {
    });
  <!-- endtab -->
{% endtabbed_codeblock %}

Así se hace en Angular

{% tabbed_codeblock  %}
  <!-- tab js -->
    var AppComponent = ng.Component({
      selector: 'my-app',
      template: '<h1>My First Angular 2 App</h1>'
    })
    .Class({
      constructor: function () { }
    });
  <!-- endtab -->
{% endtabbed_codeblock %}

Sintaxís de eventos asociados a los inputs: Ahora las aplicaciones hechas en Angular 2 permiten que escribas el evento disparador entre paréntesis, por ejemplo en AngularJS tu debías escribir:

 Así se hace en AngularJS:

{% tabbed_codeblock  %}
  <!-- tab html -->
    <button ng-click="thing.submit(item)" type="submit">
  <!-- endtab -->
{% endtabbed_codeblock %}

Mientras que en Angular 2 ya lo harás así:

{% tabbed_codeblock  %}
  <!-- tab html -->
    <button (click)="submit(item)" type="submit">
  <!-- endtab -->
{% endtabbed_codeblock %}

Adiós al $scope

El famoso $scope que todos amamos y utilizamos en AngularJS se ha remplazado por el Controller As, y esto era evidente que iba a suceder debido a que desde la versión 1.2 de AngularJS estaban recomendando como buena práctica sustituirlo, como era antes?

Así lo hacías en AngularJS:

{% tabbed_codeblock  %}
  <!-- tab js -->
    angular.module(‘example’).controller(‘ExampleCtrl’, function($scope) {
        $scope.name = “John Smith”;
    });
  <!-- endtab -->
{% endtabbed_codeblock %}

Así se hace en Angular:

{% tabbed_codeblock  %}
  <!-- tab js -->
    (function() {
      var AppComponent = ng
      .Component({
        selector: 'my-app',
        template: '<h1>My First Angular 2 App</h1>'
      })
      .Class({
        constructor: function () {
          this.name = "John Smith";
        }
      })();
  <!-- endtab -->
{% endtabbed_codeblock %}

El mejor desempeño: Esta ha sido una de las más grandes diferencias que se han anunciado con Angular 2 se habla mucho de su desempeño, y sus nuevas características asociadas como "ultra fast change detection" y "estructuras inmutables", esto se debe a que ya no existe mas un "two-way data binding" (Doble enlazamiento de los datos) ya que entre otras cosas el concepto de ng-model también ha desaparecido.

Recuerda que este post pertenece a una serie de tutoriales sobre Angular 2 desde cero, te invito a que revises mis anteriores posts:

[AngularJS 2 Parte 0](http://www.sebastian-gomez.com/desarrollo-web/introduccion-a-angularjs-parte-0/)

[Qué es AngularJS? Parte 1](http://www.sebastian-gomez.com/desarrollo-web/que-es-angularjs-parte-1/)

o si vienes siguiente mis tutoriales, que avances al siguiente:

[Hola mundo en Angular 2 Parte3](http://www.sebastian-gomez.com/desarrollo-web/angular-2-con-javascript-hola-mundo-parte-3/)

Escríbeme si tienes alguna duda y no olvides si te ha gustado este post compartelo!