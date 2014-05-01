PagoFácil Recurring-Charges API Documentation
==

# Abstract

PagoFácil recurring Charges API provides a number of methods in order to register, cancel, consult and (soon to be released) modify recurring payments for any customer which has access to this product.

# Endpoints

## Consuming Endpoints

Requests can send POST or GET data and depending on which ENDPOINT is being invocated response format vary.

There are two different sets of endpoints: **tests** and **production**, and are intended to be used as each name suggests:

## Tests Environment

All requests made to this environment are processed, stored and produce an *adhoc* response to the consumer, but **will never process the Credit Card charge**. This charges' information will be available at [**Tests Manager**](https://stmanager.pagofacil.net).

In tests environment all recieved data is checked to be well formatted, and will allways send a success register as recurring charge, but if **cargo** (*do_charge*, see variables specification) is set to 1, a random success/fail response will be sent as charge outcome.

According to your needs, you may choose from the following tests endpoints' table,

|request method|input type|response format|URL|
|--------------|----------|---------------|---|
|GET|Serialized String|JSON|**https:**//www.pagofacil.net/st/public/Wsrrecurrentes/index/format/json?|
|POST|JSON Object|JSON|**https:**//www.pagofacil.net/st/public/Wsjrecurrentes/|
|GET|Serialized String|XML|**https:**//www.pagofacil.net/st/public/Wsrrecurrentes/|

## Production Environment


# Methods

1. register
2. cancel
3. modify (soon to be released)
4. check one
5. check all (incluiding optionally all operations by that account)

### NOTES on English documentation
Most variables are in Spanish, so every table will have an extra column offering English translation of each and every variable but **use spanish named variables when requesting any transaction to PagoFácil API** until further notice. (Version 2.0 contemplates both English and Spanish named variables to co-exist).

#### Date Format

It should also be noted that *date* variables are latin format: **dd-mm-yyyy** (instead of US format like mm-dd-yyyy -**NO**-).

## register method

### variables:

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

### variables:
|name|translation|type|mandatory|comments|
|----|----|----|----|----|----|
|**idRecurrente**|recurring_charge_id|varchar(50)|yes|Recurring charge ID provided by either the response object or manager report.|
|**idSucursal**|branch_id|varchar(20)|yes|Branch ID to which this recurring charge belongs.|

## check all
See 