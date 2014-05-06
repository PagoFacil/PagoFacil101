PagoFácil Transaction Search API Documentation
==============================================

# Abstract

Transaction Search API is meant to give each and every customer a secure, comprehensive and detailed way to browse through its own transaction history.

# Endpoints

# Methods

1. rss (*search* soon to be)

### Notes for English documentation

#### Date Format

## rss method

### request variables

|**name**|**translation***|**type**|**mandatory**|**comments**|
|-----|-----|:-----|:-----:|:-----|
|data[**api_secret**]||varchar(65)|yes|API secret|
|data[**dateStart**]||date|no|Date|
|data[**dateEnd**]||date|no|date|
|data[**status_id**]||int||Catalog: 0.- failed 1.- approved|
|data[**branch_id**]||int||ID from the branch you want to get all transactions from.|
|p||int|no|If the result is bigger than the page size, you can ask for a specific page.|


