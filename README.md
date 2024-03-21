# Ver contrato OpenAPI 
https://devniel93.github.io/prueba-config-manual/

# Descripción general

Permite registrar la operación inicial de carga de archivos, registrar, remover y listar los errores de los metadatos de los archivos. Además, registrar y listar el seguimiento de las solicitudes de Afiliación o Cobro, a los usuarios de las empresas afiliadas al producto de Débito Automático.

# Exposición Servidores (URL BASE)
| Ambiente | URL|
| :-------- | :------- |
| Desarrollo | https://apibsdev.lima.bcp.com.pe/trade-banking/direct-debit/v1 |
| Certificación | https://apibsqa.lima.bcp.com.pe/trade-banking/direct-debit/v1 |
| Producción | https://apibs.lima.bcp.com.pe/trade-banking/direct-debit/v1 |

# Repositorio Contrato Open API

https://swaggerhub.lima.bcp.com.pe/apis/BCP_Org/business-direct-debit-v1/1.0.0

# Recursos

## Batch Debit
El recurso gestiona la información de una carga de archivo(s) de afiliacion o cobro de débito automático.

### Endpoints
Endpoints disponibles para el recurso.

| Recurso | Método HTTP     | Endpoint  |    Grant Type |
| :-------- | :------- | :------------------------- | :------------------------- |
| batch-debits | POST | POST /batch-debits |Client Credential|
| batch-debits | GET | GET /batch-debits/{batchDebitId}/files |Client Credential|
| batch-debits | GET | GET /batch-debits/{batchDebitId}/files/{fileId} |Client Credential|
| files | GET | GET /files/{fileId}/operational-errors |Client Credential|

### Modelo de Datos

| Nombre | XPath | Tipo de Dato|Array | Ejemplo | Descripción  | Codigos  |Pattern  |
| :-------- | :------- | :------------ | :---------- | :---------- | :---------- |:---------- |:---------- |
| `personId` | `Person` | `string` | No | 85423694 | Identificador del cliente | |
| `account` | `Account` | `object` | Si | | Objeto que representa la cuenta | |
| `accountId` |`Account/AccountId` |  `string` | No | 00019100109502007005 | Número de cuenta | |
| `formattedAccountNumber` |`Account/FormattedAccountNumber` |  `string` | No | 191-00109502-0-07 | Número de cuenta comercial formateado | |
| `status` |`Account/Status` |  `string` | No | Active | Estado de la cuenta | Active  Deactivated  Blocked|`^[A-Z]{3,3}$`|
| `cci` |`Account/CCI` |  `string` | No | 00225010919303912995 | Codigo de cuenta interbancario para Perú | |
| `openingDate` |`Account/OpeningDate` |  `string` | No | 1997-07-24 | Fecha de apertura de la cuenta. | |


## POST /batch-debits
Registra una carga de archivos de afiliacion o cobro de débito automático.

#### Parámetros
| Parámetro | Tipo     | Requerido    | Descripción    |
| :-------- | :------- | :--------------- | :--------------- |
| `Authorization` | `header` | Si | Token de autorización |
| `Request-ID` | `header` | Si | Este campo es un valor estandar ya existente y sera usado como identificador. |
| `request-date` | `header` | Si | Fecha de la petición |
| `app-code` | `header` | Si | Codigo de la aplicacion que invoca el servicio. Se debe usar el codigo de 2 caracteres que tienen asignada las aplicaciones, puede ser el canal. |
| `caller-name` | `header` | Si | Nombre de la API que realiza la invocacion al servicio. |
| `Ocp-Apim-Subscription-Key` | `header` | Si | Clave de suscripción de Azure API Management |
| `user-code` | `header` | No | Codigo de usuario |

#### Ejemplos de Uso
#### 1. Registar una carga de archivo de afiliación
##### Request
```json
POST /batch-debits
```
##### Response

```json
{
  "batchDebit": {
    "batchDebitTypeCode": "AFI",
    "company": {
      "companyId": 200
    },
    "files": [
      {
        "fileName": "AfiliacionSoles0808.txt",
        "fileSize": 120000
      }
    ]
  }
}
```

## GET /batch-debits/{batchDebitId}/files
Lista los archivos de una carga de afiliación o cobro de débito automático.

