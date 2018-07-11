---
title: Creación y Personalización de interfaces gráficas usando NetBeans
tags:
  - Java
  - Netbeans.
  - Programación orientada a objetos.
url: 191.html
id: 191
categories:
  - Java
date: 2015-10-20 15:50:28
---

Trabajar JAVA desde NetBeans es una gran manera de agilizar algunos de nuestros desarrollos ya que nos permite hacer muchas cosas, pero una de las más importantes cosas es que nos ofrece NetBeans es la creación de interfaces gráficas por medio de una interfaz gráfica (Valga la redundancia). 
<!--more --> Podemos programar aplicando varios de los conceptos de la programación orientada a objetos: polimorfismo, herencia, todo lo que queramos por consola, pero, ¿Basta simplemente con limitarnos a ver los resultados que arroja nuestra aplicación en una consola o una ventana de resultados? Es por esto que si queremos avanzar, hacer y ver las cosas de una manera diferente, además de tener una aplicación con vista amigable ante los ojos del usuario o cliente, debemos empezar a trabajar con interfaces graficas o los famosos “form” que nos brindan muchísimas herramientas de programación.

Es por esto que en este pequeño tutorial trataremos sobre este tema y se enseñarán las principales cosas o básicas a tener en cuenta, además de cómo hacerlo, al momento de trabajar con interfaces gráficas.

Empezaremos creando un nuevo proyecto en NetBeans como normalmente lo hacemos.

