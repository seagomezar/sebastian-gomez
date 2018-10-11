---
title: Datos en tiempo real con Firebase en Android
tags:
  - Android
  - Firebase
url: 461.html
thumbnailImage: https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/397729-full.png
thumbnailImagePosition: left
id: 461
categories:
  - Firebase
date: 2016-05-18 21:17:32
---

Cuando creamos una aplicación con Android Studio usando Firebase, es importante identificar las opciones que firebase y su librería para Android nos ofrece, esto nos ayudará a sacar el mayor provecho de firebase, en este post voy a mostrarte en detalle como funciona el concepto de “Data Syncronization”.

<!-- more -->

 Si no sabes como utilizar o como dar los primeros pasos con Firebase y Android Studio, te recomiendo que mires este post primero: [Introducción a Firebase para Android](http://www.sebastian-gomez.com/firebase/introduccion-a-firebase-para-android/), allí te enseño como configurar el contexto y todo lo que requieres para empezar correctamente con Firebase en Android Studio. Acceder a los datos de Firebase desde nuestra aplicación es muy simple, simplemente se require declarar una variable de tipo Firebase.

{% tabbed_codeblock  %}
    <!-- tab java -->
        Firebase mRef;
    <!-- endtab -->
{% endtabbed_codeblock %}

Esta variable debe ser inicializada con un objeto de tipo Firebase, cuyo constructor recibe la URL de tu endpoint en firebase.google.com, es decir:

{% tabbed_codeblock  %}
    <!-- tab java -->
       mRootRef = new Firebase("https://miendpointenfirebase.firebaseio.com");
    <!-- endtab -->
{% endtabbed_codeblock %}

Con esto tenemos enlazada nuestra aplicación con el Endpoint de Firebase. Supongamos que adentro de nuestro enpoint tenemos un objeto llamado mensaje, y queremos mostrar el contenido de dicho objeto en nuestra aplicación (Mira de que te hablo: ) [![Screen Shot 2016-05-18 at 4.13.57 PM](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-18-at-4.13.57-PM-300x99.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-18-at-4.13.57-PM.png) [](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-18-at-3.45.55-PM.png) Para mostrar este mensaje en un TextView, podriamos hacer lo siguiente:

1.  Obtenemos la referencia al TextView (Recuerda que esto debe estar en la definición de la clase)
    
{% tabbed_codeblock  %}
    <!-- tab java -->
        mTextView = (TextView)findViewById(R.id.textView);
    <!-- endtab -->
{% endtabbed_codeblock %}
    
    
2.  Obtenemos la referencia al objeto mensaje de nuestro Endpoint, (Esto debe estar en onStart() )

{% tabbed_codeblock  %}
    <!-- tab java -->
        Firebase messagesRef = mRootRef.child("messages");
    <!-- endtab -->
{% endtabbed_codeblock %}
    
3.  Le añadimos un listener que nos permita conocer cuando cambia este mensaje:

{% tabbed_codeblock  %}
    <!-- tab java -->
        messagesRef.addValueEventListener(new ValueEventListener() {
            @Override
            public void onDataChange(DataSnapshot dataSnapshot) {
                //Cada vez que el mensaje cambie, se va a llamar este bloque de codigo
            }
        
            @Override
            public void onCancelled(FirebaseError firebaseError) {
        
            }
        });
    <!-- endtab -->
{% endtabbed_codeblock %}
    
4.  Asignamos el mensaje a nuestro TextView, para eso es necesario añadir exactamente que tipo de dato vamos a recibir, por eso es necesario dentro de la función getValue añadir el String.class:
    
{% tabbed_codeblock  %}
    <!-- tab java -->
        messagesRef.addValueEventListener(new ValueEventListener() {
            @Override
            public void onDataChange(DataSnapshot dataSnapshot) {
                //Cada vez que el mensaje cambie, se va a llamar este bloque de codigo
                String mensaje = dataSnapshot.getValue(String.class);
                mTextView.setText(mensaje);
            }
        
            @Override
            public void onCancelled(FirebaseError firebaseError) {
        
            }
        });
    <!-- endtab -->
{% endtabbed_codeblock %}

De esta manera cada vez que cambia el valor del "mensaje" cambiará automáticamente y en tiempo real en nuestra aplicación.   
![Screen Shot 2016-05-18 at 4.14.50 PM](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-18-at-4.14.50-PM-300x75.png)
![Screen Shot 2016-05-18 at 4.11.25 PM](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-18-at-4.11.25-PM-175x300.png)               

A estas alturas te estarás preguntando para que sirve entonces el método onCancelled, simplemente dicho método se ejecutará si ocurre algún problema tratando de acceder al objeto en nuestro Endpoint. 

Eso es todo, espero que este post te sea de utilidad, Si tienes alguna duda no dudes en dejarme un comentario en la parte de abajo, recuerda que si te gustó también puedes compartir usando los links a las redes sociales en la parte de abajo.