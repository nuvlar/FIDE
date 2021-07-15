# JWT

JWT (JSON Web Token) es un estándar qué está dentro del documento RFC 7519.

En el mismo se define un mecanismo para poder propagar entre dos partes, y de forma segura, la identidad de un determinado usuario, además con una serie de claims o privilegios.

Estos privilegios están codificados en objetos de tipo JSON, que se incrustan dentro de del payload o cuerpo de un mensaje que va firmado digitalmente.

# Estándar de recetas electrónicas FIDE JSON

El JSON Web Token resultante de la firma de un payload de receta bajo la estructura especificada en la siguiente sección corresponde a la receta bajo el estandard FIDE.

Las recetas con el estándar FIDE están diseñadas para poder ser contenidas en una URL de navegador web. Ya que el estándar JWT es compatible con el estándar URL, no hay necesidad de codificar antes de insertar una receta en una dirección web. La dirección URL puede ser almacenada en un un código QR para su rápida captura.

## Ejemplo de una receta electrónica con el estándar FIDE-0.2

### Cadena JWT:

```
eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJ2ZXJzaW9uIjoiRklERS0wLjIiLCJqdGkiOiIxLTg1Ny0xNjE5NTQ1NjU0IiwiaXNzIjoiTVJEIiwiaWF0IjoxNjE5NTQ1NjU0LCJkdGMiOjE2MTk1NDU2NTQsImVudmlyb25tZW50IjoiZGlzdCIsInJlcXVlc3RlciI6eyJpZGVudGlmaWVyIjoxLCJjZXJ0U2VyaWFsIjoiaHR0cHM6XC9cL2FwaXNuZXQuY29sLmdvYi5teFwvd3NTaWduR29iXC9hcGlWMVwvT2J0ZW5lclwvQ2VydGlmaWNhZG8_c2VyaWFsPTMwMzAzMDMwMzEzMDMwMzAzMDMwMzAzNTMwMzYzNjM4MzYzMTM3MzUiLCJuYW1lIjoiTHVpcyBFc3Bpbm9zYSBTaWVycmEiLCJxdWFsaWZpY2F0aW9uIjpbeyJuYW1lIjoiTmV1cm9sb2dcdTAwZWRhIiwiaWRlbnRpZmllciI6IjI0NjA3MjkiLCJpc3N1ZXIiOiJGYWN1bHRhZCBkZSBNZWRpY2luYSBVQVNMUCJ9XSwiYWRkcmVzcyI6eyJsaW5lIjoiQmF0YWxsXHUwMGYzbiBkZSBTYW4gUGF0cmljaW8gMTEyLCBTYW4gUGVkcm8gR2FyemEgR2FyY1x1MDBlZGEsIE4uIEwuIn0sInRlbGVwaG9uZSI6Iis1MjgxODg4ODA2NzUifSwic3ViamVjdCI6eyJpZGVudGlmaWVyIjo4NTcsIm5hbWUiOiJMdWlzIFBydWViYSBQcnVlYmEgTVJEIiwid2VpZ2h0Ijo3OCwiaGVpZ2h0IjoxNjcsImJpcnRoRGF0ZSI6IjE5NjYtMDUtMjIifSwibWVkaWNhdGlvbiI6W3siaWRlbnRpZmllciI6MTE0MywibmFtZSI6IkNJUFJPIFhSIDUwME1HIFRBQiBDXC83Iiwic3Vic3RhbmNlIjoiQ0lQUk9GTE9YQUNJTk8iLCJkb3NhZ2VJbnN0cnVjdGlvbiI6eyJ0ZXh0IjoiUHJ1ZWJhIEJ1c2NhbWVkIn0sImZyYWN0aW9uIjo0fV0sImNlcnRpZmljYXRlVVJMIjoiaHR0cHM6XC9cL3d3dy5taXJlY2V0YWRpZ2l0YWwuY29tXC9jZXJ0c1wvY2VydDEifQ.hpIi2MV_iNjFYtr4bRgR2F4LMAscNQXxCySfmpW_--uEwBqcpEggdsXrOuF3SU1m1n9rXB7Kg47_m8m6Pjg919gBTo_6A0JLcOocwCbP4-zk_rHXHpZkQ56_U-gMezptw92mguSn81F9TMG8Q-bMsjZeUCBBR05OE35qVi8veq97T-GM88SnXun1lOp29cudTWOHw4RPd7MeaQ_FcKMcJ2cIYdM18IdJmvG9sbJyw7YI2q5KphbT0d_BQzIDS4PYutbwS11WYX7SBXmBICZadv5XgW7cNOMcaT2TZiIVg7bJaDoxFqTRHwtyZi0FNJyT48j-p_cn6CRsH5_fMKYA6A

### Certificado público para validar firma:

https://apisnet.col.gob.mx/wsSignGob/apiV1/Obtener/Certificado?serial=3030303031303030303030353036363836313735

### Payload decodificado:

```json
{
  "version": "FIDE-0.2",
  "jti": "1-857-1619545654",
  "iss": "MRD",
  "iat": 1619545654,
  "dtc": 1619545654,
  "environment": "dist",
  "requester": {
    "identifier": 1,
    "certSerial": "https://apisnet.col.gob.mx/wsSignGob/apiV1/Obtener/Certificado?serial=3030303031303030303030353036363836313735",
    "name": "Luis Espinosa Sierra",
    "qualification": [
      {
        "name": "Neurología",
        "identifier": "2460729",
        "issuer": "Facultad de Medicina UASLP"
      }
    ],
    "address": {
      "line": "Batallón de San Patricio 112, San Pedro Garza García, N. L."
    },
    "telephone": "+528188880675"
  },
  "subject": {
    "identifier": 857,
    "name": "Luis Prueba Prueba MRD",
    "weight": 78,
    "height": 167,
    "birthDate": "1966-05-22"
  },
  "medication": [
    {
      "identifier": 1143,
      "name": "CIPRO XR 500MG TAB C/7",
      "substance": "CIPROFLOXACINO",
      "dosageInstruction": {
        "text": "Prueba Buscamed"
      },
      "fraction": 4
    }
  ],
  "certificateURL": "https://www.mirecetadigital.com/certs/cert1"
}
```

### QR de la receta:

QR FIDE

![Receta QR](fide_qr.png?raw=true "fide:https://mirecetadigital.com/api/v1/receta.php?iure=1-857-1619545654&sd=70225b95014a30ab8b582906c3dba970349d6f53a346a5db39ae4011df862642")

QR FIDE base32

![Receta QR](fide_qr_b32.png?raw=true "MZUWIZJ2NB2HI4DTHIXS63LJOJSWGZLUMFSGSZ3JORQWYLTDN5WS6YLQNEXXMMJPOJSWGZLUMEXHA2DQH5UXK4TFHUYS2OBVG4WTCNRRHE2TINJWGU2CM43EHU3TAMRSGVRDSNJQGE2GCMZQMFRDQYRVHAZDSMBWMMZWIYTBHE3TAMZUHFSDMZRVGNQTGNBWME2WIYRTHFQWKNBQGEYWIZRYGYZDMNBS")


