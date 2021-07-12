# Modelo de trabajo para Recetas electrónicas FIDE

Se propone un sistema de trabajo que permita la interacción de múltiples participantes con responsabilidades plenamente diferenciadas. 

* Sistemas de emisión de recetas
* Sistemas de revisión y validación de recetas
* Sistemas de validación de datos médicos
* Plataformas de distribuición
* Farmacias
* Laboratorios
* Sistemas de cotización 
* Sistemas de salud 
* Autoridades competentes

Adicionalmente, se plantea la creación de una *Unidad centralizadora*, misma que sería responsable del manejo final de los estados de cada receta y de simplificar los procesos de comunicación entre todos los participantes del modelo.

## Consideraciones del modelo

Un participante que desee generar recetas, deberá registarse en la unidad centralizadora, misma que le otorgará un prefijo de identificación y el acceso a los estándares de desarrollo disponibles. 

Empleando el prefijo de identificación, el implementador que desee generar recetas, podrá emitir recetas en el estándar que serán aceptadas por cualquier participante de FIDE. Para ello, el validador hará uso del servicio de la Unidad Centralizadora para conocer el status de surtido de una receta. 

La unidad centralizadora será alimentada a su vez por entidades de verificación de datos, mismas que se encargarán de revisar que los datos médicos de las recetas se encuentren completos y sean válidos.

Para el procesamiento de las recetas FIDE se tienen múltiples opciones:

- *Despacho físico o digital en una Farmacia asociada* Para éste caso, la misma, tendría que hacer uso de un servicio FIDE de revisión y validación de recetas en la unidad centralizadora, así como de un servicio de *notificación de despacho* (total o parcial) de la receta. En el caso de una farmacia dígital, podrá emplear los servicios de plataformas de distribución FIDE asociadas o sus propias plataformas.

- *Despacho digital en un sistema de cotización* Para éste caso, el sistema se encargará de realizar la revisión y validación de la receta en la unidad centralizadora. Pasará el requerimiento a la Farmacia (física o digital) y ésta será la encargada de la notificación del despacho a la unidad centralizadora. En el caso de una farmacia dígital, podrá emplear los servicios de plataformas de distribución FIDE asociadas.

La *Unidad centralizadora* será responsable de mantener la información actualizada de cada receta, así como de proveer información "de rastreo" a las entidades generadoras de la receta. Mediante los mecanismos de protección FIDE, sólo el emisor de la receta podrá conocer los detalles del status de la misma.

La *Unidad centralizadora* entregará los reportes pertinentes a las autoridades competentes y a los sistemas de salud que la ley requiera. De la misma forma, podrá exponer reportes generales de operación a los participantes.

