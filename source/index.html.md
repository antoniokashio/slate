---
title: KashIO Payments 1.0.1.a

language_tabs:
  - curl  
  
toc_footers:
  - <a href='soporte@kashio.net'>Soporte KashIO</a>
  
includes:
  - errors

search: true
---

# Introducción

¡Bienvenido a la API de KashIO! Puede utilizar nuestra API para acceder a los endpoints de la API de KashIO Payments, donde pueden obtener información sobre cómo crear Ordenes de Pago, recibir notificaciones sobre Eventos, etc.

El API de KashIO está basado en REST. Nuestra API tiene URLs previsibles y orientadas a recursos, y usa códigos de respuesta HTTP para indicar errores API. 

Soportamos la autenticación HTTP y los verbos HTTP estándares como GET, POST, PATCH y DELETE; soportamos CORS (intercambio de recursos de origen cruzado), permitiéndole interactuar de forma segura con nuestra API desde una aplicación web de cliente.

Por simplicidad NO usamos dos APIs separadas para Integración y Producción; las cuentas tienen el modo de prueba y con solo usar la llave correspondiente, podrá migrar de ambientes de forma muy sencilla.

# Autenticación

> Para obtener acceso use este código:

```curl
   curl https://api.kashio.net/v1/payments \
  -u your_private_api_key:
```
> Curl utiliza el indicador -u para pasar las credenciales de autenticación básicas (la adición de dos puntos después de que su clave de API impide que cURL solicite una contraseña).

> No olvide reemplazar `your_private_api_key` con su Clave Secreta API.

Las peticiones a KashIO son autenticada mediante el uso la **Clave Secreta API** en la solicitud. Puede administrar las claves de su API en el Panel de control de su consola web KCMS (KashIO Customer Management System). No comparta sus Claves Secretas API en áreas de acceso público tales como GitHub, código de cliente, etc.
La autenticación en la API se realiza a través de Autenticación básica HTTP. Proporcione su clave de API como valor de usuario de autenticación básico. No es necesario proporcionar una contraseña.

Todas las solicitudes de API deben hacerse a través de HTTPS. Las peticiones realizadas en HTTP normal fallarán. Las solicitudes de API sin autenticación también fallarán.


`Authorization: basic your_private_api_key`

<aside class="notice">
Reemplace <code>your_private_api_key</code> con su **Clave Secreta API**.
</aside>


# Paginación

En KashIO todos los recursos de API tienen soporte para listados, y ellos es accesible mediante cursores. Estos métodos de API de lista comparten una estructura común, soportando estos tres parámetros: limit, starting_after y ending_before.
KashIO utiliza la paginación basada en el cursor a través de los parámetros starting_after y ending_before. Ambos toman un valor del **id** de objeto existente y devuelven objetos en un arreglo. 

# Orden de Pago (Invoice)
La Orden de Pago es la instrucción enviada a KashIO para iniciar el proceso de pago de un cliente.  

## Objeto Orden de Pago