#### Parámetros
| Parámetro | Tipo     | Requerido    | Descripción    |
| :-------- | :------- | :--------------- | :--------------- |
| `Authorization` | `header` | Si | Token de autorización |
| `Request-ID` | `header` | Si | Este campo es un valor estandar ya existente y sera usado como identificador. |
| `request-date` | `header` | Si | Fecha de la petición |
| `app-code` | `header` | Si | Codigo de la aplicacion que invoca el servicio. Se debe usar el codigo de 2 caracteres que tienen asignada las aplicaciones, puede ser el canal. |
| `caller-name` | `header` | Si | Nombre de la API que realiza la invocacion al servicio. |
| `Ocp-Apim-Subscription-Key` | `header` | Si | Clave de suscripción de Azure API Management |
| `batchDebitId` | `Path` | Si | Código de carga que agrupa los archivos del proceso de Afiliación o Cobro de débito automático. |

#### Ejemplos de Uso
#### 1. Lista los archivos de un proceso de afiliación
##### Request
```json
GET /batch-debits/12434/files
```
##### Response

```json
{
  "debitFiles": [
    {
      "fileName": "AfiliacionSoles0808.txt",
      "fileSize": 120000,
      "fileId": 25,
      "fileStatusCode": "EST001",
      "storageAccountFileName": "9fecd7a6-9d89-4d4d-a039-c28f7181fec0.txt",
      "companyService": {
        "serviceCode": 102,
        "serviceDescription": "Telefono fijo."
      },
      "processingResult": {
        "totalRecords": 145,
        "correctRecords": 145,
        "errorsQuantity": 145
      },
      "fileAmount": {
        "totalAmount": 10000,
        "currencyCode": "PEN"
      }
    }
  ]
}
```

## GET /batch-debits/{batchDebitId}/files/{fileId}
Consulta la información de un archivo cargado de Débito Automático.

#### Parámetros
| Parámetro | Tipo     | Requerido    | Descripción    |
| :-------- | :------- | :--------------- | :--------------- |
| `Authorization` | `header` | Si | Token de autorización |
| `Request-ID` | `header` | Si | Este campo es un valor estandar ya existente y sera usado como identificador. |
| `request-date` | `header` | Si | Fecha de la petición |
| `app-code` | `header` | Si | Codigo de la aplicacion que invoca el servicio. Se debe usar el codigo de 2 caracteres que tienen asignada las aplicaciones, puede ser el canal. |
| `caller-name` | `header` | Si | Nombre de la API que realiza la invocacion al servicio. |
| `Ocp-Apim-Subscription-Key` | `header` | Si | Clave de suscripción de Azure API Management |
| `batchDebitId` | `Path` | Si | Código de carga que agrupa los archivos del proceso de Afiliación o Cobro de débito automático. |
| `fileId` | `Path` | Si | Código del archivo de débito automático. |

#### Ejemplos de Uso
#### 1. Obtiene la información de un archivo de debito
##### Request
```json
GET /batch-debits/12434/files/25
```
##### Response

```json
{
  "debitFile": {
    "fileName": "AfiliacionSoles0808.txt",
    "fileSize": 120000,
    "fileId": 25,
    "fileStatusCode": "EST001",
    "storageAccountFileName": "9fecd7a6-9d89-4d4d-a039-c28f7181fec0.txt",
    "companyService": {
      "serviceCode": 102,
      "serviceDescription": "Telefono fijo."
    },
    "processingResult": {
      "totalRecords": 145,
      "correctRecords": 145,
      "errorsQuantity": 145
    },
    "fileAmount": {
      "totalAmount": 10000,
      "currencyCode": "PEN"
    }
  }
}
```

## GET /files/{fileId}/operational-errors
Lista los errores que se validaron en el proceso de carga del archivo de Débito Automático.

