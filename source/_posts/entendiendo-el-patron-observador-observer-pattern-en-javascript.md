---
title: Entendiendo el patrón observador "Observer Pattern" en Javascript
tags:
  - Desarrollo Web
  - Javascript
url: 552.html
id: 552
thumbnailImage: https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2018/02/javascript-736400_960_720.png?resize=150%2C150&ssl=1
thumbnailImagePosition: left
categories:
  - Javascript
date: 2018-02-21 10:03:59
---

En este post aprenderemos como implementar el patrón observer en Javascript y en que situaciones lo podemos usar.
<!-- excerpt -->

El patrón observador es uno de los patrones de diseño de software más usado en Javascript. De el se extienden importantes aplicaciones que pueden ayudar a definir mejores arquitecturas en aplicaciones web, por lo que su uso y estudio es altamente recomendado. En este post aprenderemos como implementarlo en Javascript y en que situaciones lo podemos usar.

En primer lugar el patrón observador se define como un patrón de comportamiento es decir que dentro del mundo de la programación orientada a objetos es un patrón responsable por la comunicación entre objetos.

En segundo lugar el patrón observador también puedes encontrarlo como el patrón publicador-subscriptor o modelo-patrón y nos da una idea básica de lo que hace. En términos simples este patrón permite la notificación a objetos (subscriptores u observador) al cambio de otro objeto (publicador).

Veamos entonces un pequeño diagrama con las responsabilidades de los involucrados en este patrón. En primer lugar el publicador debe notificar a los subscriptores o observadores que algo cambió, y en segundo lugar los subscriptores deben poder subscribirse o de suscribirse del publicador en cualquier momento:

