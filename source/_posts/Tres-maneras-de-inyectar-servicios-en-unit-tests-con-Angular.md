---
title: Tres maneras de inyectar servicios en unit tests con Angular
date: 2018-06-30 14:54:53
tags:
  - Desarrollo Web
  - Angular
url: 1050.html
thumbnailImage: https://angular.io/assets/images/logos/angular/angular.svg
thumbnailImagePosition: left
id: 1051
categories:
  - Angular
---

Cuando escribimos tests unitarios para nuestros servicios en Angular tenemos varias opciones para inyectar el servicio en cada uno de nuestros "it" bloques, en este post exploraremos cuales son algunas las ventajas y desventajas de cada una de las maneras.
<!-- more -->

Revisemos un servicio súper simple que deseamos probar, este servicio solo tendrá una propiedad y un método para probar.

{% tabbed_codeblock names.service.ts  https://github.com/seagomezar/ng-col-angular-ut/blob/step2/src/app/names.service.ts %}
<!-- tab js -->
class Observable {
    import { Injectable } from '@angular/core';

    @Injectable({
        providedIn: 'root'
    })
    export class NamesService {

        private names: string[] = ['Juan', 'Mati'];

        constructor() { }

        public getNames() {
            return this.names;
        }
    }
}
<!-- endtab -->
{% endtabbed_codeblock %}

Por defecto nuestro archivo de pruebas asociado nos propone inyectar el servicio directamente en cada bloque "it" de esta manera:

{% tabbed_codeblock names.service.spec.ts %}
<!-- tab js -->
    import { TestBed, inject } from '@angular/core/testing'; // We always need this
    import { NamesService } from './names.service'; // Our service to test

    describe('NamesService', () => {
        beforeEach(() => {
            TestBed.configureTestingModule({
            providers: [NamesService] // Service to test
            });
        });

        it('should be created', inject([NamesService], (service: NamesService) => { // Default
        it('should be something', inject([NamesService], (service: NamesService) => { // Default
        it('should be .....', inject([NamesService], (service: NamesService) => { // Default

    /*...*/
<!-- endtab -->
{% endtabbed_codeblock %}

Sin embargo. Te darás cuenta que esta manera requiere repetir la misma línea todo el tiempo. Una ventaja de este enfoque es que estas empezando con un servicio nuevo y limpio en cada bloque it. Entonces naturalmente podemos valernos de Angular TestBed para evitar repetir esta línea. Algo así: 

{% tabbed_codeblock names.service.spec.ts %}
<!-- tab js -->
    beforeEach(() => {
        TestBed.configureTestingModule({
            providers: [NamesService] // Service to test
        });
        // It is another approach to put available the service during the tests.
        service = TestBed.get(NamesService); 
    });
    /** ... */
    it('should be created', ()=> {
        // Service is perfectly available
    });
    
<!-- endtab -->
{% endtabbed_codeblock %}

TestBed.get() es una función que nos permite crear una instancia del servicio que estamos usando. Al crear el servicio dentro del beforeEach estamos garantizando que cada bloque it tenga una versión nueva del servicio evitando empezar con un servicio corrupto por test anteriores. Sin embargo podrías encontrarte con situaciones que requieran que una misma instancia del servicio y para eso la primera forma `inject([NamesService], (service: NamesService) => {` se queda corta. Para tener exactamente la misma instancia del servicio a través los bloque it tendriamos que usar el segundo enfoque `service = TestBed.get(NamesService);` dentro del bloque beforeEach. Veamos un ejemplo de su utilidad con una simple propiedad.

{% tabbed_codeblock names.service.spec.ts %}
<!-- tab js -->
    beforeAll(()=> {
        service = TestBed.get(NamesService); 
    });

    it('should be myVar set to 13', () => {
        service.setMyVar(13);
        expect(service.myVar).toBe(13);
    });

    it('should be myVar still set to 13 and then set to 12', () => {
        expect(service.myVar).toBe(13);
        service.setMyVar(12);
        expect(service.myVar).toBe(12);
    });
<!-- endtab -->
{% endtabbed_codeblock %}

Así tu puedes persistir el estado del servicio a través de los diferentes its y podría sobre todo ser muy útil en pruebas unitarias relacionadas con procesos de CRUD para evitar repetir código y tener la trazabilidad de todo el test. 

Así repasando las tres estrategias para inyectar servicios en nuestros test tenemos:

- `inject([NamesService], (service: NamesService) => {`
- `beforeEach(()=>service = TestBed.get(NamesService);}`
- `beforeAll(()=>service = TestBed.get(NamesService);}`

Eso es todo, espero que este post te sea de utilidad, Si tienes alguna duda no dudes en dejarme un comentario en la parte de abajo, recuerda que si te gustó también puedes compartir usando los links a las redes sociales en la parte de abajo.