#### Parámetros
| Parámetro | Tipo     | Requerido    | Descripción    |
| :-------- | :------- | :--------------- | :--------------- |
| `Authorization` | `header` | Si | Token de autorización |
| `Request-ID` | `header` | Si | Este campo es un valor estandar ya existente y sera usado como identificador. |
| `request-date` | `header` | Si | Fecha de la petición |
| `app-code` | `header` | Si | Codigo de la aplicacion que invoca el servicio. Se debe usar el codigo de 2 caracteres que tienen asignada las aplicaciones, puede ser el canal. |
| `caller-name` | `header` | Si | Nombre de la API que realiza la invocacion al servicio. |
| `Ocp-Apim-Subscription-Key` | `header` | Si | Clave de suscripción de Azure API Management |
| `batchDebitId` | `Path` | Si | Código de carga que agrupa los archivos del proceso de Afiliación o Cobro de débito automático. |
| `fileId` | `Path` | Si | Código del archivo de débito automático. |
| `limit` | `Query` | Si | Indica la cantidad máxima de registros a traer. |
| `integer` | `Query` | Si | Indica la cantidad de registros que serán exonerados en la búsqueda. |

#### Ejemplos de Uso
#### 1. Lista los errores de validación en un archivo de debito
##### Request
```json
GET /files/25/operational-errors
```
##### Response

```json
{
  "operationalErrors": [
    {
      "errorRowNumber": 1,
      "consumerCode": "US001",
      "errorMessage": "La cuenta de abono no es válida."
    }
  ],
  "metadata": {
    "totalRecords": 225,
    "offset": 50,
    "limit": 25
  }
}
```
## Payment Tracking
El recurso gestiona la información de seguimiento de una de aprobación para debito automatico .

### Endpoints
Endpoints disponibles para el recurso.

| Recurso | Método HTTP     | Endpoint  |    Grant Type |
| :-------- | :------- | :------------------------- | :------------------------- |
| payments-tracking | POST | POST /payments-tracking/{paymentTrackingId} |Client Credential|
| payments-tracking | GET | GET /payments-tracking/{paymentTrackingId} |Client Credential|
| payments-tracking | PATCH | PATCH /payments-tracking/{paymentTrackingId} |Client Credential|
| payments-tracking | POST | POST /payments-tracking/search |Client Credential|

### Modelo de Datos

| Nombre | XPath | Tipo de Dato|Array | Ejemplo | Descripción  | Codigos  |Pattern  |
| :-------- | :------- | :------------ | :---------- | :---------- | :---------- |:---------- |:---------- |
| `personId` | `Person` | `string` | No | 85423694 | Identificador del cliente | |
| `account` | `Account` | `object` | Si | | Objeto que representa la cuenta | |
| `accountId` |`Account/AccountId` |  `string` | No | 00019100109502007005 | Número de cuenta | |
| `formattedAccountNumber` |`Account/FormattedAccountNumber` |  `string` | No | 191-00109502-0-07 | Número de cuenta comercial formateado | |
| `status` |`Account/Status` |  `string` | No | Active | Estado de la cuenta | Active  Deactivated  Blocked|`^[A-Z]{3,3}$`|
| `cci` |`Account/CCI` |  `string` | No | 00225010919303912995 | Codigo de cuenta interbancario para Perú | |
| `openingDate` |`Account/OpeningDate` |  `string` | No | 1997-07-24 | Fecha de apertura de la cuenta. | |

## POST /payments-tracking/{paymentTrackingId}
Registra el seguimiento de aprobación del proceso de Débito Automático.

#### Parámetros
| Parámetro | Tipo     | Requerido    | Descripción    |
| :-------- | :------- | :--------------- | :--------------- |
| `Authorization` | `header` | Si | Token de autorización |
| `Request-ID` | `header` | Si | Este campo es un valor estandar ya existente y sera usado como identificador. |
| `request-date` | `header` | Si | Fecha de la petición |
| `app-code` | `header` | Si | Codigo de la aplicacion que invoca el servicio. Se debe usar el codigo de 2 caracteres que tienen asignada las aplicaciones, puede ser el canal. |
| `caller-name` | `header` | Si | Nombre de la API que realiza la invocacion al servicio. |
| `Ocp-Apim-Subscription-Key` | `header` | Si | Clave de suscripción de Azure API Management |
| `paymentTrackingId` | `Path` | Si | Código de seguimiento de la solicitud. |

#### Ejemplos de Uso
#### 1. Lista los errores de validación en un archivo de debito
##### Request
```json
POST /payments-tracking/1
{
  "trackingStatusCode": "EST001",
  "file": 25,
  "batchDebitId": 102030,
  "applicationCode": "NTLC"
}
```
##### Response

