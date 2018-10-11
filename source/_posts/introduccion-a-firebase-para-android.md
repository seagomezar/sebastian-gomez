---
title: Introducción a Firebase para Android
tags:
  - Android
  - Firebase
url: 434.html
thumbnailImage: https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/397729-full.png
thumbnailImagePosition: left
id: 434
categories:
  - Firebase
date: 2016-05-10 20:43:57
---

Firebase es lo que se conoce como “Backend como Servicio”, que básicamente provee de una API para guardar y sincronizar datos en la nube en tiempo real, ya sea desde un sitio web, una aplicación móvil híbrida o una aplicación móvil nativa. 
En este post veremos como crear una aplicación móvil con Android y Firebase.
<!-- more -->

Entre todas las características que ofrece Firebase, las principales son:

*   Muy fácil implementación.
*   Sincronización instantánea.
*   Integración con Servicios de Autenticación.

Firebase se convierte en una opción importante a considerar cuando queremos crear un prototipo muy rápido y no tenemos el tiempo o los conocimientos para desarrollar un backend o una API propia con alguno de los lenguajes tradicionales como PHP, Python, RoR, “Node.Js” (no tan tradicional).

No voy a explicarte como abrir una cuenta en Firebase, ni como Usarlo, ya que esto lo explico en este post, para este post partiremos que tu ya tienes una cuenta creada en Firebase y un Endpoint ( [https://miendpointenfirebase.firebaseio.com/](https://miendpointenfirebase.firebaseio.com/)). También debes tener instalado Android Studio.

Manos a la obra lo primero que haremos será crear un nuevo proyecto en Android Studio y una Actividad en Blanco:[![Screen Shot 2016-05-02 at 10.46.48 AM](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-02-at-10.46.48-AM-276x300.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-02-at-10.46.48-AM.png) [![Screen Shot 2016-05-02 at 10.47.32 AM](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-02-at-10.47.32-AM-1024x557.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-02-at-10.47.32-AM.png) [![Screen Shot 2016-05-02 at 10.47.49 AM](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-02-at-10.47.49-AM-300x288.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-02-at-10.47.49-AM.png) En nuestro Caso vamos a crear una Aplicación en tiempo real que permita visualizar cual es tu estado de ánimo, así que la llamaré EstadoDeAnimo.

[![Screen Shot 2016-05-02 at 10.50.04 AM](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-02-at-10.50.04-AM-1024x769.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-02-at-10.50.04-AM.png)

Ahora vamos a activar Firebase en nuestra aplicación, para ello vamos a File>Project Structure>Cloud y activamos la casilla de Firebase y hacemos click en Ok, (Si así de fácil) al haber hecho esto ya queda activado Firebase en nuestro proyecto.

[![Screen Shot 2016-05-02 at 10.52.33 AM](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-02-at-10.52.33-AM-281x300.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-02-at-10.52.33-AM.png)

[![Screen Shot 2016-05-02 at 10.53.00 AM](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-02-at-10.53.00-AM-300x249.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-02-at-10.53.00-AM.png)

Para comprobar que efectivamente todo ha salido bien hasta aquí vamos chequear un poco nuestro archivo build.gradle (Vamos a chequear el que corresponde a App).

[![Screen Shot 2016-05-02 at 10.56.31 AM](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-02-at-10.56.31-AM-300x154.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-02-at-10.56.31-AM.png)

Notarás que se han añadido algunas dependencias de Firebase. Sin embargo falta algo mas que debemos añadir antes de las dependencias algunas líneas relacionadas con la licencia de Firebase para evitar errores de compilación y licencias duplicadas, (Esperamos que para las próximas versiones no sea necesario). Las líneas que añadiremos serán estas:

{% tabbed_codeblock  %}
    <!-- tab java -->
        packagingOptions {
            exclude 'META-INF/LICENSE'
            exclude 'META-INF/LICENSE-FIREBASE.txt'
            exclude 'META-INF/NOTICE'
        }
    <!-- endtab -->
{% endtabbed_codeblock %}

Dejando a nuestro archivo build.gradle de esta manera:

[![Screen Shot 2016-05-02 at 11.03.56 AM](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-02-at-11.03.56-AM-300x213.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-02-at-11.03.56-AM.png)

Así finaliza la instalación de Firebase en nuestra aplicación, ahora vamos a crear nuestra aplicación: nuestra aplicación consistirá en un TextView donde aparecerá nuestro estado de animo y tres botones para cambiarlo deberá verse así:

[![Screen Shot 2016-05-02 at 11.10.52 AM](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-02-at-11.10.52-AM-161x300.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-02-at-11.10.52-AM.png)

Para a hacer esto vamos a App>res>layout>activity_main.xml y mediante nuestro editor gráfico vamos a poner todo como nuestra vista anterior, los botones los nombraremos de manera que sea mas fácil acordarnos: (buttonAlegre, buttonTriste, buttonEnojado), mientras que el texView lo dejaremos simplemente “textView”:

[![Screen Shot 2016-05-02 at 11.11.12 AM](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-02-at-11.11.12-AM-300x159.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-02-at-11.11.12-AM.png) [![Screen Shot 2016-05-02 at 11.11.22 AM](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-02-at-11.11.22-AM-300x194.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-02-at-11.11.22-AM.png) [![Screen Shot 2016-05-02 at 11.11.36 AM](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-02-at-11.11.36-AM-300x152.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-02-at-11.11.36-AM.png) [![Screen Shot 2016-05-02 at 11.18.58 AM](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-02-at-11.18.58-AM-300x290.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-02-at-11.18.58-AM.png)

La primera cosa que debemos hacer cuando vamos a trabajar con Firebase en nuestra aplicación es configurar el "Android Context”, esto se debe a que Firebase debe ser llamado en el primer ciclo de vida de nuestra aplicación. Para hacer esto vamos a crear una nueva clase adentro de java>com.nuestropaquete.estadodeanimo. Para hacer esto hacemos click derecho, new>Java Class, y en mi caso le colocaré el nombre de EstadoDeAnimo.

[![Screen Shot 2016-05-02 at 11.19.14 AM](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-02-at-11.19.14-AM-300x114.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-02-at-11.19.14-AM.png)

En esta clase partiremos de esto:

{% tabbed_codeblock  %}
    <!-- tab java -->
        /*
         * Created by seagomezar on 5/2/16.
         */

        public class EstadoDeAnimo {
        }

        a esto:

        package com.example.seagomezar.estadodeanimo;

        import com.firebase.client.Firebase;

        /*
         * Created by seagomezar on 5/2/16.
         */
        public class EstadoDeAnimo extends android.app.Application{
            @Override
            public void onCreate() {
                super.onCreate();
                Firebase.setAndroidContext(this);
            }
        }
    <!-- endtab -->
{% endtabbed_codeblock %}

package com.example.seagomezar.estadodeanimo;

Ahora vamos incluir esta clase en nuestro AndroidManifest.xml[![Screen Shot 2016-05-02 at 11.28.00 AM](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-02-at-11.28.00-AM-300x139.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-02-at-11.28.00-AM.png) Al hacer esto estamos configurando Firebase desde el comienzo del ciclo de vida de nuestra aplicación, ahora vamos a escribir la funcionalidad, para esto abrimos nuestro archivo MainActivity.java  y escribamos lo siguiente:

{% tabbed_codeblock  %}
    <!-- tab java -->
        package com.example.seagomezar.estadodeanimo;
        import android.support.v7.app.AppCompatActivity;
        import android.os.Bundle;
        import android.view.View;
        import android.widget.Button;
        import android.widget.TextView;
        import com.firebase.client.DataSnapshot;
        import com.firebase.client.Firebase;
        import com.firebase.client.FirebaseError;
        import com.firebase.client.ValueEventListener;
        import com.firebase.client.realtime.util.StringListReader;

        public class MainActivity extends AppCompatActivity {

            TextView mTextView;
            Button mButtonAlegre, mButtonTriste, mButtonEnojado;
            Firebase mRef;

            @Override
            protected void onCreate(Bundle savedInstanceState) {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.activity_main);
            }

            @Override
            protected void onStart() {
                super.onStart();
                mTextView = (TextView)findViewById(R.id.textView);
                mButtonAlegre = (Button)findViewById(R.id.buttonAlegre);
                mButtonEnojado = (Button)findViewById(R.id.buttonEnojado);
                mButtonTriste = (Button)findViewById(R.id.buttonTriste);
                mRef = new Firebase("https://miendpointenfirebase.firebaseio.com/estado");

                mRef.addValueEventListener(new ValueEventListener() {
                    @Override
                    public void onDataChange(DataSnapshot dataSnapshot) {
                        String estado = dataSnapshot.getValue(String.class);
                        mTextView.setText(estado);
                    }

                    @Override
                    public void onCancelled(FirebaseError firebaseError) {

                    }
                });

                mButtonAlegre.setOnClickListener(new View.OnClickListener() {
                    @Override
                    public void onClick(View v) {
                        mRef.setValue("Estoy Alegre! =)");
                    }
                });

                mButtonTriste.setOnClickListener(new View.OnClickListener() {
                    @Override
                    public void onClick(View v) {
                        mRef.setValue("Estoy Triste! =(");
                    }
                });

                mButtonEnojado.setOnClickListener(new View.OnClickListener() {
                    @Override
                    public void onClick(View v) {
                        mRef.setValue("Estoy Enojado! -.-");
                    }
                });
            }
        }
    <!-- endtab -->
{% endtabbed_codeblock %}

Miremos en detalle que es lo que hacen las líneas mas importantes de nuestro código.

En las lineas 15 a 17 declaramos las variables de nuestra App, una para cada elemento en la interfaz y una variable para manejar nuestro Firebase.

En las líneas 27 a 31 enlazamos las variables con los elementos de la interfaz y en la línea 32 enlazamos nuestra variable local de Firebase con nuestro endpoint.

  Entre las líneas 34 a 36 definimos un Listener que nos indica cuando cambia el valor en nuestro Endpoint, y en caso de que cambie vamos a obtener (línea 37) el valor que cambió y se lo vamos a asignar a una variable llamada estado, para luego en la línea 38 pasarla a nuestro TextView.

Entre las líneas 47 a 66 simplemente creamos un Listener para cuando nuestro usuario pulse el botón cambiemos el valor del estado en nuestro EndPoint.

Nota que el valor que se cambie o actualice será el mismo para todos los usuarios que tengan la aplicación.

Estamos listos!!, Ahora dale play y déjate sorprender por lo bien que funciona, aunque este ejemplo es básico, te servirá de punto de partida para todas las aplicaciones en tiempo real que quieras desarrollar con Firebase y Android!!

Aquí te dejo algunas imágenes de Nuestra Aplicación y el EndPoint.

[![Screen Shot 2016-05-02 at 11.52.16 AM](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-02-at-11.52.16-AM-300x88.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-02-at-11.52.16-AM.png) 
[![Screen Shot 2016-05-02 at 11.53.08 AM](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-02-at-11.53.08-AM-300x77.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screen-Shot-2016-05-02-at-11.53.08-AM.png) 
[![Screenshot_20160502-115246](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screenshot_20160502-115246-180x300.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screenshot_20160502-115246.png) 
[![Screenshot_20160502-115257](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screenshot_20160502-115257-180x300.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2016/05/Screenshot_20160502-115257.png)

Eso es todo, espero que este post te sea de utilidad, Si tienes alguna duda no dudes en dejarme un comentario en la parte de abajo, recuerda que si te gustó también puedes compartir usando los links a las redes sociales en la parte de abajo.