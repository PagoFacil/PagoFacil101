PagoFácil Transaction Search API Documentation
==============================================

# Abstract

Transaction Search API is meant to give each and every customer a secure, comprehensive and detailed way to browse through its own transaction history.

**This functionality is still in public beta release.**


## Endpoints

There are two different endpoints for each use: tests and production, each one will only reflect transactions made in each environment.

### Tests

A special note on *tests* environment: PagoFácil periodically erases all transactions older than a month, so please take this in consideration.

````
https://www.pagofacil.net/st/public/reports/Wsrreports/index/format/json/?
````

### Production

````
https://www.pagofacil.net/ws/public/reports/Wsrreports/index/format/json/?
````


# Methods

1. rss (*search* soon to be)

### NOTES for English documentation
Most variables are in Spanish, so every table will have an extra column offering English translation of each and every variable but **use spanish named variables when requesting any transaction to PagoFácil API** until further notice. (Version 2.0 contemplates both English and Spanish named variables to co-exist).

#### Date Format

It should also be noted that *date* variables are latin format: **dd-mm-yyyy** (instead of US format like mm-dd-yyyy -**NO**-).
## rss method

### request variables

|**name**|**translation***|**type**|**mandatory**|**comments**|
|-----|-----|:-----|:-----:|:-----|
|data[**api_secret**]||varchar(65)|yes|API secret|
|data[**dateStart**]||date|no|Date|
|data[**dateEnd**]||date|no|date|
|data[**status_id**]||int|no|Catalog: 0.- failed 1.- approved|
|data[**branch_id**]||int|no|ID from the branch you want to get all transactions from.|
|p||int|no|If the result is bigger than the page size, you can ask for a specific page.|

### example

#### request

````
curl "https://www.pagofacil.net/st/public/reports/Wsrreports/index/format/json/?
method=rss&
data%5Bdate%5D=26-03-2012&
data%5BdateEnd%5D=26-03-2014&
data%5Bbranch_id%5D=1&
data%5Btransaction_id%5D=T-RPFE1S1I750&
data%5Bstatus_id%5D=2&
p=1&
data%5Bapi_secret%5D=0617d6721ad648162b3eacf16310d47c6b991effb224ad7862cefebf1b3fdcab"
````

Some notes on previous example:
- this request points to Tests environment
- note **method=rss** as method to invoke
- particularly opening and closing brackets ("[" and "]") URL Encoded as **%5B** and **%5D** (any semi respected programming language should have an equivalent for URL Encode function)
- all values for data[foo] should be URL Encoded (see previous point)


#### response

`````` json
{
	"status":{
		"result":"ok",
		"elements":"1",
		"no_pages":"1",
		"no_page":"1",
		"no_elements_page":"1",
		"no_elements_per_page":"500",
		"overload":"0",
		"response_time":"8691",
		"message":"Search Ok",
		"charset":"utf-8"
	},
	"transactions":{
		"T-RPFE1S1I750":{
			"transaction_id":"T-RPFE1S1I750",
			"order_id":"Contrato30",
			"branch_id":"1",
			"branch_name":"Corporativo",
			"param_1":"",
			"param_2":"",
			"param_3":"",
			"param_4":"",
			"param_5":"",
			"amount":"9304.46",
			"timestamp":"2014-01-08 11:29:31",
			"authorized":"1",
			"authorization_number":"123456",
			"status":"2",
			"processor_message":"Approved",
			"transaction_type":"4",
			"transaction_via":"recurring",
			"card_info":{
				"brand":"Master Card",
				"country":"",
				"last_4_digits":"1234",
				"bank":"ACME Bank"
			},
			"cardholder":{
				"email":"johndoe@aol.com",
				"name":"John Doe"
			}
		}
	}
}
``````

As previous example shows, respnse is formed by two different JSON objects:
1. *status* object
2. list of *transaction* type objects

##### *status* object
Status object shows the status of the transaction incluiding extra variables for pagination. If query results in more than 500 items (maximum per page) it will be paginated and so should be indicated in another request using another parameter using the **p** variable.

##### *transaction* list
List of all *transaction* objects which meat criteria.

