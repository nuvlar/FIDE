# Estándar de recetas electrónicas FIDE

*Aviso: La versión 0.2 de este estándar continúa en desarrollo activo, para una versión estable del estándar favor de referir a la versión [0.1](estandar-0-1.md)*

El estándar FIDE de recetas electrónicas es un estándar desarrollado de manera conjunta con distintas empresas y organizadores del sector médico diseñado para crear una receta autocontenida y sin necesidad de un servidor centralizado de validación para la mayoría de los casos. 

Este estándar está diseñado para ser compatible con los lineamientos de HL7 FIHR, permitiendo una implementación sencilla en sistemas que ya utilicen este estándar, así como en sistemas que no lo hagan.

Para consultar la propuesta de modelo, se puede referir a: [Modelo de operación FIDE con Unidad Centralizadora](Model.md)

Como propuesta inicial de desarrollo, las recetas de FIDE se firman por medio del estándar JSON Web Token ([RFC 7519](https://tools.ietf.org/html/rfc7519)) con el algoritmo de firmado RS256 del estándar JSON Web Signature ([RFC 7515](https://tools.ietf.org/html/rfc7515)). Queda abierta la posibilidad de incorporar posteriormente distintas tecnologías respetando el esquema de la receta. 

Para simplificar la documentación se denominará "Receta Electrónica Firmada" o "REF" al contenido (payload) que se utilizará como herramienta de comunicación. En primera instancia, se refiere al "JWT", sin embargo, puede ser extendido con otras tecnologías.

### Tecnologías compatibles
- [Estructura JWT](JSON.md) *Propuesta
- Estructura XML

## Estructura de recetas (Ver. 0.2)

El payload de las recetas deben de tener la siguiente estructura

|Campo|Tipo|Requerido|Explicación|
|--|--|--|--|
|version|string|Sí|Identifica la versión de estándar bajo la cual se emitió la receta (Actual: "FIDE-0.2")|
|jti|string|Sí|El identificador único de la receta. Debe de contener la suficiente información para que las posibilidades de duplicarse (incluso entre plataformas expedidoras) sea mínima|
|iss|string|No|El identificador de la entidad (empresa o sistema) que generó la receta.|
|exp|int (unixtime)|No|El momento en el tiempo a partir del cual la receta será inválida|
|nbf|int (unixtime)|No|El momento en el tiempo a partir del cual la receta será válida|
|dtc|int (unixtime)|No|El timestamp de la fecha y hora de consulta|
|generalInstructions|string|No|Indicaciones generales de la receta|
|environment|string|Sí|El ambiente en el que se emite la receta (puede ser "dist" para distribución o "dev" para pruebas). Las farmacias sólamente aceptarán las recetas emitidas en ambiente "dist"|
|certificateURL|string|Sí, en recetas firmadas por servidor, y no por médico|URL HTTP del certificado público de la llave utilizada para firmar la receta, en caso de no utilizar la llave privada del médico. Solamente válida cuando no haya medicamentos controlados.|
|requester|object Requester|Sí|Un objeto Requester con los datos del médico que emite la receta|
|subject|object Subject|Sí|Los datos del paciente|
|medication[]|object array Medication|Sí|El tratamiento (medicamentos) que el paciente necesita|
|diagnostics[]|object array Diagnostic|No|Los diagnósticos asociados a la receta|


### Objeto Requester
|Campo|Tipo|Requerido|Explicación|
|--|--|--|--|
|identifier|string|No|El identificador interno del médico (en la plataforma emisora de la receta).|
|title|string|No|El título del médico (Dr., Dra., FT, etc.)|
|name|string|Sí|El nombre del médico que emite la receta|
|certSerial|string|Sí, en recetas de medicamentos controlados|URL de consulta del El del certificado público del médico (archivo .cer). Éste puede provenir del SAT o de alguna entidad diferente, como la secretaría de economía o la autoridad competente |
|telephone|string|Sí|Número telefónico del médico (en formato internacional, +525844392754)|
|email|string|No|Dirección de correo electrónico del médico|
|qualification[]|object array Qualification|Sí|Arreglo de objetos de conteniendo la formación del médico|
|qualification[].name|string|Sí|El nombre de la especialidad (o medicina general en su caso) del médico|
|qualification[].identifier|string|Sí|Número de cédula profesional del médico emitida por la SEP|
|qualification[].issuer|string|Sí|Institución certificadora del médico (escuela que emite el título)|
|qualification[].year|int|No|Año de expedición de la cédula|
|gender|string enum|No|El género del médico: `[male,female,other,unknown]`|
|birthDate|string date|No|La fecha de nacimiento del médico en formato YYYY, YYYY-MM, or YYYY-MM-DD |
|rfc|string|No|Número de Registro Federal de Contribuyentes (RFC) del médico|
|curp|string|No|Clave Única del Registro de la Población (CURP) del médico|
|address|object Address|Sí|Objeto con la dirección del consultorio del médico|

### Objeto Subject

|Campo|Tipo|Requerido|Explicación|
|--|--|--|--|
|subject.name|string|Sí|El nombre completo del paciente|
|subject.identifier|string|No|El identificador interno del paciente en la plataforma|
|subject.rfc|string|No|Número de Registro Federal de Contribuyentes (RFC) del paciente|
|subject.curp|string|No|Clave Única del Registro de la Población (CURP) del paciente|
|subject.telephone|string|No|Número telefónico del paciente (en formato internacional, +525844392754)|
|subject.email|string|No|Dirección de correo electrónico del paciente|
|subject.gender|string enum|No|El género del médico: `[male,female,other,unknown]`|
|subject.weight|float|No|Peso(en kg) del paciente|
|subject.height|float|No|Altura (en cm) del paciente|
|subject.birthDate|string date|No|La fecha de nacimiento del paciente en formato YYYY, YYYY-MM, or YYYY-MM-DD |
|subject.bloodPressure|object|No|objeto con presión arterial del paciente (mmHg)|
|subject.bloodPressure.systolic|int|Sí|Presión sistólica del paciente|
|subject.bloodPressure.diastolic|int|Sí|Presión diastólica del paciente|
|subject.temperature|float|No|Temperatura (en centígrados) del paciente|
|subject.address|object Address|No|Objeto con la dirección de vivienda del paciente|


### Objeto Medication

|Campo|Tipo|Requerido|Explicación|
|--|--|--|--|
|name|string|Sí|El nombre del medicamento en texto plano|
|sustance|string|Sí|La composición o sustancias activas del medicamento en texto plano|
|dosageInstruction|object Dosage|Sí|Un objeto de tipo Dosage con las instrucciones de |
|identifier|string|Sí|El identificador interno del medicamento (SKU de plataforma emisora)|
|code|string|No|El identificador universal del medicamento (EAN), siguiendo estándares como  RxNorm, SNOMED CT, IDMP, etc. Si no hay un código a utilizar, escribir el nombre del medicamento|
|form|string|No|El identificador de forma farmacéutica siguiendo el diccionario FIDE-FORM-1|
|admin|string|No|El identificador de la vía de administración siguiendo el diccionaro FIDE-ADMIN-1|
|fraction|int|Sí|La fracción legislativa del medicamento|
|initDate|int (unixtime)|No|Fecha de inicio del tratamiento|
|diagnostics[]|object array Diagnostic|No|Diagnósticos relacionados con el medicamento|
|ingredient\[\]|object array Ingredient|No|Un arreglo de los ingredientes que tiene el medicamento (pueden ser activos o no activos)|

### Objeto Ingredient

|Campo|Tipo|Requerido|Explicación|
|--|--|--|--|
|description|text|Sí|Nombre del ingrediente|
|code|int|No|Código SNOMED del ingrediente (http://snomed.info/sct)|
|category|string|No|Categoría de ingrediente según https://www.hl7.org/fhir/valueset-substance-category.html|
|strength|object|No|El radio de concentración de sustancia|
|strength.numerator|object|No|El numerador del radio de concentración de sustancia|
|strength.denominator|object|No|El denominador del radio de concentración de sustancia|

### Objeto Address

|Campo|Tipo|Requerido|Explicación|
|--|--|--|--|
|address.country|string|No|País|
|address.state|string|No|Estado|
|address.city|string|No|Ciudad|
|address.postalCode|string|No|Código postal|
|address.line|string|Sí|Lugar y dirección|

### Objeto Dosage

El objeto Dosage se compone de los siguientes campos:

|Campo|Tipo|Requerido|Explicación|
|--|--|--|--|
|text|string|No|Explicación en texto sobre cómo tomar el medicamento|
|additionalInstructions|int enum|No|El código de instrucciones adicionales según [SNOMED CT Additional Dosage Instructions](https://www.hl7.org/fhir/valueset-additional-instruction-codes.html)|
|frequency|string FIDE-FREQUENCY-1|No|Describe la frecuencia del tratamiento según la metodología FIDE-FREQUENCY-1|

## Objeto Diagnostic

|Campo|Tipo|Requerido|Explicación|
|--|--|--|--|
|code|string|Si|Código CIE del diagnóstico|
|versionCode|int|Si|Versión CIE|
|name|string|Si|Diangóstico en texto plano|
|codeName|string|Si|Nombre del diagnóstico asociado CIE|

## Estructura de QR

Un QR de receta FIDE contiene un apuntador a un servicio que proporcione mediante una peticion el texto plano (`text/plain`) de la estructura de la receta firmada. La estructura del contenido del QR debe de contener los siguientes elementos:

1. El identificador de serivio `fide` seguido por un caracter de dos puntos `:`.
2. El URL del endpoint al cual se va a acceder, con los siguientes parámetros GET:
   * iure: el identificador único de la receta electrónica
   * sd: el hash SHA-256 de la REF de la receta. Este parámetro es necesario para prevenir el filtrado de información personal mediante ataques por fuerza bruta.
3. Se recomienda codificar el string formado en [Base 32](https://tools.ietf.org/html/rfc4648) y proceder a reemplazar cada caracter padding '=' por '0'. Esto para conseguir con una cadena conformada unicamente por caracteres alfanuméricos y evitar problemas de lectura del QR en equipos de escaneo cuya configuración de teclado pueda malinterpretar la lectura de caracteres especiales (‘:’, ‘’, ‘-’, '=', etc ...)
4. De manera adicional, se puede incorporar un parámetro que haga referencia a la tecnología de comunicación que se deseea emplear: JSON, XML, o cualquiera de sus variantes

```
fide:https://mirecetadigital.com/receta.php?iure=12345&sd=9f86d081884c7d659a2feaa0c55ad015a3bf4f1b2b0b822cd15d6c15b0f00a08
```

```
MZUWIZJ2NB2HI4DTHIXS63LJOJSWGZLUMFSGSZ3JORQWYLTDN5WS6YLQNEXXMMJPOJSWGZLUMEXHA2DQH5UXK4TFHUYS2OBVG4WTCNRRHE2TINJWGU2CM43EHU3TAMRSGVRDSNJQGE2GCMZQMFRDQYRVHAZDSMBWMMZWIYTBHE3TAMZUHFSDMZRVGNQTGNBWME2WIYRTHFQWKNBQGEYWIZRYGYZDMNBS
```

_Un QR correspondiente a una receta FIDE debe ser idempotente. La URL siempre debera regresar la misma receta FIDE si se usan los mismos parametros URL._

## Proceso de Validación de recetas 

1. El paciente acude a una Farmacia (digital o físicamente) con una Receta Electrónica 
2. Si se trata de una farmacia física, el empleado escanea el QR de la receta para obtener la URL de la misma, si se trata de una farmacia digital, el programa de la farmacia recibe la URL de la receta directamente. En caso de que el QR se encuentre codificado en Base32 como se especificó anteriormente, revertir el reemplazo de caracteres '=' por '0' y decodificar el QR Base32.
3. El programa de farmacia hace una petición al URL de la receta
4. El servidor externo regresa la REF
5. El programa de farmacia verifica la validez con el estándar REF
6. El programa de farmacia solicita al servicio de validación el estátus de la receta.
7. La entidad regresa los datos de la receta, o un error si ese médico no existe o la receta no es válida.
8. El programa de farmacia se encarga del despacho y avisar al servicio de validación acerca del surtido de la receta.
9. El servicio de validación despacha un webhook al endpoint contenido en el cuerpo de la REF(si es que se contiene).

## Verificación de veracidad de recetas sin relación de confianza

Las recetas emitidas con el estándar de FIDE no requieren una relación de confianza entre las farmacias y el emisor de recetas. Cada receta electrónica contiene dentro de sí misma los métodos para verificar su veracidad.

Los pasos para validar una receta con el estándar FIDE-0.2 son los siguientes:

1. Extraer información del payload de la REF. Esto se logra decodificando el payload (la sección de la REF comprendida entre dos puntos (.)) mediante el estándar [Base64URL](https://base64.guru/standards/base64url) y después interpretándolo como una cadena en formato JSON.
2. Obtener el número de serie del certificado digital del médico (el campo `requester.certSerial`).
3. Solicitar al API del SAT el certificado del médico (en la url `https://apisnet.col.gob.mx/wsSignGob/apiV1/Obtener/Certificado?serial=<elnumeroserialdelmedico>`).
4. Verificar que los datos del certificado coincidan con los datos del médico (su cédula profesional se encuentra en el campo `requester.qualification[n].identifier`).
5. Validar e la REF mediante el estándar o los validadores adecuados y el algoritmo RS256.
6. Validar que el campo `env` de la receta sea igual a "dist".

Una vez que estos pasos están completados puedes proceder a verificar el estátus de la receta. Si la receta no contiene medicamentos controlados puedes solamente utilizar el paso 1 y surtir la receta.


## Verificación de Estatus de Receta mediante *Servicios de Validación*
Para llevar a cabo una solicitud de estatus al servicio de validación, se requiere el IURD correspondiente, así como el sello hash sha256 del contenido total de la REF, ambos separados por un guión.

Si el IURD y el hash en la solicitud son válidos, el servicio de validación emite una respuesta con los siguientes datos:

|Campo|Descripción|
|--|--|
|fecha| Timestamp en que se emite la respuesta|
|iure|Identificador Único de la Receta Electrónica|
|estatus|Sin Surtir, Surtido Parcial, Surtido Completo, No Vigente|
|tratamiento\[\]|Listado de tratamientos en la receta|
|tratamiento\[uid\].cantidad|Cantidad pendiente de entregar del medicamento|
|tratamiento\[uid\].unidad|Unidad referente a la presentación del medicamento|

Observaciones sobre seguridad y privacidad:
* El servicio de validación no expone datos sensibles relativos al médico o al paciente,
* La única manera de saber qué medicamentos se han surtido de la receta es tener la receta físicamente para poder correr el algoritmo sha-256 sobre ella y averiguar su llave.

### FIDE-FREQUENCY-1 Frecuencias de tratamiento
Las frecuencias de tratamiento se indican con la siguiente estructura:

```
A[B]xC[xD]
```
Donde 

* A es la cantidad de unidades del medicamento que hay que tomarse (las unidades de manejan según FIDE-FOR-1).
* B es un modificador de A, el cual indica la unidad de medida que se requiere tomar del medicamento, según FIDE-MED-1.
* C es la cantidad de horas que hay que dejar pasar entre cada dosis.
* D es la cantidad de días que se tiene que mantener el tratamiento. 
* Los signos `[]` indican que lo que se encuentra entre ellos es opcional.
* Los signos `x` son literales y no son modificables, se utilizan como separación.

Si se necesitan indicaciones más complejas que lo que este algoritmo permite se puede utilizar el parámetro trt[].ind y describir el tratamiento textualmente.


### FIDE-FORM-1 Diccionario de formas farmacéuticas

Este diccionario se utiliza en cualquier lugar en el que se requiera definir una forma farmacéutica de un medicamento:

|Valor|Explicación|
|--|--|
|aer|Aerosol|
|cap|Cápsulas|
|clp|Cápsulas de liberación prolongada|
|com|Comprimidos|
|crm|Crema|
|gel|Gel|
|jar|Jarabe|
|ovu|Óvulo|
|par|Parches|
|pst|Pasta|
|plv|Polvo|
|pmd|Pomada|
|shm|Shampoo|
|sld|Sólido|
|sol|Solución|
|sin|Solución inyectable|
|spr|Spray|
|spn|Suspensión|
|spt|Supositorio|
|tab|Tabletas|
|tlp|Tabletas de liberación prolongada|
|tds|Tabletas dispersables|
|tef|Tabletas efervescentes|
|ung|Ungüento|
|otr|Otro|

### Diccionario de medidas
Las principales unidades son aquellas usadas para medir peso, volumen y cantidad de una sustancia.
* Peso: expresado en Kg, g, mg, mcg
* Volumen: expresado en litros, ml=cc
* Cantidad de una Sustancia: expresado en Moles (Mol, milimoles)
* La concentración de un fármaco se expresa usualmente en miligramos (mg)
La siguiente tabla muestra las medidas utilizables en el estándar. Todos los siguientes valores son sensibles a capitalización:

|Nombre|Valor|
|-|-|
|Litros|L|
|Mililitros|mL|
|Milimol|Mmol|
|Miliequivalente|mEq|
|Kilogramos|Kg|
|Gramos|G|
|Miligramos|Mg|
|Microgramos|Mcg|
|Horas|hrs|
|Unidades internacionales (unidad de medida para insulinas, vitaminas y penicilinas)|UI|
|Libras|Lb|
|Onzas|Oz|
|Galones|Gal|
|Dosis|dos|
|Nebulizaciones|nbl|

### Transformaciones de Unidad / Cantidad
Para entregar la información más completa, el sistema de validación, realizará una transformación basada en la duración del tratamiento, para calcular la cantidad total de unidades que se deben surtir (tomando como base la forma farmacéutica). 

Por ejemplo:
```
1x8x15 cápsulas 
= Una cápsula cada ocho horas por quince días 
= 3 cápsulas diarias por quince días 
= 45 cápsulas.
```
```
2cucharaditax8x5 
= Dos cucharaditas (5 mL) cada ocho horas por 5 días
= 10 mL cada ocho horas por 5 días
= 30 mL por 5 días
= 150 mL
```

## Notificación de Despacho
Cada entidad farmacológica que realice el despacho de medicamentos realizará una notificación al servicio de validación empleando el servicio de notificación de despapacho. De ésta manera, los servicios de validación podrán llevar el seguimiento de los medicamentos surtidos y notificar a su vez a los interesados. 

### Objeto de Despacho

|Campo|Descripción|
|-|-|
|fecha|Timestamp del momento en que se efectúa el surtido de la receta|
|iure|Identificador Único de la Receta electrónical a la que hace referencia|
|dispenseType|Completo o Parcial|
|performer|Objeto Surtidor con información de quien realiza el despacho|
|dispenseRequest\[\]| Arreglo de objetos con información sobre los elementos que se despachan| 

### Objeto Surtidor

|Campo|Descripción|
|-|-|
|performer|Información de quién está realizado el despacho de la receta|
|performer.identifier|Identificador de la entidad que realiza el despacho del medicamento|
|performerType|Tipo de surtidor (Farmacia Física, Farmacia Digital, etc)|
|performer.licence|Número de licencia sanitaria del establecimiento|
|performer.type|Tipo de establecimiento relativo a la licencia sanitaria|
|performer.rfc|Registro Federal de Causantes asociado a la entidad que realiza el despacho de la receta|
|performer.responsable|Datos completos del responsable del despacho|
|performer.responsable.id|Identificador en la plataforma del responsable|
|performer.responsable.name|Nombre del responsable que realiza el despacho|
|performer.address|Objeto Dirección del responsable|

### Objeto de Surtido

|Campo|Descripción|
|-|-|
|dispenseRequest[treatment.uid].quantity|Número de ítems que se despachan |
|dispenseRequest[treatment.uid].form|Forma en que se entrega el medicamento (caja, envase, ampolleta, etc)|
|dispenseRequest[treatment.uid].unit|Unidad de medicamento en el envase (ej: ml, pastillas, etc)|
|dispenseRequest[treatment.uid].content|Cantidad de unidades del medicamento en el envase|
|dispenseRequest[treatment.uid].substitution|Información relativa a la sustitución|

### Objeto de Sustitución

|Campo|Descripción|
|-|-|
|substitution|Información completa del medicamento que realiza la sustitución durante el despacho|
|substitution.reason|Razón por la que se realiza la sustitución|
|substitution.concept|Tipo de sustitución: Equivalente, ComposiciónEquivalente, ComposiciónMarca, ComposiciónGenerica, TerapeuticaAlternativa, TerapeuticaMarca, TerapeuticaGenerica, Formulario, Ninguna|
|substitution.medication|Objeto medicamento que sustituye al tratamiento original|

## Notificación de Evento de Seguimiento
De la misma manera, las entidades de seguimiento pueden alimentar a los servicios de validación con eventos de seguimiento sobre los medicamentos para mantener informados a los interesados

|Campo|Descripción|
|-|-|
|date|Timestamp del momento en que se registró el evento
|iure|Identificador Único de la Receta Electrónica|
|identifier|Identificador del evento|
|eventType|Categoría del evento que se está registrando, por ejemplo, “TomaDeMedicamento” o “ReaccionAdversa”|
|requesterId|Identificador de la plataforma que registra el evento|
|detail|Detalle del evento|
|detectedIssue|Arreglo de Objetos de incidencia que se reportan en el evento (si existen)|

# Objeto de Incidencia

|Campo|Descripción|
|-|-|
|detectedIssue.implicated[]|Arreglo de Objetos de Medicamentos que se relacionan con el evento|
|detectedIssue.severity|Alto, Moderado, Bajo|
|detectedIssue.evidence|Documentación referente a la incidencia|
|detectedIssue.mitigation|Acciones realizadas para resolver el problema|
|detectedIssue.mitigation[].action|Acción realizada|
|detectedIssue.mitigation[].date|Fecha en que se realiza la acción|
|detectedIssue.mitigation[].author|Responsable de la acción realizada|