[![poo-1](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-1.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-1.png)

Crearemos un paquete en nuestra aplicación y dentro de él agregaremos un nuevo JFrame form, con el cual haremos un Loguin o Inicio de sesión a nuestra aplicación por medio de Interfaces gráficas. Es de muchísima importancia que nuestras aplicaciones a desarrollar tengan controles de accesos con inicios de sesión, puesto que cualquiera podría meterse a ella y arruinar o hacer modificaciones no deseadas a la aplicación de la compañía.

 [![poo-2](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-2.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-2.png)

Bastará con hacer click derecho sobre el paquete > New > JFrame form… Y ya tendremos la interfaz de lo que será nuestro Loguin.

[![poo-30](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-30-300x123.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-30.png) [![poo-3](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-3.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-3.png)

Por lo que general en JAVA NetBeans luego de hacer esto nos quedara nuestra interfaz base y el cuadro de herramientas al lado derecho con el que trabajaremos.

[![poo-4](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-4-300x227.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-4.png)

Dentro de la caja de herramientas o “Palette” nos iremos a Swing Controls y de allí arrastraremos un “Panel” o JPanel el cual bastara con arrastrarlo hasta nuestra interfaz. [![poo-5](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-5.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-5.png) Una vez hecho esto, después de agregar todos los Label, Button y Text Field necesarios tendremos algo parecido a esto: [![poo-6](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-6-300x224.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-6.png)

Los “Label” nos servirán para poner anuncios o textos simples dentro de nuestra interfaz, solo basta con arrastrar uno hasta la interfaz, darle click derecho > Edit Text y ponerle el texto deseado a mostrar.

Los “Text Field” o “Text Box, cajas de texto” serán los más importantes, puesto que nos servirán para recibir los datos o valores con que nuestra aplicación trabajará, hará cálculos o guardará en una base de datos en caso de estar trabajando con una.

Y finalmente los “Button” o botones, nos desencadenaran la acción que queramos al momento de hacer click sobre alguno de ellos.

**OJO:** _Es importante saber que el TextField de “Password” no será una caja de texto común y corriente a las demás, puesto que ahí se ingresará la contraseña para el inicio de sesión._

[![poo-7](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-7.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-7.png)

_Es por esto que dentro de la caja de herramienta buscaremos y arrastraremos un “Password Field” que será el que usaremos al momento de ingresar la contraseña para el inicio de sesión en nuestra aplicación. Más adelante veremos por qué no utilizar “TextField” o caja de texto común y corriente para el ingreso de la contraseña._

Para empezar a programar la funcionalidad del botón o del inicio de sesión, daremos doble click al botón de nuestra interfaz, al que le dimos el nombre de “Ingresar” y nos abrirá dentro del código su evento o acción en la cual ingresaremos el código que nos servirá para que en el momento de hacer click al botón, éste ejecute el código dentro de él y así desencadene o dispare la acción a realizar.

[![poo-8](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-8-300x48.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-8.png) Procedemos a insertar el código: [![poo-9](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-9-300x161.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-9.png)

**NOTA:** _Para entender la funcionalidad del código y leer la explicación completa de cada uno, favor revisar los archivos .java del ejercicio, puesto que la explicación principal del tutorial es interfaces gráficas y por resumir no se explicarán dichos códigos aquí, ya están explicados dentro de los archivos…_

Ahora ya después de insertar el respectivo código dentro del botón, ya tendremos la funcionalidad de éste lista, así cada vez que el usuario ingrese su ID, contraseña y presione el botón “Ingresar” dicho código y acción se ejecutará y validará los datos recién ingresados, en caso de ser correctos le permitirá pasar al menú principal o en caso contrario le mostrará un mensaje indicando que ha ingresado algún dato incorrecto.

Para personalizar nuestra ventana con un fondo, además de ponerle un logotipo o icono en la parte superior de la ventana, procedemos a ingresar el siguiente código en la clase principal de nuestro JFrame Loguin:

**Nota_:_** _Al momento en que visualizamos nuestra ventana “JFrame” estamos parados sobre la opción “Design”, para programar código dentro de tal, debemos seleccionar la opción “Source”_

[![poo-10](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-10-300x34.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-10.png) [![poo-11](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-11-300x171.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-11.png)

En el código anterior, estamos haciendo referencia a la imagen que será nuestro fondo y a la que será nuestro icono superior de la ventana. Esto se hace indicando la ubicación, el nombre del paquete o carpeta dentro del proyecto en donde están las imágenes y el nombre de la imagen a utilizar. _(Es importante poner los nombres tal cual, puesto que cualquier letra faltante, una mayúscula o minúscula no debida nos generaría un error)_

Por ende crearíamos un nuevo paquete dentro de nuestro proyecto donde sólo almacenaremos las imágenes o iconos a utilizar. Para agregarlas basta con arrastrarlas desde el “Escritorio” de nuestro computador o desde donde las tengamos hasta el paquete dentro de NetBeans.

[![poo-12](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-12.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-12.png)

Luego de haber seguido todos los pasos anteriores ya deberíamos tener nuestra ventana de Inicio de sesión de la siguiente forma al momento de ejecutar el proyecto;

A excepción del tipo de letra y color que se escogen a gusto personal por medio de las propiedades de cada “Label”, se selecciona el Label > click derecho > Propiedades y ahí cambiaríamos los estilos de letra y color.

Nótese el icono en la parte de arriba de la ventana y un pequeño título.

[![poo-13](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-13-300x256.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-13.png)

Es importante notar que si ingresamos el ID de usuario se visualizarán los caracteres de forma normal puesto que para él utilizamos un “TextField”.

Mientras que al momento de ingresar la contraseña, sólo se visualizarán asteriscos en vez de los caracteres verdaderos que componen nuestra contraseña, esta es la propiedad única del “Password Field”, la cual nos ayuda a proteger nuestra contraseña haciendo que nadie la visualice al momento en que la digitamos.

[![poo-14](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-14-300x254.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-14.png) En caso de ingresar algún dato incorrecto nos mostrará el siguiente mensaje. [![poo-15](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-15-300x255.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-15.png) Y luego de los tres intentos erróneos nos mostrará otro mensaje y nos sacará de la aplicación. [![poo-16](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-16-300x246.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-16.png) Luego de iniciar sesión nos debe mandar a una nueva ventana la cual será nuestro menú principal, procedemos a crearla. [![poo-17](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-17-300x211.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-17.png)

Para ello creamos un nuevo JFrame form de la misma forma en que creamos el primero y le agregamos Label y botones, un botón “Calculate” que nos llevará a otra nueva ventana y uno “Exit” que nos sacará de la aplicación.

Lo único diferente que tendremos en esta nueva ventana será un “Menu Bar” que nos servirá para tener accesos directos y hacer las mismas funciones que hacen los dos botones.

Bastará con arrastrarlo desde la caja de herramientas y acomodarlo a gusto en nuestro JFrame form.

[![poo-18](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-18.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-18.png) Luego de arrastrarlo procedemos a darle nombre a cada opción. [![poo-19](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-19-300x16.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-19.png) Ahora desde la caja de herramientas arrastraremos un “Menu Item” para cada una de las dos opciones. [![poo-20](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-20.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-20.png) Bastará con seleccionar el “Menu Item” y arrastrarlo hasta la parte de arriba del “Menu Bar” donde dice Calcular y soltarlo. [![poo-21](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-21.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-21.png) Y ahora ya tenemos una sub-opción de nuestro acceso directo Calcular, el cual después de programarlo y añadirle una imagen, quedaría de la siguiente forma: [![poo-22](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-22.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-22.png) Lo mismo hacemos con el de Salir, agregamos un “Menu Item” y lo personalizamos. [![poo-23](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-23.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-23.png)

Para personalizar cada opción debemos de seleccionar el “Menu Item” > click derecho > Propiedades y desde allí seleccionar una imagen deseada que se encuentre dentro de nuestro proyecto y agregarle un texto. Ejemplo:

[![poo-24](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-24-300x144.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-24.png)

Nótese lo anterior donde están los puntos rojos. Se selecciona un icono, se agrega un texto a mostrar y en “acelerator” se pone una tecla de acceso directo, la cual también se visualizará en forma de texto;

[![poo-25](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-25.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-25.png)

Con la única diferencia que si presionamos “Ctrl + S” nos ejecutará la acción deseada que programaremos a continuación dentro del código del JFrame form.

_(También en vez de presionar las teclas de acceso directo podemos hacer simplemente click y nos ejecutará la misma acción.)_

[![poo-26](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-26-300x192.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-26.png)

Cada programación de los botones o accesos directos se hace y trabaja de la misma forma en que se programaron los botones en el JFrame form del Loguin. Luego de programar cada botón y acceso directo, además de personalizar con imagen de fondo e icono de ventana nuestro JFrame que será el menú principal, nos quedaría de la siguiente forma al ejecutarlo:

[![poo-27](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-27-300x233.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-27.png) Con los accesos directos totalmente funcionales, ya sea por medio de click o las teclas: [![poo-28](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-28-300x235.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-28.png)

Ahora crearemos la ventana “Calcular” a la cual accederemos después de dar click al botón “Calculate” desde el anterior menú principal.

Creamos un nuevo JFrame form y le agregamos los Label, Botones, Cajas de texto y demás cosas como lo explicamos al inicio del tutorial.

[![poo-29](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-29-300x172.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-29.png)

Agregaremos tres “TextField”, dos que nos servirán para ingresar dos números y otro que será para mostrar el resultado de la operación.

También dispondremos de cuatro botones para realizar los cálculos, Sumar, Restar, Multiplicar y Dividir, además de uno para limpiar los campos y otro para regresar al menú principal.

Procedemos a programar:

[![poo-30](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-30-300x123.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-30.png)

En el Método principal de la Clase de nuestro JFrame form aplicamos el código para personalizar la ventana con imagen de fondo e icono de ventana como anteriormente lo hemos hecho en las ventanas anteriores.

Programamos el evento click o acción del botón suma con el siguiente código:

[![poo-31](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-31-300x61.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-31.png)

Los demás botones, Restar, Multiplicar y Dividir se programan de la misma forma con el mismo código, la única diferencia es que cambian los signos en la variable “Reslt” donde se realiza la operación; **+**, **-**, *****, **/**

[![poo-32](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-32-300x215.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-32.png)

Acciones a ejecutar cuando se de click a los botones Limpiar o Regresar:

 **[![poo-33](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-33-300x86.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-33.png)** Ventana Calculadora ya ejecutada y aplicando la acción o evento del botón “SUMAR”: [![poo-34](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-34-300x209.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-34.png)

**Nota:** _Hay que tener en cuenta que dentro del código para trabajar con todos los objetos de JFrame form es necesario añadir las librerías correspondientes:_

[![poo-35](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-35-300x69.png)](https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2015/10/poo-35.png)

Y eso es todo tendremos lista una sencilla pero bonita interfaz gráfica. Si quieres descargar los archivos fuente haz [click aquí](http://sebastian-gomez.com/shared/sources.zip).

(Este tutorial ha sido hecho con la colaboración del estudiante Diego Ramirez Montes del curso de programación orientada a objetos).