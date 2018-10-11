---
title: Conectar y usar Mandrill con Laravel 5
tags:
  - API
  - Backend
  - Desarrollo Web
  - Laravel
  - Laravel5
  - Mandrill
  - PHP
url: 103.html
id: 103
categories:
  - Laravel
date: 2015-06-17 20:09:12
---

Una de las grandes necesidades que nos encontramos al realizar cualquier tipo de aplicación es la de enviar emails desde nuestra aplicación. Esto es un problema bastante común que nos plantea distintos enfoques. Uno de ellos es si tercerizar el sistema de envío de e-mails, o si desarrollamos nuestro propio sistema de envío y seguimiento de emails. En este sencillo post muestro como enviar emails usando la API de Mandrill desde Laravel (concretamente estoy usando Laravel 5.1 para algunos de mis proyectos). Antes de empezar necesitamos entender que es Mandrill:
<!-- more -->
Se trata de un servicio de distribución de correo transaccional que permite el envío de hasta 12.000 correos electrónicos por mes de manera gratuita. Si se necesita más, hay varias opciones a precios asequibles. Mandrill brinda soporte para registros SPF y DKIM que garantizan que su correo electrónico no será considerado como spam por la mayor parte de los servicios de e-mail. Además, permite el seguimiento de estados de correo electrónico, como enviar, rebotados, recibidas, hecho clic, marcados como spam, etc. También es compatible con las plantillas y etiquetas especiales para las pruebas A / B, lo cual siempre es una ventaja. (Tomado de:[ http://altoros.com.ar/blog/mandrill-servidor-smtp-gratuito-aplicaciones/](http://altoros.com.ar/blog/mandrill-servidor-smtp-gratuito-aplicaciones/))

Adicionalmente ofrece soporte para ser usado via API por diferentes plataformas y lenguajes de programación, entre ellos PHP, RUBY, PYTHON, NODEJS  y JAVASCRIPT. Empecemos, el ejemplo que haremos consistirá en enviar un email via Mandrill APP desde Laravel 5. Para ello necesitamos tener una cuenta en [mandrillAPP](https://mandrillapp.com/). Una vez allí iremos a la pestaña Settings y se nos mostrará un botón en la parte inferior desde donde podremos crear nuestra API key. (Para esto ya deberías tener configurado tu cuenta de correo y servidor SMTP). [![apiKeyMadrill](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/06/apiKeyMadrill.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/06/apiKeyMadrill.png) Luego de esto iremos hasta la pestaña Outbounds y la tab "templates". Allí crearemos nuestro template (haciendo click en el botón, "+create new template") en este caso haremos el simple "Hola mundo". [![mandrillHolaMundo](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/06/mandrillHolaMundo-300x123.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/06/mandrillHolaMundo.png) Al finalizar haremos click en publish. eso es todo lo que necesitamos del lado de Mandrill ahora pasemonos a nuestro proyecto de laravel. Voy a suponer que ya tienes un proyecto de Laravel al menos con un controlador y una ruta que puedas usar en mi caso tengo un controller llamado UsersController el cual tiene un solo método llamado welcomeEmail y cuya Ruta es http://localhost/my-proyecto/test_email por tanto al acceder a esa ruta estaría llegando inmediatamente a este método. Ahora bien lo primero que necesitamos para poder acceder a la API de Mandrill desde laravel son la librería de Mandrill y una librería llamada Weblee que nos facilitará el acceso, dicho esto nuestro composer.json debería quedar de la siguiente manera (puede variar segun tu propio proyecto):

{% tabbed_codeblock  %}
  <!-- tab js -->
	{
		"name": "laravel/laravel",
		"description": "The Laravel Framework.",
		"keywords": \["framework", "laravel"\],
		"license": "MIT",
		"type": "project",
		"require": {
			"php": ">=5.5.9",
			"laravel/framework": "5.1.*",
			"illuminate/html": "~5.0",
			"intervention/image":"2.*",
			"barryvdh/laravel-cors": "0.5.x@dev",
			"orangehill/iseed": "dev-master",
			"weblee/mandrill": "dev-master",
			"mandrill/mandrill": "1.0.*"
		},
		"require-dev": {
			"fzaninotto/faker": "~1.4",
			"mockery/mockery": "0.9.*",
			"phpunit/phpunit": "~4.0",
			"phpspec/phpspec": "~2.1"
		},
		"autoload": {
			"classmap": \[
				"database",
				"app/Http/Controllers"
			\],
			"psr-4": {
				"App\\\": "app/"
			}
		},
		"autoload-dev": {
			"classmap": \[
				"tests/TestCase.php"
			\]
		},
		"scripts": {
			"post-install-cmd": \[
				"php artisan clear-compiled",
				"php artisan optimize"
			\],
			"post-update-cmd": \[
				"php artisan clear-compiled",
				"php artisan optimize"
			\],
			"post-create-project-cmd": \[
				"php -r \\"copy('.env.example', '.env');\\"",
				"php artisan key:generate"
			\]
		},
		"config": {
			"preferred-install": "dist"
		},
		"minimum-stability": "dev",
		"prefer-stable": true
	}
	<!-- endtab -->
{% endtabbed_codeblock %}

Una vez que hagamos esto debemos correr nuestro comando composer update para que se instalen dichas librerías. Ahora necesitamos configurar nuestro archivo config/mail.php con nuestras credenciales(en caso de que no lo hayas hecho) y debemos actualizar nuestro archivo config/services.php con nuestra mandrill API key:

{% tabbed_codeblock  %}
  <!-- tab js -->
  'mandrill' => \[
        'secret' => env('MANDRILL_KEY'),
    \],
	<!-- endtab -->
{% endtabbed_codeblock %}

Adicionalmente debemos agregar lo providers de las librerias añadidas:

{% tabbed_codeblock  %}
  	<!-- tab js -->
		'providers' => \[
			......
			'Weblee\\Mandrill\\MandrillServiceProvider'
		\],

		'aliases' => \[
			......
			'MandrillMail'  => 'Weblee\\Mandrill\\MandrillFacade'
		\]
	<!-- endtab -->
{% endtabbed_codeblock %}

Bien, hecho lo anterior tenemos listo nuestra app de Laravel para usar la API de Mandrill desde el controller, simplemente nos queda incluir el NameSpace en el encabezado de nuestro controller use Weblee\\Mandrill\\Mail; y creamos las siguientes dos funciones en nuestro controller:

{% tabbed_codeblock  %}
  	<!-- tab php -->
		/* Send Welcome Email throught mandriall APP
		* https://mandrillapp.com/api/docs/messages.php.html
		* @param  int  $activationLink
		* @param  int  $userEmail
		* @param  int  $userName
		* @return Response
		*/
		private function welcomeEmail($userEmail,$userName){
			try{
				$template_name = 'hola mundo';
				$template_content = array(
					array(
						'name' => 'test',
						'content' => 'test'
					)
				);
				$message = array(
					'to' => array(
						array(
							'email' =>	$userEmail,
							'name' 	=> 	$userName,
							'type' 	=> 'to'
						)
					)
				);
				$result = \\MandrillMail::messages()->sendTemplate($template\_name, $template\_content, $message);
				if($result\[0\]\["status"\]=="sent" AND !$result\[0\]\["reject_reason"\]){
					return true;
				}
				else{
					return false;
				}
			}
			catch(Exeption $e){
				return false;
			}
		}
		public function testEmail(){
			echo $this->welcomeEmail("example@gmail.com","test");
		}
	<!-- endtab -->
{% endtabbed_codeblock %}

En ocaciones algunas personas se pueden encontrar con errores de certificado SSL de Mandrill, esto se debe a que probablemente no estan usando el protocolo HTTPS, esto se soluciona bastante facil al editar el archivo mandrill.php en la linea 65 añadiendo las siguientes dos lineas. curl\_setopt($this->ch, CURLOPT\_SSL\_VERIFYHOST, 0); curl\_setopt($this->ch, CURLOPT\_SSL\_VERIFYPEER, 0); Una vez hecho esto tendremos conectado laravel con Mandrill y podremos enviar los emails de nuestra APP atraves de un Servidor especializado de correos con todas las características que éste ofrece.