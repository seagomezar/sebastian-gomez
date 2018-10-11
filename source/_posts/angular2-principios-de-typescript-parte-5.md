---
title: Angular, Principios de Typescript Parte 5
tags:
  - Angular
  - Desarrollo Web
  - AngularJS
  - CSS
  - HTML5
  - Javascript
  - TypeScript
url: 361.html
thumbnailImage: https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/11/a1.png
thumbnailImagePosition: left
id: 361
categories:
  - Typescript
date: 2015-11-19 21:04:25
---

Si has seguido mis tutoriales sobre Angular hasta aquí te darás cuenta que he tratado al máximo de evitar el uso de Typescript, de hecho si revisas directamente la documentación de Angular te darás cuenta que de la mayoría de la documentación esta hecha en Typescript. Esto se debe básicamente el equipo de Angular decidio crear Angular sobre TypesScript, TypeScript es una colaboración oficial entre Google y Microsoft con el fin de orientar completamente Javascript a Objetos. 

<!-- more -->

Es decir que podamos usar con Javascript todo el "Paradigma de programación orientada a objetos", como clases, constructores, métodos, Public, Private, Static, etc.

### Que es TypeScript?

Podemos definir TypeScript como un transcopilador de Javascript (WTF?) Si un transcompilador básicamente TypeScript convierte el código TypeScript en código Javascript, sin embargo eso no es todo. Como te he contado en tutoriales pasados existen estándares de Javascript como ECMAScript 5 y ECMAScript 6, ECMAScript 5 es el estándar actual de Javascript que soportan la mayoría de los navegadores actuales, sin embargo ECMAScript 6 mejora el ECMAScript 5 con un montón de nuevas funcionalidades y no es soportado por muchos navegadores. Es por esto que TypeScript es un transcompilador de Javascript es decir que puede transformar tu código TypeScript en código Javascript compatible con ECMAScript 5 o ECMAScript 6.

### Primeros pasos en con TypeScript

En este tutorial nos olvidaremos de Angular2 en si, y nos dedicaremos solo a TypeScript de manera independiente, por tanto nuestro primer paso consistirá en la instalación de TypeScript. Para ello vamos a ejecutar el siguiente comando (Recuerda tener instalado NodeJS).

```
npm install -g typescript
```

Hecho esto ya tenemos TypeScript en nuestro sistema, veamos ahora algunos ejemplos:

#### Variables y funciones Typadas

Simplemente puedes usar tipos de datos es decir  

{% tabbed_codeblock  %}
  <!-- tab js -->
    nombre:string = "Esto es un string";
    edad:number = 43;
    bandera: boolean = true;

    //recibe un string como parámetro y retorna un string como resultado
    function hola(nombre:string):string{
        return "hola"+nombre;
    }
  <!-- endtab -->
{% endtabbed_codeblock %}

#### Arreglos

{% tabbed_codeblock  %}
  <!-- tab js -->
    Empresas: Array<string> = ['IBM', 'Microsoft', 'Google'];
    Empresas: string[] = ['Apple', 'Dell', 'HP'];
  <!-- endtab -->
{% endtabbed_codeblock %}

#### Any

Puedes usar el tipo Any para ponerle un cualquier tipo a una variable, es como un comodin.

{% tabbed_codeblock  %}
  <!-- tab js -->
    algunaCosa: any = 'as string';
    algunaCosa = 1;
    algunaCosa = [1, 2, 3];
  <!-- endtab -->
{% endtabbed_codeblock %}

#### Void

Puedes declarar métodos que no retornan nada.

{% tabbed_codeblock  %}
  <!-- tab js -->
    function setNombre(nombre: string): void {
      this.nombre = nombre;
    }
  <!-- endtab -->
{% endtabbed_codeblock %}

#### Clases y Objetos

Podemos crear clases con atributos, métodos y constructores.

{% tabbed_codeblock  %}
  <!-- tab js -->
    class persona {
      nombre:string;
      edad:number;
      casado:boolean;
      constructor(nombre:string, edad:number, casado:boolean){
          this.nombre = nombre;
          this.edad = edad;
          this.casado = casado;
      }
      myMetodo():string{
          return "Este metodo simplemente retorna un string";
      }
    }
    persona = new Persona("juan",12,false);
  <!-- endtab -->
{% endtabbed_codeblock %}

#### Herencia

Si! Aunque no lo creas puedes usar herencia con TypeScript y es ridículamente fácil:

{% tabbed_codeblock  %}
  <!-- tab js -->
    class Estudiante extends Persona {
      universidad:string;
      constructor(){
          super();
      }
    }
  <!-- endtab -->
{% endtabbed_codeblock %}

### Como usar TypeScript en mis proyectos?

Luego de haber instalado TypesScript debes crear tu archivo nombre.ts nota que termina en .ts porque es la notacion especifica de TypeScript. Una vez tengas esto tu puedes correr el comando

tsc nombre.ts

y veras que te creara un Archivo llamado nombre.js en la misma carpeta donde este tu archivo .ts, y es este archivo el que puede incluir en tus proyectos, independientemente que estés usando Angular2 o no. Otra caracteristica de TypeScript, es que como te dije puedes convertir tu codigo Javascript en ECMAScript 5 o ECMAScript 6, para ello puedes crear un un archivo  tsconfig.json y jugar con las siguientes acciones:

{% tabbed_codeblock  %}
  <!-- tab js -->    
    {
      "compilerOptions": {
        "target": "ES5",  //Puedes elegir tambien ES6 para ECMAScript 6
        "module": "commonjs", //Por defecto
        "sourceMap": true, //Que te cree tambien un archivo.map asociado
        "emitDecoratorMetadata": true, //Que puedas usar el patron decorador
        "experimentalDecorators": true, 
        "removeComments": false, //Que te borre los comentarios
        "noImplicitAny": false
      }
    }
  <!-- endtab -->
{% endtabbed_codeblock %}

Si usas este archivo puede ejecutar simplemente el comando:

```
tsc
```

Así el transformara todos los archivos con extensión .ts a .js sin necesidad de especificarle uno por uno. Y eso es todo, recuerda Este post hace parte de una serie de tutoriales sobre Angular. Aunque no vimos mucho sobre Angular en este Post te aseguro que estos principios nos serviran para continuar en el aprendizaje de Angular a continuación te dejo los links.

[Angular Parte 0](http://www.sebastian-gomez.com/desarrollo-web/introduccion-a-angularjs-parte-0/)

[Qué es AngularJS? Parte 1](http://www.sebastian-gomez.com/desarrollo-web/angular-1-vs-angular-2-parte-2/)

[AngularJS vs Angular? Parte 2](http://www.sebastian-gomez.com/desarrollo-web/angular-1-vs-angular-2-parte-2/)

[Hola Mundo con Angular Parte 3](http://www.sebastian-gomez.com/desarrollo-web/angular-2-con-javascript-hola-mundo-parte-3/)

[Angular aplicación que saluda Parte 4](http://www.sebastian-gomez.com/desarrollo-web/ angular-2-aplicacion-que-saluda-parte-4 /)

Si tienes alguna duda no dudes en dejarme un comentario en la parte de abajo, recuerda que si te gustó también puedes compartir usando los links a las redes sociales en la parte de abajo.