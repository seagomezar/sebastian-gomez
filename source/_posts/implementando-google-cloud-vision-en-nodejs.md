---
title: Implementando Google Cloud Vision en NodeJS
tags:
  - CSS3
  - HTML5
  - Javascript
  - NodeJS
  - Inteligencia Artificial
  - Desarrollo web
url: 503.html
thumbnailImage: https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2017/12/cloud-vision.jpg
thumbnailImagePosition: left
id: 503
categories:
  - Javascript
date: 2017-12-26 12:34:35
---

Siguiendo con la temática de la Cloud Visión de Google en este post vamos a implementarla en NodeJS para detectar características sobre imágenes. 
<!-- excerpt -->
Lo primero que necesitamos entender es que la SDK de la Vision Cloud de Google debería operar sobre un servidor, es allí donde surge la necesidad de usar NodeJS sin embargo si tienes alguna experiencia con NodeJS sabrás que en la mayoría de los casos se requiere de una librería para procesar las peticiones HTTP Post, Get y Put. En este post usaremos ExpressJS ya que es la librería mas conocida para esta tarea. Si tienes dudas sobre las generalidades de la Cloud Vision de Google o la capa gratuita en este [post ](https://www.sebastian-gomez.com/desarrollo-web/introduccion-a-google-vision-api/)puedes revisarlas.

En primer lugar vamos a revisar las característica que nos ofrece la API de visión de Google para implementar con NodeJs:

*   Detección de rostros
*   Atributos de las imágenes.
*   Anotación de etiquetas.
*   Detección de contenido para adultos.
*   Detección de logos.
*   Encuadre de los elementos de la imagen.
*   Reconocimiento óptico de caracteres.

La arquitectura propuesta para realizar la implementación de Cloud Visión de Google es esta:

[![](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2017/12/560CD915-5BD9-4DD9-A6BD-2C81C69DF003-1024x508.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2017/12/560CD915-5BD9-4DD9-A6BD-2C81C69DF003.png)

Revisemos las responsabilidades que tendría cada uno de los componentes de la arquitectura anterior:

Client Browser:

*   Presentar información.
*   Tomar fotografías o videos.
*   Preprocesar imágenes.
*   Enviar las imágenes al servidor.

ExpressJS:

*   Filtrar las peticiones del cliente.
*   Recibir las imágenes y almacenarlas en el servidor.
*   Enviar las respuestas al cliente.

Google Vision SDK *:

*   Procesar las imágenes.
*   Operaciones matemáticas.
    *   Detección de píxeles.
    *   Filtros.
    *   Transformaciones.
*   Codificación de las imágenes.
*   Preparación de las peticiones.

Google Vision API:

*   Comparar las imágenes contra el conjunto de imágenes.
*   Aplicar rutinas de machine learning con las imágenes como entrada.
*   Conectarse con otras tecnologías como Tensor Flow.
*   Analizar los resultados y aumentar la base de conocimiento.

(*) Un SDK (Software Development Kit), o kit de desarrollo de software, es un conjunto de herramientas que ayudan a la programación de aplicaciones para un entorno tecnológico particular.

Antes de empezar necesitamos obtener una clave y las credenciales del proyecto para hacer uso de la API y la SDK, esto lo podemos conseguir directamente en la consola: [https://cloud.google.com/](https://cloud.google.com/)siguiendo estos simples pasos.

*   Crear el proyecto: [https://console.cloud.google.com/projectcreate](https://console.cloud.google.com/projectcreate)
*   Acceder al proyecto.
*   Seleccionar cuentas de servicio.
*   Crear una cuenta de servicio → completar la información.
*   Seleccionamos suministrar una nueva clave privada en formato JSON.
*   Guardamos el archivo JSON que se genera.

Manos a la obra con la implementación: en primer lugar creemos un proyecto en Node desde cero usando la línea de comandos.

*   Crear un directorio: mkdir mi-directorio
*   Inicializar git: git init
*   Inicializar node: npm init
*   Instalar las dependencias:
    *   npm install @google-cloud/vision --save
    *   npm install express --save
    *   npm install multer --save
*   Configurar nuestro comando npm start en el package.json para que inicie nuestro server:

{% tabbed_codeblock  %}
  <!-- tab js -->
    ...
    "scripts": {
        "test":"echo\\"Error: no test specified\\"&& exit 1",
        "start":"node server.js"
    },
    ...
  <!-- endtab -->
{% endtabbed_codeblock %}

Esto nos creará la estructura base del proyecto y deberás añadir ademas algunos archivos. A continuación te muestro exactamente como debería lucir tu carpeta del proyecto y la explicación de cada item:

[![](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2017/12/71465872-9C25-4862-9540-4661925AA96E-1024x485.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2017/12/71465872-9C25-4862-9540-4661925AA96E.png)

Como notarás los únicos archivos sobre los cuales escribiremos código son server.js, index.html y app.js. Empecemos entonces con una funcionalidad básica: ¿Como tomar fotos con javascript?

[https://codepen.io/seagomezar/pen/QayorL](https://codepen.io/seagomezar/pen/QayorL)

Como verás simplemente creando un stream y accediendo a los elementos del DOM es posible obtener la mas sencilla funcionalidad de tomar desde la cámara de tu ordenador las imágenes y pintarlas dentro de la etiqueta img.

Ahora veremos como subir la imagen que tomamos a nuestro servidor: En primer lugar desde nuestra aplicación una vez que tomamos la imagen y la pintamos dentro de la etiqueta SRC necesitamos hacer una petición HTTP al servidor indicándole que allí va la imagen. Para esto abre tu archivo app.js y añade la función upload:

{% tabbed_codeblock  %}
  <!-- tab js -->
    function upload() {
        const http = new XMLHttpRequest();
        const url = "upload";
        snap().then((blob) => {
            http.open("POST", url, true);
            http.setRequestHeader('X-Requested-With', 'XMLHttpRequest');
            http.onreadystatechange = (data) => {
                //Call a function when the state changes.
                if (http.readyState == 4 && http.status == 200) {
                    console.log(http.response);
                }
            }
            const formData = new FormData();
            formData.append("uploads", blob);
            http.send(formData);
        });
    }
  <!-- endtab -->
{% endtabbed_codeblock %}

Como verás en esta función invocamos a la función snap() que definimos para capturar imágenes desde nuestra cámara en el browser para enviar el contenido de la imagen mediante una XMLHttpRequest.  Luego necesitamos crear nuestro servidor para que sea capaz de recibir y procesar dicha imagen, es en este punto entonces donde necesitamos crear y configurar un servidor básico con express que soporte la opción de subir la imagen. Por tanto tu archivo server.js debe lucir de ésta manera:

 
{% tabbed_codeblock  %}
  <!-- tab js -->
    const express = require('express');
    const app = express();
    /* serves main page */
    app.get("/", function (req, res) {
        res.sendfile('index.html')
    });
    /* serves all the static files */
    app.get(/^(.+)$/, function (req, res) {
        console.log('static file request : ' + req.params);
        res.sendfile(__dirname + req.params\[0\]);
    });
    var port = process.env.PORT || 5000;
    app.listen(port, function () {
        console.log("Listening on " + port);
    });
    // Setting up multer to upload images
    const multer = require('multer');
    const storage = multer.diskStorage({
        destination: (req, file, cb) => {
            cb(null, 'uploads/')
        },
        filename: (req, file, cb) => {
            cb(null, Date.now() + '.jpg')//Appending.jpg
        }
    });
    const upload = multer({ storage: storage });
    app.post("/upload", upload.single('uploads'), (req, res) => {
        const currentFile = req.file.path;
        console.log("Image path: " + req.file.path);
        res.send("Ok");
    });
  <!-- endtab -->
{% endtabbed_codeblock %}

En la primera parte de este archivo verás la funcionalidad básica que tiene un servidor de NodeJs usando express y sus respectivas configuraciones, sin profundizar mucho en esto nuestra función de interés es la función upload la cual procesará la imagen y la guardará en la carpeta uploads. Verás que  esta función también pondrá cada imagen en la carpeta de uploads con un numero consecutivo relacionado con el momento actual, esto lo hacemos para evitar duplicidades en las imágenes.

Hasta este momento solo hemos creado funciones utilitarias en el server.js para inicializar el server y para subir archivos. Pero no hemos realizado ningún proceso sobre la imagen, ahora vamos a aprender como detectar características sobre la imagen usando la SDK de Cloud Vision API:

{% tabbed_codeblock  %}
  <!-- tab js -->
    const vision = require('@google-cloud/vision')({
        projectId: 'vision-poc-180601',
        keyFilename: './cloud-credentials.json'
    });

    app.post("/labels", upload.single('uploads'), function (req, res) {
        const currentFile = req.file.path;
        const request = {
            source: {
                filename: currentFile
            }
        };
        vision.labelDetection(request)
        .then((results) => {
            const labels = results\[0\].labelAnnotations;
            console.log('Labels:');
            labels.forEach((label) => console.log(label.description));
                res.send(labels);
            })
        .catch((err) => {
            console.error('ERROR:', err);
            res.send("BAD");
        });
    });
  <!-- endtab -->
{% endtabbed_codeblock %}

Primero vamos a añadir las líneas que inicializan nuestra configuración de la Cloud Vision de Google. Luego creamos con express un endpoint que se encarga de recibir una imagen y obtener todos los labels o etiquetas que se pueden obtener de dicha imagen. Y los retornaremos como respuesta de la petición para presentarlos en el front-end.

Finalmente podemos usar otra característica que nos permite hallar los rostros en una imagen con su respectiva traza y path de cada característica del rostro:

{% tabbed_codeblock  %}
  <!-- tab js -->
    app.post("/faces", upload.single('uploads'), (req, res) => {
        const currentFile = req.file.path;
        vision.faceDetection({ source: { filename: currentFile } })
            .then((results) => {
                const faces = results\[0\].faceAnnotations;
                res.send(faces);
            })
            .catch((err) => {
                console.error('ERROR:', err);
                res.send("BAD");
            });
    });
  <!-- endtab -->
{% endtabbed_codeblock %}

Una vez que tenemos nuestro archivo de servidor server.js completo, debemos modificar un poco nuestro index.html y app.js para mostrar nuestros hallazgos:

{% tabbed_codeblock  %}
  <!-- tab html -->
    <!—index.html—>
    <button class="shutter" onclick="upload()">
        Tomar y subir una foto
    </button>
    <button class="shutter" onclick="sendToLabelDetection()">
        Analizar imagen
    </button>
    <button class="shutter" onclick="sendToFaceDetection()">
        Detectar rostros y características
    </button>

    <div>
        <h2>Características de la imagen</h2>
        <div id="labels"></div>
    </div>
  <!-- endtab -->
{% endtabbed_codeblock %}

Poniendo todo lo anterior junto podrás ver las características(labels) de la imagen en una manera similar a esta en la detección de características:

[![](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2017/12/589231F9-3898-4D24-982A-9C14165372AF.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2017/12/589231F9-3898-4D24-982A-9C14165372AF.png)

Sin embargo para la detección y extracción de rostros he implementado solo el login por consola donde podrás ver un elemento de un array por rostro detectado y dentro de cada rostro detectado las “landmarks” que delimitan exactamente cada uno de los elementos del rostro:

[![](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2017/12/08180391-A2C7-4B81-9598-EC743DF0E449-1024x424.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2017/12/08180391-A2C7-4B81-9598-EC743DF0E449.png) [![](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2017/12/F4C92A6E-35F4-4F07-90A5-3EE820D2160A-1024x291.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2017/12/F4C92A6E-35F4-4F07-90A5-3EE820D2160A.png)

Puedes jugar con estas funciones para crear combinaciones de características únicas para tu aplicación o producto. En este repositorio podrás jugar con este código  y usarlo para la implementación de tus propias funcionalidades, también encontrarás algunas otras funciones como por ejemplo detectar personas felices, o tristes e una foto:

[https://github.com/seagomezar/devfest-vision](https://github.com/seagomezar/devfest-vision)

Para consultar más información sobre las operaciones de la SDK y la API de cloud vision para NodeJS puedes ver el siguiente repositorio:

[https://github.com/googleapis/nodejs-vision/tree/master/samples](https://github.com/googleapis/nodejs-vision/tree/master/samples)


Eso es todo, espero que este post te sea de utilidad y lo puedas aplicar a algún proyecto que tengas en mente, déjame un comentario si lograste implementarlo, si quieres añadir alguna otra funcionalidad o si tienes alguna duda no dudes en dejarme un comentario en la parte de abajo, recuerda que si te gustó también puedes compartir usando los links a las redes sociales en la parte de abajo.