```json
  El registro la solicitud se realizo con exito.
```
## GET /payments-tracking/{paymentTrackingId}
Consulta la información de un seguimiento de aprobación de Debito Automático.

#### Parámetros
| Parámetro | Tipo     | Requerido    | Descripción    |
| :-------- | :------- | :--------------- | :--------------- |
| `Authorization` | `header` | Si | Token de autorización |
| `Request-ID` | `header` | Si | Este campo es un valor estandar ya existente y sera usado como identificador. |
| `request-date` | `header` | Si | Fecha de la petición |
| `app-code` | `header` | Si | Codigo de la aplicacion que invoca el servicio. Se debe usar el codigo de 2 caracteres que tienen asignada las aplicaciones, puede ser el canal. |
| `caller-name` | `header` | Si | Nombre de la API que realiza la invocacion al servicio. |
| `Ocp-Apim-Subscription-Key` | `header` | Si | Clave de suscripción de Azure API Management |
| `paymentTrackingId` | `Path` | Si | Código de seguimiento de la solicitud. |

#### Ejemplos de Uso
#### 1. Coonsulta la información de un seguimiento de aprobación de Debito Automático.
##### Request
```json
GET /payments-tracking/7025
```
##### Response

```json
  {
  "paymentTracking": {
    "paymentTrackingId": 7025,
    "applicationCode": "NTLC",
    "trackingStatusCode": "EST001",
    "debitFileInformation": {
      "fileName": "AfiliacionSoles0808.txt",
      "fileSize": 120000,
      "fileId": 25,
      "fileStatusCode": "EST001",
      "storageAccountFileName": "9fecd7a6-9d89-4d4d-a039-c28f7181fec0.txt",
      "companyService": {
        "serviceCode": 102,
        "serviceDescription": "Telefono fijo."
      },
      "processingResult": {
        "totalRecords": 145,
        "correctRecords": 145,
        "errorsQuantity": 145
      },
      "fileAmount": {
        "totalAmount": 10000,
        "currencyCode": "PEN"
      }
    },
    "trackingRegistrationDate": "2017-07-21T17:32:28Z",
    "trackingSteps": [
      {
        "stepStatusCode": "EST001",
        "stepRegistrationDate": "15-09-2023 10:58:00",
        "trackingStepUser": {
          "code": "U0001",
          "fullName": "Jose Perez"
        }
      }
    ]
  },
  "batchDebit": {
    "batchDebitTypeCode": "AFI",
    "company": {
      "companyId": 200
    }
  }
}
```
## PATCH /payments-tracking/{paymentTrackingId}
Actualiza el seguimiento de aprobación de una solicitud de Debito Automatico.

#### Parámetros
| Parámetro | Tipo     | Requerido    | Descripción    |
| :-------- | :------- | :--------------- | :--------------- |
| `Authorization` | `header` | Si | Token de autorización |
| `Request-ID` | `header` | Si | Este campo es un valor estandar ya existente y sera usado como identificador. |
| `request-date` | `header` | Si | Fecha de la petición |
| `app-code` | `header` | Si | Codigo de la aplicacion que invoca el servicio. Se debe usar el codigo de 2 caracteres que tienen asignada las aplicaciones, puede ser el canal. |
| `caller-name` | `header` | Si | Nombre de la API que realiza la invocacion al servicio. |
| `Ocp-Apim-Subscription-Key` | `header` | Si | Clave de suscripción de Azure API Management |
| `paymentTrackingId` | `Path` | Si | Código de seguimiento de la solicitud. |

#### Ejemplos de Uso
#### 1. Coonsulta la información de un seguimiento de aprobación de Debito Automático.
##### Request
```json
PATCH /payments-tracking/7025
{
  "trackingStatusCode": "EST001",
  "batchDebitId": 102030
}
```
##### Response

```json
  El procesamiento de la petición ha sido exitosa.
```

## POST /payments-tracking/search
Lista los seguimientos de aprobación de Debito Automático.