[![](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2018/02/A004581F-4C80-4ADA-A5A5-A298253AF072.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2018/02/A004581F-4C80-4ADA-A5A5-A298253AF072.png)

¿Cómo podemos entonces implementarlo en Javascript?, empecemos implementando una clase llamada Publicador que contenga los métodos subscribe(), unsubscribe(), y notify().

{% tabbed_codeblock  %}
  <!-- tab js -->
    class Publicador {
  
        constructor() {
          this.subscriptors = \[\];
        }
    
        subscribe(subscriptor) {
          this.subscriptors.push(subscriptor);
        }
      
        unsubscribe(subscriptor) {
          this.subscriptors = this.subscriptors.filter( item => item !== subscriptor );
        }
      
        notify(event) {
          this.subscriptors.forEach( item => {
            item.call(this, event); 
          });
        }  
    }
  <!-- endtab -->
{% endtabbed_codeblock %}

Como verás hemos manejado la lista de subscriptores con un array de Javascript como propiedad de la clase así es posible fácilmente subscribir y des-subscribir subscriptores. También la función notify itera sobre cada uno de los subscriptores y se encarga de invocarlos con el evento.

Ahora imaginemos que usaremos esta definición de Publicador para un periódico que regularmente publica nuevas ediciones.

{% tabbed_codeblock  %}
  <!-- tab js -->
    const periodico = new Publicador();
  <!-- endtab -->
{% endtabbed_codeblock %}

Bien, pensemos un poco más acerca de los clientes. Estos van a necesitar saber cuando llegue una nueva versión del periódico. Inicialmente pensemos en que los clientes son funciones:

{% tabbed_codeblock  %}
  <!-- tab js -->
    function Observer(edicion) {
      console.log("LLegó una nueva edición con el nombre de: " + edicion);
    }

    periodico.subscribe(Observer);
    periodico.subscribe(Observer); 
    periodico.notify("Nueva edicion");
  <!-- endtab -->
{% endtabbed_codeblock %}

Con esta definición anterior al ejecutarlo obtenemos algo así:

"LLegó una nueva edición con el nombre de: Nueva edicion"  
"LLegó una nueva edición con el nombre de: Nueva edicion"

Si queremos ser conscientes de que cliente recibió que edición del periódico podemos retocar un poco las definiciones de la función notify y de la función observer:

{% tabbed_codeblock  %}
  <!-- tab js -->
    class Publicador {
      ...
      notify(event) {
        let index = 0;
        this.subscriptors.forEach( item => {
          item.call(this, index, event); 
          index++;
        });
      }  
      ...
    }
    ...
    function Observer(index, edicion) {
      console.log("Al Observador #" + 
                  index + " le llegó una nueva edición con el nombre de: " + 
                  edicion);
    }
  <!-- endtab -->
{% endtabbed_codeblock %}

De esta manera tenemos como output algo mucho mas entendible:

{% tabbed_codeblock  %}
  <!-- tab js -->
    "Al Observador #0 le llegó una nueva edición con el nombre de: Nueva edicion"  
    "Al Observador #1 le llegó una nueva edición con el nombre de: Nueva edicion"
  <!-- endtab -->
{% endtabbed_codeblock %}

Sin embargo como un mejor acercamiento podríamos definir una clase para los clientes que nos permita crear instancias de ella y tener un control mas granular. También debemos definir un método únicamente diseñado para escuchar por nuevas decisiones del periódico.

{% tabbed_codeblock  %}
  <!-- tab js -->
    class Observador {
      constructor(id) {
        this.id = id;
        console.log("Se ha creado el subscriptor #: " + id);
      }
      buzon(edicion) {
        console.log("Subscriptor # " + this.id + " recibió una nueva edición: " + edicion);
      }
    }

    const subscriptor1 = new Subscriptor(1);
    const subscriptor2 = new Subscriptor(2);
    const subscriptor3 = new Subscriptor(3);
  <!-- endtab -->
{% endtabbed_codeblock %}

De esta forma podemos tener más control sobre los subscriptores y podemos subscribirlos y des-subscribirlos de mejor manera. A continuación verás como publicar multiples ediciones de un perioido así como la habilidad suscribir y des-suscribir clientes:

{% tabbed_codeblock  %}
  <!-- tab js -->
    periodico.subscribe(subscriptor1);
    periodico.subscribe(subscriptor2);
    periodico.notify("Nueva edicion");
    periodico.subscribe(subscriptor3); 
    periodico.notify("Segunda edicion"); 
    periodico.unsubscribe(subscriptor1); 
    periodico.notify("Tercera edicion"); 
    periodico.subscribe(subscriptor2);
    periodico.notify("Nueva edicion");
    periodico.subscribe(subscriptor3); 
    periodico.notify("Segunda edicion"); 
    periodico.unsubscribe(subscriptor1); 
    periodico.notify("Tercera edicion");
  <!-- endtab -->
{% endtabbed_codeblock %}

Y obtenemos la siguiente salida:

{% tabbed_codeblock  %}
  <!-- tab js -->
    "--- Primera edición ---"

    "Subscriptor # 1 recibió una nueva edición: Nueva edicion"

    "Subscriptor # 2 recibió una nueva edición: Nueva edicion"

    "--- Segunda edición ---"

    "Subscriptor # 1 recibió una nueva edición: Segunda edicion"

    "Subscriptor # 2 recibió una nueva edición: Segunda edicion"

    "Subscriptor # 3 recibió una nueva edición: Segunda edicion"

    "--- Tercera edición ---"

    "Subscriptor # 2 recibió una nueva edición: Tercera edicion"

    "Subscriptor # 3 recibió una nueva edición: Tercera edicion"
  <!-- endtab -->
{% endtabbed_codeblock %}

De esta manera queda totalmente completo el patrón observer. Como vez es muy fácil implementar el patrón observer y su utilidad es casi inmediata. A continuación podrás observar todo el código completo de este ejemplo:

{% tabbed_codeblock  %}
  <!-- tab js -->
    class Publicador {
      constructor() {
        this.subscriptors = \[\];
      }
  
      subscribe(subscriptor) {
        this.subscriptors.push(subscriptor);
      }
    
      unsubscribe(subscriptor) {
        this.subscriptors = this.subscriptors.filter( item => item !== subscriptor );
      }
    
      notify(event) {
        this.subscriptors.forEach( item => {
          item.buzon.call(item, event);
        });
      }  
  }

  class Subscriptor {
    constructor(id) {
      this.id = id;
      console.log("Se ha creado el subscriptor #: " + id);
    }
    buzon(edicion) {
      console.log("Subscriptor # " + this.id + " recibió una nueva edición: " + edicion);
    }
  }

  const periodico = new Publicador();

  const subscriptor1 = new Subscriptor(1);
  const subscriptor2 = new Subscriptor(2);
  const subscriptor3 = new Subscriptor(3);

  console.log("--- Primera edición ---");

  periodico.subscribe(subscriptor1);

  periodico.subscribe(subscriptor2);

  periodico.notify("Nueva edicion");

  console.log("--- Segunda edición ---");

  periodico.subscribe(subscriptor3); 

  periodico.notify("Segunda edicion"); 

  console.log("--- Tercera edición ---");

  periodico.unsubscribe(subscriptor1); 

  periodico.notify("Tercera edicion");
  <!-- endtab -->
{% endtabbed_codeblock %}

Eso es todo, espero que este post te sea de utilidad y lo puedas aplicar a algún proyecto que tengas en mente y que simplemente te haya ayudado a entender la naturaleza del patrón observer. déjame un comentario si lograste implementarlo, si quieres añadir alguna otra funcionalidad o si tienes alguna duda no dudes en dejarme un comentario en la parte de abajo, recuerda que si te gustó también puedes compartir usando los links a las redes sociales en la parte de abajo.