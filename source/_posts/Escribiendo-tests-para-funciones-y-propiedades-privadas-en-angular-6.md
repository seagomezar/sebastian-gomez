---
title: Escribiendo tests para funciones y propiedades privadas en angular 6
date: 2018-06-15 13:24:29
tags:
  - Desarrollo Web
  - Angular
url: 1050.html
thumbnailImage: https://angular.io/assets/images/logos/angular/angular.svg
thumbnailImagePosition: left
id: 1050
categories:
  - Angular
---

Escribir pruebas unitarias debe convertirse en una tarea diaria, no es posible crear una aplicación en Angular de calidad sin realizar un proceso consciente de unit testing. En este post veremos como probar funciones que retornan propiedades privadas en Angular 6 con Karma y Jasmine.
<!-- excerpt -->

Vamos a suponer que sabes que es Angular y que sabes como crear un servicio en Angular, por tanto habrás notado que al escribir `ng generate service <nombre del servicio>` se crean dos archivos, el primero es tu servicio y el segundo notarás que se crea con el mismo nombre de tu servicio pero con la extensión .spec al final, te darás cuenta que lo único que quiere hacer este archivo es proveerte un esqueleto fácil y entendible en el que puedas empezar a escribir pruebas de la manera más fácil posible.

Empecemos por algunos fundamentos. Supongamos que decidiste crear un servicio llamado "names" porque será un servicio que retorne una lista de nombres. Para ello vas a guardar dichos nombres en una propiedad privada y vas crear un metodo get para devolver dichos nombres a un controlador u otro servicio. Sería algo así:

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

Ahora bien, nuestra primera tentación será escribir en nuestra prueba unitaria algo como esto: 

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

        it('should be created', inject([NamesService], (service: NamesService) => {
            expect(service).toBeTruthy(); // We hope the service has been inyected and defined
        }));

        it('should return all names', inject([NamesService], (service: NamesService) => {
            expect(service.getNames()).toBe(service.names); // Not allowed
            expect(service.getNames()[0]).toBe(service.names[0]); // Not allowed again
            expect(service.getNames()[0]).toBe('Mati); // Ok, but Seriously
        }));

    });
<!-- endtab -->
{% endtabbed_codeblock %}

Sin embargo, te darás cuenta de desafortunadamente no puedes acceder a la propiedad names del servicio porque dicha propiedad es privada. Tu test debería estar fallando. Por eso te verás tentado a comparar con los nombres directamente lo cual no resulta ser una buena idea si la cantidad de nombres es muy grande y si cambia constantemente. Por eso lo que tu podrías hacer es omitir a typescript y acceder al objeto mediante la sintaxis de array (cosas que solo javascript nos permite). `service["names"]` miremos como quedaría:

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

        it('should be created', inject([NamesService], (service: NamesService) => {
            expect(service).toBeTruthy(); // We hope the service has been inyected and defined
        }));

        it('should return all names', inject([NamesService], (service: NamesService) => {
            expect(service.getNames().length).toBe(service["names"].length); // OK
            expect(service.getNames()[0]).toBe(service["names"][0]); // OK
        }));
    });
<!-- endtab -->
{% endtabbed_codeblock %}

Habrá muchas discusiones sobre si es correcto teóricamente probar las propiedades privadas o no, si es correcto aplicar esta sintaxís o si el test es limpio o no. Sin embargo esto no tiene una respuesta concreta. Dependerá de la filosofía del equipo y de la guía de estilo que tu equipo adopte. Personalmente y en mis equipos de trabajo me gusta tener una navaja suiza de opciones para diferentes casos y tipos de proyecto por lo cual puesde ver esta opción como una herramienta más dentro de tus habilidades de testing.

Eso es todo, espero que este post te sea de utilidad, Si tienes alguna duda no dudes en dejarme un comentario en la parte de abajo, recuerda que si te gustó también puedes compartir usando los links a las redes sociales en la parte de abajo.