#### Parámetros
| Parámetro | Tipo     | Requerido    | Descripción    |
| :-------- | :------- | :--------------- | :--------------- |
| `Authorization` | `header` | Si | Token de autorización |
| `Request-ID` | `header` | Si | Este campo es un valor estandar ya existente y sera usado como identificador. |
| `request-date` | `header` | Si | Fecha de la petición |
| `app-code` | `header` | Si | Codigo de la aplicacion que invoca el servicio. Se debe usar el codigo de 2 caracteres que tienen asignada las aplicaciones, puede ser el canal. |
| `caller-name` | `header` | Si | Nombre de la API que realiza la invocacion al servicio. |
| `Ocp-Apim-Subscription-Key` | `header` | Si | Clave de suscripción de Azure API Management |

#### Ejemplos de Uso
#### 1. Coonsulta la información de un seguimiento de aprobación de Debito Automático.
##### Request
```json
PATCH /payments-tracking/search
{
  "paymentTracking": {
    "paymentTrackingId": 7025,
    "companyId": 200,
    "trackingStatus": "EST001",
    "companyService": {
      "serviceCode": 102
    }
  },
  "metadata": {
    "offset": 50,
    "limit": 25
  }
}
```
##### Response

```json
{
  "paymentsTracking": [
    {
      "paymentTrackingId": 7025,
      "applicationCode": "NTLC",
      "trackingStatusCode": "EST001",
      "debitFileInformation": {
        "fileName": "AfiliacionSoles0808.txt",
        "fileSize": 120000,
        "fileId": 25,
        "fileStatusCode": "EST001",
        "storageAccountFileName": "9fecd7a6-9d89-4d4d-a039-c28f7181fec0.txt",
        "companyService": {
          "serviceCode": 102,
          "serviceDescription": "Telefono fijo."
        },
        "processingResult": {
          "totalRecords": 145,
          "correctRecords": 145,
          "errorsQuantity": 145
        },
        "fileAmount": {
          "totalAmount": 10000,
          "currencyCode": "PEN"
        }
      },
      "trackingRegistrationDate": "2017-07-21T17:32:28Z",
      "trackingSteps": [
        {
          "stepStatusCode": "EST001",
          "stepRegistrationDate": "15-09-2023 10:58:00",
          "trackingStepUser": {
            "code": "U0001",
            "fullName": "Jose Perez"
          }
        }
      ]
    }
  ],
  "metadata": {
    "totalRecords": 225,
    "offset": 50,
    "limit": 25
  }
}  
```
## Debit Company
El recurso gestiona la información de las compañias de debito.

### Endpoints
Endpoints disponibles para el recurso.

| Recurso | Método HTTP     | Endpoint  |    Grant Type |
| :-------- | :------- | :------------------------- | :------------------------- |
| companies | GET | GET /companies/{companyId} |Client Credential|
| companies | GET | GET /companies/{companyId}/accounts |Client Credential|

### Modelo de Datos

| Nombre | XPath | Tipo de Dato|Array | Ejemplo | Descripción  | Codigos  |Pattern  |
| :-------- | :------- | :------------ | :---------- | :---------- | :---------- |:---------- |:---------- |
| `personId` | `Person` | `string` | No | 85423694 | Identificador del cliente | |
| `account` | `Account` | `object` | Si | | Objeto que representa la cuenta | |
| `accountId` |`Account/AccountId` |  `string` | No | 00019100109502007005 | Número de cuenta | |
| `formattedAccountNumber` |`Account/FormattedAccountNumber` |  `string` | No | 191-00109502-0-07 | Número de cuenta comercial formateado | |
| `status` |`Account/Status` |  `string` | No | Active | Estado de la cuenta | Active  Deactivated  Blocked|`^[A-Z]{3,3}$`|
| `cci` |`Account/CCI` |  `string` | No | 00225010919303912995 | Codigo de cuenta interbancario para Perú | |
| `openingDate` |`Account/OpeningDate` |  `string` | No | 1997-07-24 | Fecha de apertura de la cuenta. | |

## GET /companies/{companyId}
Consulta la información de una empresa de Débito Automático.

