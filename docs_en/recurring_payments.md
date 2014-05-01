PagoFácil Recurring-Charges API Documentation
==

# Abstract

PagoFácil recurring Charges API provides a number of methods in order to register, cancel, consult and (soon to be released) modify recurring payments for any customer which has access to this product.

# Endpoints

## About Consuming Endpoints

Requests can send POST or GET data and depending on which ENDPOINT is being invocated response format vary.

### API Keys

Before getting to down to the code you should get your API Keys which are two sets: one set will work for Tests Environment, and the other one for Production Environment. This was thought this way in order to avoid any confusion on wich environment is being used. This keys are available on your [account manager](https://manager.pagofacil.net).

### API Secret

In order to fulfill request for specific methods API Engine will ask for an *API Secret key**, to get this key ask us at soporte-at-pagofacil-dot-net how to get it.

Methods which require this API Secret Key are:
- recurring charge cancelation
- reporting


### Environments

There are two different sets of endpoints: **tests** and **production**, and are intended to be used as each name suggests:

## Tests Environment

All requests made to this environment are processed, stored and produce an *adhoc* response to the consumer, but **will never process the Credit Card charge**. This charges' information will be available at [**Tests Manager**](https://stmanager.pagofacil.net).

In tests environment all recieved data is only checked to be well formatted and if it is so will allways send a success register as recurring charge, but if **cargo** (*do_charge*, see variables specification) is set to 1, a random success/fail response will be sent as charge outcome.

According to your needs, you may choose from the following tests endpoints' table,

|method|input |response | URL|
|----|------------------|---------------|-----------------|
|GET |Serialized String |JSON |**https:**//www.pagofacil.net/st/public/Wsrrecurrentes/index/format/json?|
|POST|JSON Object       |JSON |**https:**//www.pagofacil.net/st/public/Wsjrecurrentes/|
|GET |Serialized String |XML  |**https:**//www.pagofacil.net/st/public/Wsrrecurrentes/|

### Examples

#### Request For GET-JSON and GET-XML

```
curl "https://www.pagofacil.net/st/public/Wsrrecurrentes/index/format/json?\
method=transaccion&\
data%5Bnombre%5D=John&\
data%5Bapellidos%5D=Doe&\
data%5BnumeroTarjeta%5D=4111111111111111&\
data%5Bcvt%5D=222&\
data%5Bcp%5D=11000&\
data%5BmesExpiracion%5D=09&\
data%5BanyoExpiracion%5D=16&\
data%5Bmonto%5D=800&\
data%5BidSucursal%5D=a30bb30df93a9468b878a51e70eac065497bdaab&\
data%5BidUsuario%5D=a2ad03e28952266fb687c249fa6ab45bbb4d7a1d&\
data%5BIP%5D=200.1.1.1&\
data%5BidServicio%5D=2&%20\
data%5Bemail%5D=user@gmail.com&\
data%5Btelefono%5D=5555555555&\
data%5Bcelular%5D=5555111111&\
data%5BcalleyNumero%5D=Amargura%203&\
data%5Bcolonia%5D=la%20otra%20colonia&\
data%5Bmunicipio%5D=guadalajara&\
data%5Bestado%5D=Jalisco&\
data%5Bpais%5D=Mexico&\
data%5BidCliente%5D=Contrato789232&\
data%5BdiaPago%5D=05&\
data%5BfechaIniCobro%5D=16-09-2013&\
data%5BhttpUserAgent%5D=Mozilla/5.0%20Windows%20NT%206.0%20Safari/537.36"
```

Some notes on previous example:
- note **method=transaccion" as method to invoke
- particularly opening and closing brackets ("[" and "]") URL Encoded as **%5B** and **%5D** (any semi respected programming language should have an equivalent for URL Encode function)
- all values for data[foo] should be URL Encoded (see previous point)

#### Request For POST-JSON

Send by POST an object like this
```
{
    "jsonrpc": "2.0", // fixed key-value pair
    "method": "transaccion", // method to invoke
    "params": {
        "data": {
            "nombre": "Mario A",
            "apellidos": "Martinez Leandro",
            "numeroTarjeta": "5111111111111111",
            "cvt": "222",
            "cp": "11000",
            "mesExpiracion": "09",
            "anyoExpiracion": "16",
            "monto": "800",
            "idSucursal": "a30bb30df93a9468b878a51e70eac065497bdaab",
            "idUsuario": "a2ad03e28952266fb687c249fa6ab45bbb4d7a1d",
            "IP": "200.1.1.1",
            "idServicio": 2,  // fixed value for recurring charges
            "email": "user@gmail.com",
            "telefono": "5555555555",
            "celular": "5555111111",
            "calleyNumero": "Amargura 3",
            "colonia": "la otra colonia",
            "municipio": "guadalajara",
            "estado": "Jalisco",
            "pais": "Mexico",
            "idCliente": "Contrato789232",
            "diaPago": "05",
            "fechaIniCobro": "16-09-2013",
            "httpUserAgent": "Mozilla/5.0 (Windows NT 6.0) Safari/537.36"
        }
    },
    "id": 1397151272 // send a random number, has to be uniqe
}
```

#### JSON Response
```
{
  "result": {
    "status": 1, // means OK, charge registered
    "idRecurrente": "T-RAPFE1S1I368", // important value
    "texto": "Transaccion registrada con exito",
    "fechaRegistro": "2014/05/01 15:56:26",
    "idCargo": "368",
    "empresa": "PagoFácil.Net",
    "TransIni": "15:56:26 pm 01/05/2014",
    "TransFin": "15:56:27 pm 01/05/2014",
    "TipoTC": "Master Card",
    "data": {
      "nombre": "Mario A",
      "apellidos": "Martinez Leandro",
      "numeroTarjeta": "(16) **** **** **** 1111",
      "cvt": "(3) ***",
      "cp": "11000",
      "mesExpiracion": "(2) **",
      "anyoExpiracion": "(2) **",
      "monto": "800",
      "idSucursal": "a30bb30df93a9468b878a51e70eac065497bdaab",
      "idUsuario": "a2ad03e28952266fb687c249fa6ab45bbb4d7a1d",
      "IP": "200.1.1.1",
      "idServicio": 2,
      "email": "user@gmail.com",
      "telefono": "5555555555",
      "celular": "55551111111",
      "calleyNumero": "amargura 3",
      "colonia": "la otra colonia",
      "municipio": "Guadalajara",
      "estado": "Jalisco",
      "pais": "Mexico",
      "idCliente": "Contrato789232",
      "diaPago": "05",
      "fechaIniCobro": "16-09-2013",
      "httpUserAgent": "Mozilla/5.0 (Windows NT 6.0) Safari/537.36",
      "transFechaHora": 1398977786
    },
    "dataVal": {
      "idSucursal": "1",
      "nombre": "Mario A",
      "apellidos": "Martinez Leandro",
      "numeroTarjeta": "(16) **** **** **** 1111",
      "cp": "11000",
      "cvt": "(3) ***",
      "monto": "800",
      "mesExpiracion": "(2) **",
      "anyoExpiracion": "(2) **",
      "idUsuario": "1",
      "idServicio": "2",
      "idCliente": "Contrato789232",
      "diaPago": "5",
      "fechaIniCobro": "2013-09-16",
      "ip": "200.57.73.165",
      "httpUserAgent": "Mozilla/5.0 (Windows NT 6.0) AppleWebKit/537.36 (KHTML like Gecko) Chrome/27.0.1453.110 Safari/537.36",
      "tipoTarjeta": "Master Card",
      "hashKeyCC": "398442abcd105ad2406ad660efd14540c34ab112",
      "idEmpresa": "1",
      "nombre_comercial": "PagoFácil.Net",
      "emailEmpresa": "javier@pagofacil.net",
      "tipo": "E",
      "transFechaHora": 1398977786,
      "noProcess": null,
      "noMail": null,
      "notaMail": null,
      "fechaFinCobro": null,
      "https": "on"
    }
  },
  "id": "1397151272",
  "jsonrpc": "2.0"
}
```

## Production Environment

Production environment is exactly the same as Tests environment.

Once you have developed and tested your application and it works OK in Tests environment and you're ready to migrate it to production environment, do so, just consider these important points:

 - Requests' format **is the same**
 - Endpoints' URL **are not the same** (se following table)
 - Change your two API keys to the Production set values
 - **EVERY REQUEST MADE WITHIN THIS ENVIRONMENT WILL DERIVATE IN A CHARGE [$$$!] TO THE CREDIT CARD** Ultimately, this is what we want, right? Just **be cautious**.

So if you're ok with this, here are the **production endpoints**:

|method|input |response | URL|
|----|------------------|---------------|-----------------|
|GET |Serialized String |JSON |**https:**//www.pagofacil.net/ws/public/Wsrrecurrentes/index/format/json?|
|POST|JSON Object       |JSON |**https:**//www.pagofacil.net/ws/public/Wsjrecurrentes/|
|GET |Serialized String |XML  |**https:**//www.pagofacil.net/ws/public/Wsrrecurrentes/|



# Methods

Using this API you have access to the following methods for the recurring charges product. Note that if what you're looking for is not within the API's capabilities, you can find it on the [manager](manager.pagofacil.net).

1. register
2. cancel
3. modify (soon to be released)
4. check one
5. check all (incluiding optionally all operations by that account)

### NOTES for English documentation
Most variables are in Spanish, so every table will have an extra column offering English translation of each and every variable but **use spanish named variables when requesting any transaction to PagoFácil API** until further notice. (Version 2.0 contemplates both English and Spanish named variables to co-exist).

#### Date Format

It should also be noted that *date* variables are latin format: **dd-mm-yyyy** (instead of US format like mm-dd-yyyy -**NO**-).

## register method

### request variables:

|**name**|**translation***|**type**|**mandatory**|**comments**|
|-----|-----|:-----|:-----:|:-----|
|**nombre**|first_name|varchar(50)|yes|Cardholder name|
|**apellidos**|last_name|varchar(50)|yes|Cardholder lastname(s) |
|**numeroTarjeta**|cc_number|varchar(16)|yes|Credit Card number. **No spaces. No dashes.**|
|**cvt**|cvt|integer(3)|no|Card's security code. This field is needed if *cargo* variable is set to **1**, any other case won't be needed. This field is never stored.|
|**cp**|zip_code|varchar(9)|yes|Cardholder's address Zip Code|
|**mesExpiracion**|exp_month|int(2)|yes|Credit Card expiration month|
|**anyoExpiracion**|exp_year|int(2)|yes|Credit Card expiration year|
|**monto**|amount|decimal(7,2)|yes|Amount to be charged|
|**idSucursal**|branch_id|varchar(20)|yes||
|**idUsuario**|user_id|varchar(20)|yes||
|**idServicio**|service_id|integer|yes|This variable denotes which service is consumed. Set idServicio = 1 to use recurring Payments|
|**email**|email|varchar(200)|yes|Cardholder's email|
|**telefono**|phone_number|varchar(10)|yes|Cardholders home phone number|
|**celular**|cellphone_number|varchar(10)|yes|Cardholder's cell phone number|
|**calleYNumero**|address_street|varchar(45)|yes|Cardholder's Address Street Name and Number|
|**colonia**|neighborhood|varchar(30)|yes|Cardholder's Address Neighborhood.|
|**municipio**|city|varchar(30)|yes|Cardholder's Address City.|
|**estado**|state|varchar(45)|yes|Cardholder's Address State.|
|**pais**|country|varchar(50)|yes|Cardholder's Country|
|**idCliente**|client_id|varchar(10)|no||
|**diaPago**|charge_day|int(2)|yes|Day of the month when the recurring charge will be applied. If it is set to 29, 30 or 31 and the applying month doesn't have that day, it will be applied the last day of such monht.|
|**fechaIniCobro**|charge_start|date|yes|Start date for recurring charge, should be set to the future. Use **dd-mm-yyyy** format.|
|**fechaFinCobro**|charge_end|date|no|End date for recurring charge. Note that it is not a mandatory field. Use **dd-mm-yyyy** format.|
|**httpUserAgent**|user_agent|varchar(150)|no|Registrant's HTTP User Agent|
|**ip**|ip_address|varchar(16)|no|IPV4 dotted-decimal notation, i.e. *192.23.45.210*.|
|**cargo**|do_charge|int(1)|no|If this variable is set to **1**, the moment the recurring payment is registered will do one charge to the credit card and with the amount specified in the same request, no matter *fechaIniCobro* (charge_start) nor *diaPago* (charge_day) values.|

### response example

## cancel method

## check one

This method allows to check the status of a recurring charge using its own id.

### request variables:
|name|translation|type|mandatory|comments|
|----|----|----|----|----|----|
|**idRecurrente**|recurring_charge_id|varchar(50)|yes|Recurring charge ID provided by either the response object or manager report.|
|**idSucursal**|branch_id|varchar(20)|yes|Branch ID to which this recurring charge belongs.|

## check all
See reports endpoint.


## Common Errors
(Still in process)
### Bad API Keys
```
{
	"WebServices_Recurrentes":{
		"transaccion":{
			"registrado":"0",
			"transaccion":"n/a",
			"autorizacion":"n/a",
			"texto":"Errores en los datos de entrada Validaciones",
			"error":{
				"idUsuario":"idUsuario  a2ad03e28952266fb687c249fa6ab45bbb4d7a1d no encontrado"
			},
		}
	}
}

```