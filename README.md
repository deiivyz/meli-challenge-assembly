# Desaf铆o Mutantes - Meli 馃К

Este desaf铆o consta de una aplicaci贸n desarrollada en java que se encuentra habilitada en la nube y permite mediante el consumo de un servicio post la verificaci贸n de cadenas de adn de diferentes humanos con el fin de determinar si estos son mutantes o no.
La secuencia de adn debe ser una matriz de NxN representada en un array de strings, cuyos caracteres solo pueden ser "A", "G", "T" o "C".
Todos los an谩isis realizados son almacenados en una base de datos mongo desplegada en la nube.
Por medio del consumo de un servicio Get, la aplicaci贸n permite consultar un reporte estad铆stico sobre el an谩lisis realizado a las diferentes secuencias de ADN.

El proyecto est谩 construido bajo el m贸dulo multi m贸dulo de maven, donde contamos con un m贸dulo que almacena nuestra capa web ( **meli-challenge-web-backend** ) y existe un segundo m贸dulo que se encarga de empaquetar de una manera aislada toda nuestra l贸gica de negocio ( **meli-challenge-domain** ).

## Instalaci贸n 馃敡 

### Pre-requisitos 鈿欙笍

- Instalar previamente JDK versi贸n 8
- Instalar IDE java de preferencia
- Configurar lombok en el IDE instalado

### En marcha  馃殌

Luego de clonar el proyecto desde la rama main, realize la importaci贸n de este en el IDE como un proyecto ya existente de maven. 
Para poner en marcha, ejecute la clase principal como una aplicaci贸n java. Este correr谩 por defecto en el puerto 9099.

Para ejecutar los test unitarios, ejecute el siguiente comando ubicado en el assembly del proyecto:
```sh
mvn test
```

## Tecnolog铆as   馃捇 

Para el desarrollo de este proyecto se hizo uso de las siguientes tecnolog铆as :

| Tecnolog铆a | Descripci贸n |
| ------ | ------ |
| [SpringBoot](https://spring.io/projects/spring-boot) |M贸dulo del framework spring para el desarrollo 谩gil de aplicaciones |
| [JUnit](https://junit.org/junit4/) | Biblioteca para la creaci贸n de pruebas unitarias |
| [Maven](https://maven.apache.org/) | Gestor de dependencias |
| [Lombok](https://projectlombok.org/) | Librer铆a para reducci贸n de c贸digo a trav茅s a anotaciones |
| [Swagger](https://swagger.io/) | Herramienta para documentaci贸n automatizada de las APIs |
| [Spring Data MongoDB](https://spring.io/projects/spring-data-mongodb/)  | M贸dulo de Spring que simplifica la persistencia de los datos |
| [SpringBoot Cach茅](https://spring.io/guides/gs/caching/) | M贸dulo para administrar el cach茅 de la aplicaci贸n |
| [SpringBoot Validation](https://spring.io/guides/gs/validating-form-input/)  | M贸dulo para la validaci贸n personalizada de par谩metros mediante anotaciones |

## Sobre la soluci贸n 馃挕
El enfoque que se le di贸 a la soluci贸n de este desaf铆o, mas que recorrer cada elemento de la matriz buscando caracteres semejantes a su alrededor, fue implementar una serie de builders que formaran palabras en todas las posibles direcciones de la matriz (Horizontal, vertical, y todas sus oblicuas), las cuales pasan posteriormente por un proceso de verificaci贸n de coincidencias con los patrones dados en el enunciado.
Las verificaciones positivas se van totalizando para poder determinar si estamos frente al caso de un mutante, y detener el analisis del programa una vez se cumpla la condici贸n, evitando procesar datos innecesarios.

## Probando la aplicaci贸n 馃暤馃徎鈥嶁檧锔?

La aplicaci贸n est谩 desplegada en heroku (PaaS). Para revisar la documentaci贸n de los servicios a consumir ingrese [aqu铆.](https://dz-meli-challenge.herokuapp.com/swagger-ui.html) 

#### Detectar si un humano es mutante

Para esto, partimos de que la secuencia de ADN se recibe como un arreglo de Strings que representan una matriz de (NxX).
Sabremos si un humano es o no mutante si se encuentra mas de una secuencia de 4 letras iguales de forma oblicua, horizontal o vertical, como se puede ver en el siguiente caso:

![N|Solid](https://i.ibb.co/BNzppjh/mutantes-Img.png)

Por postman, consumimos un servicio tipo POST con la url https://dz-meli-challenge.herokuapp.com/mutant/ , y le agregamos a la petici贸n un cuerpo tipo json en el que ir谩 la secuencia de ADN de la siguiente manera : 
* _Caso mutante:_ { "dna": [ 
"ATGCGA",
"CAGTGC",
"TTATGT",
"AGAAGG",
"CCCCTA",
"TCACTG" ] }

* _Caso no mutante:_ { "dna": [ 
"ATGCGA",
"CTGTGC",
"TTATGT",
"AGAAGG",
"CACCTA",
"TCACTG" ] }

En caso de presentar errores en el request, como por ejemplo una matriz M x N , adn nulo o vac铆o, caracteres diferentes a A,T,C,G , la respuesta ser谩 un "Bad Request (400)".

Si la secuencia de ADN corresponde a un mutante, la respuesta ser谩 "HTTP OK (200)".

De lo contrario, si no se trata de un mutante, la respuesta ser谩 "Forbidden (403)".

#### Consultar estad铆siticas

Para esto, se debe consumir el servicio GET con url https://dz-meli-challenge.herokuapp.com/stats/, el cual tiene como respuesta un json con la siguiente estructura:
{
"count_mutant_dna":40,
"count_human_dna":100, 
"ratio":0.4
}
* _count_human_dna_ : Cantidad de adns evaluados
* _count_mutant_dna_ : Cantidad de adns evaluados que han sido mutantes
* _ratio_ : Porcentaje del total equivalente a humanos mutantes 

## Autor 鉁嶏笍锔?

* **Carlos David Zapata R煤a** - *Construcci贸n y documentaci贸n.* - [David Zapata](https://www.linkedin.com/in/deiivyzapata/) 