#### Parámetros
| Parámetro | Tipo     | Requerido    | Descripción    |
| :-------- | :------- | :--------------- | :--------------- |
| `Authorization` | `header` | Si | Token de autorización |
| `Request-ID` | `header` | Si | Este campo es un valor estandar ya existente y sera usado como identificador. |
| `request-date` | `header` | Si | Fecha de la petición |
| `app-code` | `header` | Si | Codigo de la aplicacion que invoca el servicio. Se debe usar el codigo de 2 caracteres que tienen asignada las aplicaciones, puede ser el canal. |
| `caller-name` | `header` | Si | Nombre de la API que realiza la invocacion al servicio. |
| `Ocp-Apim-Subscription-Key` | `header` | Si | Clave de suscripción de Azure API Management |
| `companyId` | `Path` | Si | Código cic de la empresa |

#### Ejemplos de Uso
#### 1. Consulta la información de una empresa de Débito Automático.
##### Request
```json
GET /companies/1
```
##### Response

```json
{
  "company": {
    "companyId": 8200200,
    "companyCode": 200,
    "companyName": "Telefonica",
    "identificationDocument": {
      "documentType": {
        "code": "DNI",
        "description": "Documento Nacional de Identidad."
      },
      "documentNumber": 73889235
    },
    "registrationDate": "2017-07-21T17:32:28Z",
    "companyStatus": "ENABLED"
  }
}  
``` 

## GET /companies/1/accounts
Lista las cuentas de una empresa de Débito Automático.

#### Parámetros
| Parámetro | Tipo     | Requerido    | Descripción    |
| :-------- | :------- | :--------------- | :--------------- |
| `Authorization` | `header` | Si | Token de autorización |
| `Request-ID` | `header` | Si | Este campo es un valor estandar ya existente y sera usado como identificador. |
| `request-date` | `header` | Si | Fecha de la petición |
| `app-code` | `header` | Si | Codigo de la aplicacion que invoca el servicio. Se debe usar el codigo de 2 caracteres que tienen asignada las aplicaciones, puede ser el canal. |
| `caller-name` | `header` | Si | Nombre de la API que realiza la invocacion al servicio. |
| `Ocp-Apim-Subscription-Key` | `header` | Si | Clave de suscripción de Azure API Management |
| `companyId` | `Path` | Si | Código cic de la empresa |

#### Ejemplos de Uso
#### 1. Lista las cuentas de una empresa de Débito Automático.
##### Request
```json
GET /companies/1/accounts
```
##### Response

```json
{
  "company": {
    "companyId": 8200200,
    "companyCode": 200,
    "companyName": "Telefonica",
    "identificationDocument": {
      "documentType": {
        "code": "DNI",
        "description": "Documento Nacional de Identidad."
      },
      "documentNumber": 73889235
    },
    "registrationDate": "2017-07-21T17:32:28Z",
    "companyStatus": "ENABLED"
  }
}  
```

# Autenticacion
Esta API está protegida por el protocolo Open Authentication (OAuth). Luego del intercambio de token con OAuth, se concede un token OAuth válido para acceder a los diferentes endpoints API en nombre de un usuario de la aplicación autorizada.
Para ello, se deben utilizar los siguientes pasos:

##### 1. Gestionar la creación del Cliente CAS
[Solicitud de Creación de Cliente CAS](https://confluence.devsecopsbcp.com/pages/viewpage.action?pageId=635077948)

##### 2. Gestionar la creación del Subscription-Key
[Solicitud para la creación de una suscripción](https://confluence.devsecopsbcp.com/display/AAGDAPUB/Suscripciones+en+Azure+APIM#SuscripcionesenAzureAPIM-Solicitudparalacreaci%C3%B3ndeunasuscripci%C3%B3nyobtenci%C3%B3ndeclavedesuscripci%C3%B3n)

# Configuraciones Adicionales

##### 1. Formato FCI
No aplica.

##### 2. Formato FAE
No aplica.

##### 3. Formato ZConnect:
No aplica.


# Documentación de Arquitectura

Documentación de la Arquitectura de Solución asociada al API.


| Código SDAS | Enlace     | Descripción                |
| :-------- | :------- | :------------------------- |
| **D-01471** | [Documentación](https://confluence.devsecopsbcp.com/download/attachments/561972461/Arq%20Componentes%20e%20Integracion%20-%20Debito%20Automatico.png?api=v2) | Creación del API |


# Portal de APIs
El API se encuentra en el Portafolio de APIs en el siguiente link
https://www.portalapisbcp.com/#/api-detail/2407
