---
title: Como crear espectaculares presentaciones usando HTML y CSS
tags:
  - CSS3
  - Desarrollo frontend
  - Desarrollo Web
  - HTML
  - HTML5
  - Javascript
  - Reveal.js
url: 89.html
id: 89
categories:
  - Desarrollo Web
date: 2015-07-25 05:06:17
---

Muchas veces en cursos o conferencias que presenciamos nos hemos encontrado con una novedosa manera de ver  el contenido o la información que nos es presentada. Esta forma es mediante el navegador web que ofrece grandes ventajas respecto a los métodos tradicionales de creación de diapositivas como Powerpoint o Prezi. <!-- more --> Crear este tipo de presentaciones es relativamente fácil y esto crea un gran impacto en nuestra audiencia. No necesitamos grandes conocimientos técnicos ni nada por el estilo, simplemente necesitamos la decisión de hacer las cosas diferentes y de aprender algo nuevo. Así que manos a la obra, en este post aprenderemos como crear presentaciones usando HTML, CSS bajo un framework hecho en Javascript para facilitar nuestro trabajo en la creación de presentaciones: [Reveal.js](http://lab.hakim.se/reveal-js/). Palabras mas palabras menos reveal.js se resume en la siguiente presentación hecha por el team mismo de Reveal.js solamente que se encuentra en inglés: Bien, lo primero que necesitamos hacer entonces es descargar reveal.js, esto es bastante sencillo puesto que solo debes ir al siguiente enlace (es una pagina de github) [reveal.js](https://github.com/hakimel/reveal.js) y descargar como zip (veras esta opción en la parte derecha). Una vez descargado este archivo lo vamos a descomprimir y vamos a crear una nueva carpeta donde crearemos nuestras presentaciones. Esta carpeta sugiero que quede con la siguiente estructura (por cuestiones de manejabilidad y escalabilidad): 1. Antes que nada copiaremos de la carpeta de reveal.js a nuestra carpeta de trabajo las siguientes carpetas: css , js , lib , plugin 2. Luego crearemos una carpeta llamada images (o imágenes) donde guardaremos las imágenes que usemos en nuestra presentación. 3. Finalmente crearemos un archivo .html con el nombre de nuestra presentación. En mi caso se llamará tutorial.html. Hecho esto deberíamos tener entonces una carpeta con la siguiente estructura: [![folder-structure-reveal](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/07/folder-structure-reveal-300x149.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/07/folder-structure-reveal.png) Bien, ahora que tenemos listo nuestro folder y nuestro archivo vamos a crear nuestra presentación. Abriremos entonces nuestro archivo tutorial.html (O cualquiera que sea el nombre que le hayas puesto a tu archivo) con un editor de texto (en mi caso uso [atom](https://atom.io/) pero notepad++ o incluso bloc de notas funciona bien) y vamos a copiar las siguientes líneas:

{% tabbed_codeblock  %}
  <!-- tab html -->
    <!Doctype html>
		<html lang="es">
			<head>
				<meta charset="utf-8">
				<!--Aca tu puedes poner el titulo de la presentación-->
				<title>El tutorial de reveal</title>
				<meta name="description" content="Una descripción para que algunos buscadores indexen o cambien la descripción de la presentación según lo que tu quieras">
				<meta name="author" content="Sebastian Gomez">
				<meta name="apple-mobile-web-app-capable" content="yes" />
				<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" />
				<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, minimal-ui">
				<link rel="stylesheet" href="css/reveal.css">
				<link rel="stylesheet" href="css/theme/black.css" id="theme">
				<!\-\- Code syntax highlighting -->
				<link rel="stylesheet" href="lib/css/zenburn.css">
				<!\-\- Printing and PDF exports -->
				<script>
					var link = document.createElement( 'link' );
					link.rel = 'stylesheet';
					link.type = 'text/css';
					link.href = window.location.search.match( /print-pdf/gi ) ? 'css/print/pdf.css' : 'css/print/paper.css';
					document.getElementsByTagName( 'head' )\[0\].appendChild( link );
				</script>
				<!--\[if lt IE 9\]>
				<script src="lib/js/html5shiv.js"></script>
				<!\[endif\]-->
			</head>
			<body>
				<div class="reveal">
					<!\-\- Ninguno de los elementos anteriores es necesario que lo modifiques si no es necesario -->
					<!\-\- Cada etiqueta section que se encuentre adentro del <div class="slides"> representará a una diapositiva  de la presentación -->
					<div class="slides">
						<section>
							<h1>Hola</h1><!--Adentro puede ir cualquier codigo html que será mostrado como diapositiva-->
						</section>
					</div>
				</div>
				<!--El siguiente codigo son los archivos requeridos por reveal.js para funcioanr correctamente, ademas provee una serie de configuraciones con las cuales se puede jugar un poco más adelante-->
				<script src="lib/js/head.min.js"></script>
				<script src="js/reveal.js"></script>
				<script>
					// Full list of configuration options available at:
					// https://github.com/hakimel/reveal.js#configuration
					Reveal.initialize({
						controls: true,
						progress: true,
						history: true,
						center: true,
						transition: 'slide', // none/fade/slide/convex/concave/zoom
						// Optional reveal.js plugins
						dependencies: \[
							{ src: 'lib/js/classList.js', condition: function() { return !document.body.classList; } },
							{ src: 'plugin/markdown/marked.js', condition: function() { return !!document.querySelector( '\[data-markdown\]' ); } },
							{ src: 'plugin/markdown/markdown.js', condition: function() { return !!document.querySelector( '\[data-markdown\]' ); } },
							{ src: 'plugin/highlight/highlight.js', async: true, condition: function() { return !!document.querySelector( 'pre code' ); }, callback: function() { hljs.initHighlightingOnLoad(); } },
							{ src: 'plugin/zoom-js/zoom.js', async: true },
							{ src: 'plugin/notes/notes.js', async: true }
						\]
					});
				</script>
			</body>
		</html>
  <!-- endtab -->
{% endtabbed_codeblock %}

Si abres el archivo tutorial.html en tu navegador deberias ver algo similar a esto: Como podemos observar el encabezado de nuestro archivo no va a cambiar mucho. Solamente cambiara el titulo de presentación que se encuentra adentro de las etiquetas <title>, el footer del archivo (desde la linea ### hasta el final) tampoco va a cambiar demasiado salvo que seamos ya expertos y queramos hacer alguna modificación custom. La parte importante se encuentra dentro de las etiquetas section, cada etiqueta section representa una diapositiva y todo lo que pongamos adentro va a mostrarse en una diapositiva, así que creemos unas cuantas diapositivas mas usando las etiquetas básicas de html.

{% tabbed_codeblock  %}
  <!-- tab html -->
    <!Doctype html>
		<html lang="es">
			<head>
				<meta charset="utf-8">
				<!--Aca tu puedes poner el titulo de la presentación-->
				<title>El tutorial de reveal</title>
				<meta name="description" content="Una descripción para que algunos buscadores indexen o cambien la descripción de la presentación según lo que tu quieras">
				<meta name="author" content="Sebastian Gomez">
				<meta name="apple-mobile-web-app-capable" content="yes" />
				<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" />
				<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, minimal-ui">
				<link rel="stylesheet" href="css/reveal.css">
				<link rel="stylesheet" href="css/theme/black.css" id="theme"><!--Si queremos cambiar el tema simplemente debemos de cambiar aca el archivo .css usado-->
				<!\-\- Code syntax highlighting -->
				<link rel="stylesheet" href="lib/css/zenburn.css">
				<!\-\- Printing and PDF exports -->
				<script>
					var link = document.createElement( 'link' );
					link.rel = 'stylesheet';
					link.type = 'text/css';
					link.href = window.location.search.match( /print-pdf/gi ) ? 'css/print/pdf.css' : 'css/print/paper.css';
					document.getElementsByTagName( 'head' )\[0\].appendChild( link );
				</script>
				<!--\[if lt IE 9\]>
				<script src="lib/js/html5shiv.js"></script>
				<!\[endif\]-->
				<link rel="stylesheet" href="css/mi-estilo.css"><!--aca puedo tener mis propios estilos css-->
			</head>
			<body>
				<div class="reveal">
					<!\-\- Ninguno de los elementos anteriores es necesario que lo modifiques si no es necesario -->
					<!\-\- Cada etiqueta section que se encuentre adentro del <div class="slides"> representará a una diapositiva  de la presentación -->
					<div class="slides">
						<section>
							<h1>Hola, esta sería la diapositiva 1</h1><!--Adentro puede ir cualquier codigo html que será mostrado como diapositiva-->
							<h2>Usualmente en esta diapositiva colocamos el tema y la información</h2>
							<h3>Sebastián Gomez</h3>
						</section>
						<section>
							<h2>Podemos añadir imágenes de manera tan simple como:</h2>
							<img src="images/mi-imagen.jpg" alt="mi-imagen"/>
						</section>
						<section>
							<h2>Esta sería la diapositiva 3</h2>
							<p> Esta diapositiva tendría un párrafo con letra normal</p>
							<p class="letra-roja"> Este es un párrafo con letra roja</p>
						</section>
					</div>
				</div>
				<!--El siguiente codigo son los archivos requeridos por reveal.js para funcioanr correctamente, ademas provee una serie de configuraciones con las cuales se puede jugar un poco más adelante-->
				<script src="lib/js/head.min.js"></script>
				<script src="js/reveal.js"></script>
				<script>
					// Full list of configuration options available at:
					// https://github.com/hakimel/reveal.js#configuration
					Reveal.initialize({
						controls: true,
						progress: true,
						history: true,
						center: true,
						transition: 'slide', // none/fade/slide/convex/concave/zoom
						// Optional reveal.js plugins
						dependencies: \[
							{ src: 'lib/js/classList.js', condition: function() { return !document.body.classList; } },
							{ src: 'plugin/markdown/marked.js', condition: function() { return !!document.querySelector( '\[data-markdown\]' ); } },
							{ src: 'plugin/markdown/markdown.js', condition: function() { return !!document.querySelector( '\[data-markdown\]' ); } },
							{ src: 'plugin/highlight/highlight.js', async: true, condition: function() { return !!document.querySelector( 'pre code' ); }, callback: function() { hljs.initHighlightingOnLoad(); } },
							{ src: 'plugin/zoom-js/zoom.js', async: true },
							{ src: 'plugin/notes/notes.js', async: true }
						\]
					});
				</script>
			</body>
		</html>
  <!-- endtab -->
{% endtabbed_codeblock %}

Y se vería de esta manera: también se puede cambiar el background de toda la presentación o de cada diapositiva, y jugar un poco con los efectos y las transmisiones tal y como lo podemos observar a continuación.

{% tabbed_codeblock  %}
  <!-- tab html -->
		<div class="reveal">
			<!\-\- Ninguno de los elementos anteriores es necesario que lo modifiques si no es necesario -->
      <!\-\- Cada etiqueta section que se encuentre adentro del <div class="slides"> representará a una diapositiva  de la presentación -->
			<div class="slides">
				<section data-background="#4d7e65">
          <h1>Hola, esta sería la diapositiva 1 con otro fondo</h1><!--Adentro puede ir cualquier codigo html que será mostrado como diapositiva-->
				  <h2>Usualmente en esta diapositiva colocamos el tema y la información</h2>
          <h3>Sebastián Gomez</h3>
          <p>Adicionalmente la siguiente diapositiva hará una transición tipo zoom<p>
        </section>
        <section data-background="#000" data-background-transition="zoom">
          <h2>Podemos añadir imágenes de manera tan simple como:</h2>
          <img src="images/mi-imagen.jpg" alt="mi-imagen"/>
				</section>
        <section>
          <h2>Esta sería la diapositiva 3</h2>
          <p> Esta diapositiva tendría un párrafo con letra normal</p>
          <p class="letra-roja"> Este es un párrafo con letra roja</p>
				</section>
			</div>
		</div>
	<!-- endtab -->
{% endtabbed_codeblock %}

Y se vería de ésta manera: Es posible también anidar sections o diapositivas que permitirá crear subsecciones de la presentación bajando o subiendo de nivel

{% tabbed_codeblock  %}
  <!-- tab html -->
<div class="reveal">
			<!\-\- Ninguno de los elementos anteriores es necesario que lo modifiques si no es necesario -->
      <!\-\- Cada etiqueta section que se encuentre adentro del <div class="slides"> representará a una diapositiva  de la presentación -->
			<div class="slides">
				<section data-background="#4d7e65">
          <h1>Hola, esta sería la diapositiva 1 con otro fondo</h1><!--Adentro puede ir cualquier codigo html que será mostrado como diapositiva-->
				  <h2>Usualmente en esta diapositiva colocamos el tema y la información</h2>
          <h3>Sebastián Gomez</h3>
          <p>Adicionalmente la siguiente diapositiva hará una transición tipo zoom<p>
        </section>
        <section data-background="#000" data-background-transition="zoom">
          <h2>Podemos añadir imágenes de manera tan simple como:</h2>
          <img src="images/mi-imagen.jpg" alt="mi-imagen"/>
				</section>
        <section>
          <section>
          <h2>Esta sería la diapositiva 3</h2>
          <p> Esta diapositiva tendría un párrafo con letra normal</p>
          <p class="letra-roja"> Este es un párrafo con letra roja</p>
          </section>
            <section>
              <h2>esta sería la diapositiva 3.1 ya que es una subsección que inicia con la diapositiva 2, por esto está anidada</h2>
              <p> Esta diapositiva tendría un párrafo con letra normal</p>
              <p class="letra-roja"> Este es un párrafo con letra roja</p>
    				</section>
            <section>
              <h2>esta sería la diapositiva 3.2</h2>
              <p> Esta diapositiva tendría un párrafo con letra normal</p>
              <p class="letra-roja"> Este es un párrafo con letra roja</p>
    				</section>
				</section>
        <section>
          <h2> 123 3245</h2>
          <p> Y esta sería finalmente la diapositiva 4, aquí quedan los enlaces a los distintos temas que se pueden usar para nuestras presentacones y que vienen por defecto:<br/>

  						<!\-\- Hacks to swap themes after the page has loaded. Not flexible and only intended for the reveal.js demo deck. -->
  						<a href="#" onclick="document.getElementById('theme').setAttribute('href','css/theme/black.css'); return false;">Black (default)</a> -
  						<a href="#" onclick="document.getElementById('theme').setAttribute('href','css/theme/white.css'); return false;">White</a> -
  						<a href="#" onclick="document.getElementById('theme').setAttribute('href','css/theme/league.css'); return false;">League</a> -
  						<a href="#" onclick="document.getElementById('theme').setAttribute('href','css/theme/sky.css'); return false;">Sky</a> -
  						<a href="#" onclick="document.getElementById('theme').setAttribute('href','css/theme/beige.css'); return false;">Beige</a> -
  						<a href="#" onclick="document.getElementById('theme').setAttribute('href','css/theme/simple.css'); return false;">Simple</a> <br>
  						<a href="#" onclick="document.getElementById('theme').setAttribute('href','css/theme/serif.css'); return false;">Serif</a> -
  						<a href="#" onclick="document.getElementById('theme').setAttribute('href','css/theme/blood.css'); return false;">Blood</a> -
  						<a href="#" onclick="document.getElementById('theme').setAttribute('href','css/theme/night.css'); return false;">Night</a> -
  						<a href="#" onclick="document.getElementById('theme').setAttribute('href','css/theme/moon.css'); return false;">Moon</a> -
  						<a href="#" onclick="document.getElementById('theme').setAttribute('href','css/theme/solarized.css'); return false;">Solarized</a>
          </p>

				</section>
			</div>
		</div>
	<!-- endtab -->
{% endtabbed_codeblock %}

Y se vería de ésta manera: Es posible también customizar un poco los estilos de nuestra presentación creando modificando el archivo mi-estilo.css de nuestra carpeta css (como he mostrado co la clase letra-roja. Las imágenes que usemos las podemos guardar en nuestra carpeta images y las podemos mostrar en nuestra presentación sin ningún problema simplemente las linkeamos en el src de la etiqueta img, y así podremos crear y tener control de nuestras presentaciones. Eso es todo ahora los invito a crear espectaculares presentaciones con esta simple pero poderosa manera de crear contenido y presentaciones.