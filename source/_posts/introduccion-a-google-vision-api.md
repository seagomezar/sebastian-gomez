---
title: Introducción a Google Vision API
tags:
  - Google
  - NodeJS
  - Inteligencia Artificial
url: 483.html
id: 483
thumbnailImage: https://storage.googleapis.com/sebastian-gomez-blog.appspot.com/uploads/2017/12/cloud-vision.jpg
thumbnailImagePosition: left
categories:
  - Desarrollo Web
date: 2017-12-13 16:27:14
---

Siempre me ha fascinado la inteligencia artificial. De hecho en el pasado he dictado algunos cursos en algunas universidades en Medellín sobre inteligencia artificial y la temática es tan larga y extensa que a veces los primeros acercamientos de una persona a este tema resultan escabrosas y es probable que se rindan antes de tratar de hacer algo tangible usando inteligencia artificial. Por esto he querido crear esta historia ademas porque servirá de memoria para una presentación que haré en el Google DevFest de Medellín.

<!-- more -->

Empecemos por lo básico, quiero mostrarles una de las APIs de la toda la suite de servicios en la nube que posee Google en su producto Google Cloud Platform, Google Vision API como su nombre lo indica es una API Rest que te permite hacer un análisis detallado y un procesamiento profundo de imágenes. Si bien esta disponible para casi todos los lenguajes modernos de programación me ha parecido interesante entender como podemos poner a funcionar esta API con Javascript y afortunadamente poseen una librería/SDK que funciona perfectamente con NodeJS.

¿Como funciona Google Vision API?, pues bien, básicamente Google tiene indexada una gran colección de imágenes que ha venido creciendo a lo largo de su historia. Estas imágenes son el insumo básico para un proceso riguroso de clasificación que se ha venido mejorando cada día en Google, esto permite entonces que una vez que una imagen necesita ser analizada esta API tratará de encontrar imágenes similares y obtener los metadatos de estas imágenes similares. Por ejemplo si subes una imagen de tu perro o tu gato primero la API tratará de encontrar fotos similares y encontrará por supuesto algunas fotos con perros, luego de ese conjunto de fotos con perros tratará de encontrar algunas fotos con el mismo color, tamaño de las formas entre otras posibles características. Finalmente extraerá las etiquetas de esas imágenes similares y te las presentará como si fueran las de tu imagen. Bien, probablemente a este punto dirás “Por supuesto, esto no es Inteligencia Artificial” y tal vez estés en lo correcto.

Si bien el proceso de clasificación no es un proceso “inteligente”, estrictamente hablando si es un insumo necesario para el aprendizaje de máquina (Machine Learning). También es el insumo para extraer patrones sobre las imágenes y poderlos comparar para generar conocimiento he incluso para generar conocimiento sobre lo aprendido (Deep Machine Learning), todos estos procesos hacen parte de la Google Vision API con agravante adicional, la potencia de computo de los servidores de Google (tal vez los más poderosos del mundo) han venido trabajando desde hace algún tiempo detectando patrones sobre las imágenes, procesando millones de imágenes cada día y aprendiendo de lo que nosotros como humanos creemos que hay en una imagen.

Finalmente, te estarás preguntando… bueno si … se ve bien pero debe costar un ojo de la cara, la respuesta es no, afortunadamente para nosotros los desarrolladores, Google Vision API tiene una capa gratuita de hasta 1000 peticiones por mes, y luego $1.50 dólares aproximadamente por cada bloque de 1000 peticiones. Aquí puedes consultar más sobre los precios. Así que no hay porque al menos no probar esta API.

De igual manera debes saber que no es la única API de visión artificial y procesamiento de imágenes que podrás encontrar, de hecho hay alternativas interesantes como Microsoft Azure y OCR.Space que podrás comparar y usar. Estas alternativas son incluso más baratas pero dependerá de tus necesidades específicas cual y cuando usar cada una de ellas. Aquí te dejo una comparación de precios.

![](https://cdn-images-1.medium.com/max/1600/1*oVPEOdCfb3Dq6d4FXHrPCw.png) Comparación de precios Google Cloud, MS Azure y OCR, tomado de: https://ocr.space/compare-ocr-software. Eso es todo por esta historia, en el siguiente Post voy a tratar de explicar cómo conectar y usar esta API con NodeJs.