Parámetro | Tipo | Descripción |
--------- | --------- | ----------- |
id | String | El ID de la Orden de Pago
object | String | Tipo de objeto: **invoice**
livemode | Boolean | Si es producción **true** o pruebas **false**
customer | Object | Información de Cliente (phone, email, document_type, document_id) (Telefono en formato E.164)
created | String | Fecha de creación (ISO-8601:yyyy-MM-ddThh:mm:ss)
request_datetime | String | Fecha del Sistema de comercios (evitar DDOS)
currency | String | Moneda de la Orden de Pago (ISO-4217)
amount | Decimal | Monto de la Orden de Pago. Usar 2 decimales
invoice_id | String | Número de referencia de la factura en el sistema del comercio
expiration_datetime | String | Expiración de la Orden de Pago (ISO-8601:yyyy-MM-ddThh:mm:ss)
sub_merchant | Object | Información de SubMerchant (nombre, correo electrónico, logotipo, etc.)
url_success | String | URL donde el comprador será redirigido si el pago se realizó con éxito (ej: http://www.coffee.com/api/success.html)
url_error | String | URL donde retorna el usuario en caso no realiza el pago(ej: http://www.coffee.com/api/error.html)
token | String | Token usado para uso en Gateway (pasarela)
ktin | String | Número utilizado para pagar manualmente 
metadata | Object | Detalles de la Orden de Pago (formato JSON)
status | string | Estado de la Orden de Pago (pending, paid, expired, voided)
error | Error [] | Lista de errores para errores HTTP distintos de 200


## Crear una Orden de Pago
   
> Ejemplo de Petición

```curl 
  curl -X POST https://api.kashio.net/v1/payments/invoices \
  -u sk_test_f5yj6jAxxxHep36jAxxxep3xxxJUx: \
  -h 'content-type: application/json' \
  -d '{
	  "customer": {
	    "name": "JUAN PEREZ",
	    "email": "juan.perez.xxx@gmail.com",
	    "phone": "+13055606666",
	    "document_id": "10203040",
	    "external_id": "CUS-10203040-001",
	    "document_type": "DNI"
	  },
	  "metadata": {
	    "order_id": "000001-001",
	    "order_name": "Deuda Enero 2019",
	    "order_description": "Deuda Enero 2019"
	   },
	  "invoice_id": "INV-000001-001",
	  "request_datetime": "2020-01-01T05:00:00",
	  "due_datetime": "2020-01-02T04:59:59",
	  "expiration_datetime": "2020-02-01T05:00:00",
	  "amount": 15.00,
	  "currency": "PEN"
	}'
```

> Ejemplo de Respuesta

```json
   {
  "amount": 15.0,
  "created": "2020-01-01T23:33:36",
  "currency": "PEN",
  "customer": {
    "document_id": "10203040",
    "document_type": "DNI",
    "email": "juan.perez.xxx@gmail.com",
    "external_id": "CUS-10203040-001",
    "name": "JUAN PEREZ",
    "phone": "+13055606666"
  },
  "due_datetime": "2020-01-02T04:59:59",
  "expiration_datetime": "2020-02-01T05:00:00",
  "fees": 0.0,
  "id": "inv_live_oFPMsJmgmxBhPiWAoWFZ3n",
  "invoice_id": "INV-000001-001",
  "ktin": "201590965569",
  "late_fee": 0.0,
  "livemode": false,
  "metadata": {
    "order_description": "Deuda Enero 2019",
    "order_id": "000001-001",
    "order_name": "Deuda Enero 2019"
  },
  "object": "invoice",
  "status": "NEW",
  "sub_total": 15.0,
  "token": "E9e866w3BZRpmMEu5bgLCi"
}
```

Crear una Orden de Pago es el primer paso para recibir un pago a través de KashIO


### Petición HTTP 

`POST https://api.kashio.net/v1/payments/invoices`

### Parámetros 

Parámetro | Tipo | Obligatorio |
--------- | --------- | ----------- |
customer | Object | si
request_datetime | String | si
currency | String | si
amount | Decimal | si
invoice_id | String | si
expiration_datetime | String | no
url_success | String | no
url_error | String | no
metadata | Object | no


## Consultar una Orden de Pago

> Ejemplo de Petición

```curl 
  curl -X GET https://api.kashio.net/v1/payments/invoices/inv_live_ngMCEhfsgVUUsePqZXpcpB \
  -u sk_test_f5yj6jAHep36jAep3JU: 
```

> Ejemplo de Respuesta

```json
    {
  "amount": 15.0,
  "created": "2020-01-01T23:33:36",
  "currency": "PEN",
  "customer": {
    "document_id": "10203040",
    "document_type": "DNI",
    "email": "juan.perez.xxx@gmail.com",
    "external_id": "CUS-10203040-001",
    "name": "JUAN PEREZ",
    "phone": "+13055606666"
  },
  "due_datetime": "2020-01-02T04:59:59",
  "expiration_datetime": "2020-02-01T05:00:00",
  "fees": 0.0,
  "id": "inv_live_oFPMsJmgmxBhPiWAoWFZ3n",
  "invoice_id": "INV-000001-001",
  "ktin": "201590965569",
  "late_fee": 0.0,
  "livemode": false,
  "metadata": {
    "order_description": "Deuda Enero 2019",
    "order_id": "000001-001",
    "order_name": "Deuda Enero 2019"
  },
  "object": "invoice",
  "status": "NEW",
  "sub_total": 15.0,
  "token": "E9e866w3BZRpmMEu5bgLCi"
}
```

Este endpoint recibe una Orden de Pago especifica.


### Solicitud HTTP 

`GET https://api.kashio.net/v1/payments/invoices/{id}`

### Parametros 

Parámetro | Tipo | Obligatorio |
--------- | --------- | ----------- |
id | String | si


## Anular una Orden de Pago
   
> Ejemplo de Petición

```curl 
  curl -X DELETE https://api.kashio.net/v1/payments/invoices/inv_6hxCf5yj6jAHep3JUJZrmm \
  -u sk_test_f5yj6jAHep36jAep3JU: \
  -h 'content-type: application/json' 
```

> Ejemplo de Respuesta

```json
    {
        "id": "inv_6hxCf5yj6jAHep3JUJZrmm",
        "object": "invoice", 
        "livemode": false,
        "created": "2017-01-31T14:24:59",   
        "customer": { "phone" :"+519996666", "email": "maria.viera22@gmail.com" },
        "currency": "PEN",
        "amount": "25.00",
        "invoice_id": "ABCD1234567890",
        "request_datetime": "2017-01-31T14:24:59",
        "expiration_datetime": "2017-02-01T14:24:59",
        "qr_code": "6hxCf5yj6jAHep3JUJZrmm",
        "ktin": "1234567890",
        "status": "VOIDED"
    }
```

Anula una Orden de Pago. La Orden de Pago no podrá ser paga después de ser anulada.


### Petición HTTP 

`DELETE https://api.kashio.net/v1/payments/invoices/{id}`

### Parámetros 

Parámetro | Tipo | Obligatorio |
--------- | --------- | ----------- |
id | String | si


## Actualizar una Orden de Pago
   
> Ejemplo de Petición

```curl 
  curl -X PATCH https://api.kashio.net/v1/payments/invoices/inv_6hxCf5yj6jAHep3 \
  -u sk_test_f5yj6jAHep36jAep3JU: \
  -h 'content-type: application/json' \
  -d '{ "amount": "25.00" }'
```

> Ejemplo de Respuesta

```json
	{
	  "amount": 23.32,
	  "created": "2020-06-07T23:46:11",
	  "currency": "PEN",
	  "customer": {
	    "document_id": "10203040",
	    "document_type": "DNI",
	    "email": "juan.perez.xxx@gmail.com",
	    "name": "JUAN PEREZ",
	    "phone": "+13055606666"
	  },
	  "due_datetime": "2020-01-02T04:59:59",
	  "expiration_datetime": "2020-02-01T05:00:00",
	  "fees": 0.0,
	  "id": "inv_cert_5TUVNBFGQHtQrPvXvZPaA9",
	  "invoice_id": "INV-000001-001",
	  "ktin": "201590965587",
	  "late_fee": 0.0,
	  "livemode": false,
	  "metadata": {
	    "order_description": "Deuda Enero 2019",
	    "order_id": "000001-001",
	    "order_name": "Deuda Enero 2019"
	  },
	  "object": "invoice",
	  "status": "NEW",
	  "sub_total": 23.32,
	  "token": "WdGY6TPJAx4ceHG4CnkrTH"
	}
```

Actualiza ciertos parámetros de una Orden de Pago. No todos los parámetros pueden ser actualizados.


### Petición HTTP 

`PATCH https://api.kashio.net/v1/payments/invoices/{id}`

### Parámetros 

Parámetro | Tipo | Actualizable |
--------- | --------- | ----------- |
customer | Object | no
request_datetime | String | no
currency | String | si
amount | Decimal | si
invoice_id | String | no
expiration_datetime | String | si
sub_merchant | Object | no
url_success | String | no
url_error | String | no
size_qr | String | no
metadata | Object | no


## Consultar Lista de Ordenes de Pago

> Ejemplo de Petición

```curl 
  curl -X GET https://api.kashio.net/v1/payments/invoices?date=2017-05-30 \
  -u sk_test_f5yj6jAHep36jAep3JU: 
```

> Ejemplo de Respuesta

```json
{
  "data": [
    {
      "amount": 23.32,
      "created": "2020-06-07 23:46:11",
      "currency": "PEN",
      "customer": {},
      "id": "inv_cert_5TUVNBFGQHtQrPvXvZPaA9",
      "invoice_id": "INV-000001-001",
      "ktin": "59965587",
      "late_fee": 0.0,
      "livemode": false,
      "metadata": {
        "order_description": "Deuda Enero 2019",
        "order_id": "000001-001",
        "order_name": "Deuda Enero 2019"
      },
      "object": "invoice",
      "request_datetime": "2020-06-07 23:46:11",
      "status": "new",
      "sub_total": 23.32,
      "token": "WdGY6TPJAx4ceHG4CnkrTH"
    }
  ],
  "has_more": false,
  "object": "list"
}

```


### Solicitud HTTP 

`GET https://api.kashio.net/v1/payments/invoices?date={date}`

### Parámetros 

Parámetro | Tipo | Descripción |
--------- | --------- | ----------- |
date | String | Fecha para filtrar Ordenes de Pago
status | String | Status para filtro de Ordenes de Pago
limit | Number | Limite de registros por petición 
start | String | cursor de inicio
end | String | cursor de fin 


# Eventos
Los eventos son creados ante alguna ocurrencia en las operaciones de KashIO; un evento puede ser una creación, cambio, anulación de una entidad, o una operación como el pago de una Orden de Pago, la emisión de una transferencia, etc.

## Objeto Evento

Parámetro | Tipo | Descripción |
--------- | --------- | ----------- |
id | String | El ID de la Orden de Pago
object | String | Tipo de objeto: **event**
livemode | Boolean | Si es producción **true** o pruebas **false**
created | String | Fecha de creación (ISO-8601:yyyy-MM-ddThh:mm:ss)
type | String | Tipo de evento (ej: invoice.paid, invoice.expired)
status | String | Status del evento (new, failed, notified)	
data | Object | Object related to the event (HATEOAS, ej : //invoices/inv_abcd1234)

## Consultar un Evento

> Ejemplo de Petición

```curl 
  curl -X GET https://api.kashio.net/v1/payments/event/eve_6hxCf5yj6jAHep3 \
  -u sk_test_f5yj6jAHep36jAep3JU: 
```

> Ejemplo de Respuesta

```json
    {
        "id": "eve_6hxCf5yj6jAHep3JUJZrmm",
        "object": "event", 
        "livemode": false,
        "created": "2017-01-31T14:24:59",   
        "type": "invoice.paid",   
        "data":  { "url"  : "//invoices/inv_QokikJOvBiI2HlWgH4olfQ2",
             	   "invoice" : {
		              "id": "inv_QokikJOvBiI2HlWgH4olfQ2",
	  		      "object": "invoice",
			      …
                               }
          	 },        
        "status": "new"
    }
```

Este endpoint recibe un Evento especifico.


### Solicitud HTTP 

`GET https://api.kashio.net/v1/payments/events/{id}`

### Parametros 

Parámetro | Tipo | Obligatorio |
--------- | --------- | ----------- |
id | String | si


## Consultar Lista de Eventos

> Ejemplo de Petición

```curl 
  curl -X GET https://api.kashio.net/v1/payments/events?date=2017-05-30 \
  -u sk_test_f5yj6jAHep36jAep3JU: 
```

> Ejemplo de Respuesta

```json
{
  "object": "list", 
  "has_more": false,
  "data": [
    {
        "id": "eve_6hxCf5yj6jAHep3JUJZrmm",
        "object": "event", 
        "livemode": false,
        "created": "2017-01-31T14:24:59",   
        "type": "invoice.paid",   
        "data":  { "url"  : "//invoices/inv_QokikJOvBiI2HlWgH4olfQ2",
             	   "invoice" : {
		              "id": "inv_QokikJOvBiI2HlWgH4olfQ2",
	  		      "object": "invoice",
			      …
                               }
          	 },       
        "status": "new"    
    },
    {...},
    {...}
    ]
 }
```


### Solicitud HTTP 

`GET https://api.kashio.net/v1/payments/events?date={date}`

### Parámetros 

Parámetro | Tipo | Descripción |
--------- | --------- | ----------- |
date | String | Fecha para filtrar eventos
status | String | Status para filtro de eventos
limit | Number | Limite de registros por petición 
start | String | cursor de inicio
end | String | cursor de fin 








