---
title: Como consumir APIs REST usando AngularJS
tags:
  - AngularJS
  - API
  - Desarrollo frontend
  - Desarrollo Web
  - REST
url: 77.html
thumbnailImage: https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/11/a1.png
thumbnailImagePosition: left
id: 77
categories:
  - Angular
date: 2015-06-01 18:05:45
---

En este pequeño post quiero mostrar un ejemplo sencillo de como usar angularJS para realizar consultas o peticiones a APIs rest propias o de terceros, <!-- more --> ya que hoy en día nos encontramos en pleno auge de angularJS y de las arquitecturas orientadas a servicios (Usando servicios Rest, Sobra decir que SOAP esta out). Lo primero que quiero compartir es la API que consumiremos, esta es [JSONPlaceholder](http://jsonplaceholder.typicode.com/), allí verán una pequeña introducción de como usarla y los recursos disponible para ser accesados vía API. El ejemplo que haremos sera entonces consumir los servicios de posts, traeremos todos los posts disponibles, y también traeremos un post por id. Los pasos que seguiremos serán entonces: 1. Creación de una vista que tenga dos botones. El primer botón traerá todos los post ofrecidos por la API, y el segundo botón traerá un post especifico de acuerdo al id que ingresemos. 2. Crearemos un controlador que nos permita recibir la acción de cada uno de los botones y que sea este el encargado de llamar al servicio y recibir los datos de este para ser mostrados en la vista. 3. Crearemos el mecanismo de consumo de la API que sera una factory de angular, que nos permitirá declarar los métodos y las rutas a ser consumidas en la API. 4. Finalmente Haremos una pequeña prueba para verificar el correcto funcionamiento de esta. Eso es todo manos a la obra!!! Lo primero que haremos sera crear una estructura del proyecto como la siguiente: [![estructura del proyecto](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/06/estructura-del-proyecto.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/06/estructura-del-proyecto.png) Luego dentro del archivo index.html copiaremos el siguiente código html:

{% tabbed_codeblock  %}
  <!-- tab html -->
  	<!DOCTYPE html>
    <html ng-app='myApp'>
		<head>
			<title></title>
			<script type="text/javascript" src="//ajax.googleapis.com/ajax/libs/angularjs/1.3.15/angular.min.js"></script>
			<!--Controllers-->
			<script type="text/javascript" src="controllers/testController.js"></script>
			<!--Services-->
			<script type="text/javascript" src="services/testService.js"></script>
		</head>
		<body ng-app='myApp' ng-controller="testController">
			<input type="submit" value="Todos los posts" ng-click="getAllPosts()"/>
			<input type="submit" value="Un post" ng-click="getPost()"/>
			<input type="text" ng-model="post_id" placeholder="inserte el id del post"/>
			<div ng-if="posts.exist==1">
				<h1>Lista de Posts</h1>
				<div ng-repeat="p in posts">
					<pre>
						<h5> Post # { {p.id}}</h5>
						<h2>{ {p.title}}</h2>
						<p>{ {p.body}}</p>
					</pre>
					<hr/>
				</div>
			</div>
			<div ng-if="unPost.exist==1">
				<h1>Un solo post</h1>
				<pre>
					<h5> Post # { {unPost.id}}</h5>
					<h2>{ {unPost.title}}</h2>
					<p>{ {unPost.body}}</p>
				</pre>
				<hr/>
			</div>
		</body>
	</html>
  <!-- endtab -->
{% endtabbed_codeblock %}

Arriba en la etiqueta html tenemos el nombre de nuestra app, en este caso el nombre es myApp (esto puede ser cambiado a gusto). Como podemos ver dentro de las etiquetas <head> tenemos la libreria de angular incluida, para ello esta puede ser descargada via bower, o simplemente puedes linkearla directamente desde el cdn de google, accesible desde [angularjs.org](https://angularjs.org/) Luego tenemos nuestro controlador y nuestro servicio enlazado a nuestra vista. Posteriormente tenemos asociado el controlador para todo nuestro body, en nuestro caso es el testController. Dentro del body tenemos entonces dos botones y div resultado, cada botón tiene asociado un ng-click que llama directamente a cada función del controller, y el div que simplemente tiene asociado un modelo, donde seran seteados los datos. El flujo sera entonces cuando hagamos click en cualquiera de los dos botones se llamara la funcion del controller y este a su vez llamara al servicio para que consuma el método de la API destinado para esto. Pasemos al controlador

{% tabbed_codeblock  %}
  <!-- tab js -->
    angular.module('myApp', ['testService']);
	angular.module('myApp').controller('testController', ['$scope','testRequest',testController]);
	function testController($scope, testRequest) {
		$scope.posts={};
		$scope.getAllPosts = function(){
			testRequest.posts().success(function (data){
				$scope.posts=data; // Asignaremos los datos de todos los posts
				$scope.posts.exist=1;
			});
		}
		$scope.getPost = function(){
			$scope.unPost={};
			testRequest.post($scope.post_id).success(function (data){
				$scope.unPost=data; // Asignaremos los datos del post
				$scope.unPost.exist=1;
				$scope.posts.exist=0;
			});
		}
	}
  <!-- endtab -->
{% endtabbed_codeblock %}

El código del controlador es relativamente sencillo, simplemente declaramos nuestro modulo y nuestro controller. Dentro tendremos dos funciones una para obtener todos los posts y otra para obtener un post con un Id especifico, luego estas funciones llaman el servicio y reciben los datos de este y es asignado a una variable dentro del scope, esto con el fin de que le controller en caso de que existan pueda devolverlos a la vista para ser mostrados. Pasemos al servicio

{% tabbed_codeblock  %}
  <!-- tab js -->
    angular.module('testService', [])//Declaramos el modulo
	.factory('testRequest', function($http) { //declaramos la factory
		var path = "http://jsonplaceholder.typicode.com/";//API path
		return {
			//Login
			posts : function(){ //Retornara la lista de posts
				global = $http.get(path+'posts');
				return global;
			},
			post : function(id){ //retornara el post por el id
				global = $http.get(path+'posts/'+id);
				return global;	
			}	
		}
	});
  <!-- endtab -->
{% endtabbed_codeblock %}

Al comienzo se declara el modulo y se crea una variable llamada PATH con el fin de que este servicio sea portable en caso de que cambie la dirección de la API. Inicialmente el servicio se declara como una factory donde tendremos dos funciones, una para cada operación. Dicha factory utiliza el metodo http de angular para que pueda crear la petición REST a la API Posteriormente están los dos métodos que hacen la petición GET a la API y retornan la respuesta al controller quien a su vez asigna las variables al SCOPE para ser usados en la vista. Hecho todo lo anterior estamos listos para probar nuestra pequeña app:

**Vista Inicial**

[![1](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/06/1-300x36.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/06/1.png)

**Lista de todos los post:**

[![2](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/06/2-300x240.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/06/2.png)

**Vista de un solo post: **

![3](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/06/3-300x249.png) Listo como ven es muy sencillo el consumo de APIs con angularJS, quizá hayan formas mejores de hacerlo, sin embargo esta es una forma completamente funcional y validada. Espero les sea de utilidad.