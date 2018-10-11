---
title: Creación de un VHOST en Windows y XAMPP
tags:
  - Backend
  - Desarrollo Web
  - PHP
  - Servidores
  - VHOST
  - Windows.
  - Xampp
url: 51.html
id: 51
categories:
  - Desarrollo Web
date: 2015-05-26 18:22:26
---

Un VHOST o virtual host, es un apuntador interno que tendremos en nuestro computador a alguna aplicación web nuestra bajo un dominio o nombre que deseemos establecerle. Es decir si tenemos un proyecto en una carpeta dentro de XAMPP C:/xampp/htdocs/my-proyecto y su respectiva dirección local http://localhost/my-proyecto podremos facilmente crear un dominio ficticio dentro de nuestra máquina a dicho proyecto por ejemplo www.my-proyecto.com (esto solo será accesible desde nuestra máquina). En este sencillo post mostraré un pequeño tutorial de como crear un Virtual Host (VHost) en Windows y un servidor Apache como XAMPP. Como prerequisito entonces tendremos que tener instalado XAMPP (el link es [este](https://www.apachefriends.org/es/index.html) ). <!-- more -->

1.  Ubicar la carpeta C:\\Windows\\System32\\Drivers\\etc
2.  Abrir el archivo hosts con un editor de texto.
3.  Añadir la siguiente linea al final del archivo

```
127.0.0.1       www.my-proyecto.com
```

La url www.my-proyecto.com puede ser cambiada por el nombre que se desee, e incluso es posible tener varios subdominios, por ejemplo testing.my-proyecto-com, production.my-proyecto.com, etc etc y la dirección 127.0.0.1 nunca cambia a menos que tu hayas cambiado la configuración de hosts de windows. Hecho lo anterior entonces debemos abrir XAMPP y se mostrara la consola inicial: [![xampp](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/05/xampp-300x109.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/05/xampp.png)   En esta consola haremos click sobre el boton config de la fila correspondiente al Apache y seleccionaremos el archivo httdp.conf, hecho esto se nos abrirá una documento de texto. Iremos hasta el final del documento.

#El signo de numeral permite ingresar comentarios

```
<VirtualHost *:80>
DocumentRoot /xampp/htdocs/my-proyecto
ServerName www.my-proyecto.com
</VirtualHost>
```

Note que el puerto 80 representa el puerto donde el apache se encuentra corriendo, si usted esta usando otro puerto debe cambiarlo en la segunda linea. El DocumentRoot indica la ruta física en el disco donde se encuentra el proyecto, y finalmente el server name debe apuntar a la dirección local que creamos en nuestro archivo hosts de windows. Espero que sea de utilidad.