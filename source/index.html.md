---
title: KashIO Payments 1.0.0.a

language_tabs:
  - shell
  - javascript
  
toc_footers:
  - <a href='soporte@kashio.net'>Soporte KashIO</a>
  
includes:
  - errors

search: true
---

# Introducción

¡Bienvenido a la API de KashIO! Puede utilizar nuestra API para acceder a los Endpoints de la API de KashIO Payments, donde pueden obtener información sobre cómo crear Ordenes de Pago, recibir notificaciones sobre Eventos, etc.

El API de KashIO está basado en REST. Nuestra API tiene URLs previsibles y orientadas a recursos, y usa códigos de respuesta HTTP para indicar errores API. 
Soportamos la autenticación HTTP y los verbos HTTP estándares como GET, POST, PATCH y DELETE; soportamos CORS (intercambio de recursos de origen cruzado), permitiéndole interactuar de forma segura con nuestra API desde una aplicación web de cliente.

Por simplicidad NO usamos dos APIs separadas para Integración y Producción; las cuentas tienen el modo de prueba y con solo usar la llave correspondiente, podrá migrar de ambientes de forma muy sencilla.

# Autenticación

> Para obtener acceso use este código:

```curl
# Con CURL solo necesita pasar el parametro correcto en la cabecera de cada solicitud
curl https://api.kashio.net/v1/payments \
  -u your_private_api_key:
```
> Curl utiliza el indicador -u para pasar las credenciales de autenticación básicas (la adición de dos puntos después de que su clave de API impide que cURL solicite una contraseña).

```javascript
const KashIO = require('KashIO');

let api = KashIO.authorize('your_private_api_key');
```

> No olvice reemplazar `your_private_api_key` con su Clave Secreta API.

Las llamadas a KashIO son autenticada mediante el uso la **Clave Secreta API** en la solicitud. Puede administrar las claves de su API en el Panel de control de su consola web KCMS (KashIO Customer Management System). No comparta sus Claves Secretas API en áreas de acceso público tales como GitHub, código de cliente, etc.
La autenticación en la API se realiza a través de Autenticación básica HTTP. Proporcione su clave de API como valor de usuario de autenticación básico. No es necesario proporcionar una contraseña.

Todas las solicitudes de API deben hacerse a través de HTTPS. Las llamadas realizadas en HTTP normal fallarán. Las solicitudes de API sin autenticación también fallarán.


`Authorization: basic your_private_api_key`

<aside class="notice">
Reemplace <code>your_private_api_key</code> con su **Clave Secreta API**.
</aside>


# Orden de Pago (Invoice)
La Orden de Pago es la instrucción enviada a KashIO para iniciar el proceso de pago de un cliente.   

## Crear una Orden de Pago
```shell 
  curl --request POST \
  --url 'http://api.kashio.net/v1/payments/invoices' \
  --user your_private_api_key: \
  --header 'content-type: application/json' \
  --data '{         
            "invoice": 
            {
            "payer_phone": "+519996666",
            "currency": "PEN",
            "amount": "20.00",
            "invoice_id": "inv_1234567890"
	        }
         }'
```


> El comando anterior recibe una respuesta JSON:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

Este endpoint recibe la Orden de Pago creada.


### Solicitud HTTP 

`POST https://api.kashio.net/v1/payments/invoices`

### Parametros 

Parámetro | Requerido | Descripción | 
--------- | --------- | ----------- | 
id | NO | El ID de la Orden de Pago a consultar
ID | No | El ID de la Orden de Pago a consultar
ID | No | El ID de la Orden de Pago a consultar
ID | No | El ID de la Orden de Pago a consultar
ID | No | El ID de la Orden de Pago a consultar
ID | No | El ID de la Orden de Pago a consultar
ID | No | El ID de la Orden de Pago a consultar
ID | No | El ID de la Orden de Pago a consultar





## Consultar una Orden de Pago

```shell
curl "https://api.kashio.net/v1/payments/invoices/<id>"
  -u your_private_api_key:
```

> El comando anterior recibe una respuesta JSON:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

Este endpoint recibe una Orden de Pago especifica.


### Solicitud HTTP 

`GET https://api.kashio.net/v1/payments/invoices/<id>`

### Parametros URL 

Parámetro | Requerido | Descripción | 
--------- | --------- | ----------- | 
id | SI | El ID de la Orden de Pago a consultar




## Actualizar una Orden de Pago

```shell
curl "https://api.kashio.net/v1/payments/invoices/<id>"
  -u your_private_api_key:
```

> El comando anterior recibe una respuesta JSON:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

Este endpoint recibe la Orden de Pago creada.


### Solicitud HTTP 

`POST https://api.kashio.net/v1/payments/invoices`

### Parametros 

Parámetro | Requerido | Descripción | 
--------- | --------- | ----------- | 
id | NO | El ID de la Orden de Pago a consultar
ID | No | El ID de la Orden de Pago a consultar
ID | No | El ID de la Orden de Pago a consultar
ID | No | El ID de la Orden de Pago a consultar
ID | No | El ID de la Orden de Pago a consultar
ID | No | El ID de la Orden de Pago a consultar
ID | No | El ID de la Orden de Pago a consultar
ID | No | El ID de la Orden de Pago a consultar





## Consultar Lista de Ordenes de Pago 

```shell
curl "https://api.kashio.net/v1/payments/invoices/list"
  -u your_private_api_key:
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

Este endpoint devuelve una lista de facturas.

### HTTP Request

`GET https://api.kashio.net/v1/payments/invoices/list/`

### Query Parameters

Parámetro | Requerido | Descripción | 
--------- | --------- | ----------- | 
date | NO | Fecha en formato ISO-8601 




