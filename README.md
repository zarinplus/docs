![Zarinplus Logo](https://www.zarinplus.com/wp-content/uploads/2021/10/ZarinPlus-logo-light.svg?nocache)

# Zarinplus Docs

[![MIT licensed](https://img.shields.io/badge/license-MIT-blue.svg)](./license)

## Table of Contents

* [Payment](#payment)
	* [Create Payment Request Via API](#create-payment-request-via-api)
        * [Process Transaction Via API](#process-transaction-via-api)
	* [Cancel Transaction Via API](#cancel-transaction-via-api)
	* [Verify Transaction Via API](#verify-transaction-via-api)
	* [Reverse Transaction Via API](#reverse-transaction-via-api)

## Payment

This part of document is for merchants _(Who accept payments via points)_, If you would like to create payment requests from your server and verify the transactions, Please read this document to understand how to implement payment via request endpoint.

### Create Payment Request Via API
To make a request transaction buyers should use this API. 

#### Request Endpoint

	https://api.zarinplus.com/payment/request
	

#### Body

Send these parameters to the request endpoint via `GET` or `POST` method.

	{
		"currency" : "IRR",
		"amount" : 50000,
		"cancel" : "http://www.mystore.com/cancel/",
		"success" : "http://www.mystore.com/success/",
		"item" : "5000 Toman Voucher",
		"cellphone" : 989121111111,
		"email" : "yourname@domain.com",
		"token" : "mystoretoken",
		"custom_logo" : "http://www.mystore.com/logo.png",
		"custom_name" : "mystore",
		"custom_domain" : "http://www.mystore.com"
		"redirect" : 1,
	}

#### Body Schema

This schema define the each parameter's type and value.

	{
		"currency" : {
			"type" : "string",
			"description" : "Currency is required, Those currency codes are supported IRR, IRT, EMZ, USD"
		},
		"amount" : {
			"type" : "float"
			"description" : "Amount is required"
		},
		"cancel" : {
			"type" : "string"
			"description" : "The cancel URL"
		},
		"success" : {
			"type" : "string"
			"description" : "The success URL"
		},
		"item" : {
			"type" : "string"
			"description" : "Details of transaction"
		},
		"cellphone" : {
			"type" : "integer"
			"description" : "Buyer cellphone"
		},
		"email" : {
			"type" : "string"
			"description" : "Buyer email address"
		},
		"token" : {
			"type" : "string"
			"description" : "Token is required"
		},
		"custom_logo" : {
		   "type" : "string"
		   "description": "The logo image URL"
		},
		"custom_name" : {
		   "type" : "string"
		   "description": "Store name"
		},
		"custom_domain" : {
		   "type" : "string"
		   "description": "The store URL"
		},
		"redirect" : {
		   "type" : "integer"
		   "description": "1 for TRUE and 0 for FALSE"
		},
	}
  
#### Good Response

If you send correct request then the response should be same thing like that, Please watch [Sample Codes](https://github.com/zarinplus/samplecodes/tree/master/php/payment).

	{
		"status" : "200",
		"message" : "Get the authority code",
		"authority" : "f72705fe8a9bfc8895cc5dac121931f696d00b61"
	}

#### Redirect To Offerwall

Now you should build the offerwall's URL by replacing `{authority}` with the authority you received from previous step and then redirect to offerwall.

Also you must store the authority value for this transaction in your database for next action.

	https://api.zarinplus.com/?authority={authority}


### Process Transaction Via API

After the buyers make the payments request, we asked them to process the transaction.
Our authentication is with authorization key and they should confirm the payment transaction with their `{authorization token}` from their merchant account that will be sent from headers.

	{
		"Authorization" : "Token 60bee87h72170f19eefb4d9d0a0fda20553ec85a"
	}


#### Process Endpoint

	https://api.zarinplus.com/payment/process

#### Body

Now please, Send `authority` and `{private token}` and the `wallet_id` back for confirm.
Send this parameters to the verify endpoint via `GET` or `POST` method.

	{
		"authority" : "f72715fe8a1bfc8895cc5dac121931f696d00b61",
		"wallet_id" : "1"
	}

#### Body Schema

This schema define the each parameter's type and value.

	{
		"authority" : {
			"type" : "string"
			"description" : "Authority that you received on your success URL"
		},
		"wallet_id" : {
		   "type" : "integer"
		   "description": "The wallet_id for the currency that you have chosen"
		},
	}

#### Good Response

Well now, The payment is confirmed. It's time to waite for your transaction to be confirmed.
If you send correct request then the response should be same thing like that, Please watch [Sample Codes](https://github.com/emtiyaz-app/samplecodes/tree/master/php/payment).

	{
    		"status": 200,
    		"message": "Successful merchant - code 100"
	}

### Cancel Transaction Via API

After the buyers make the payments request, we asked them to process the transaction
they should cancel the payment transaction with their `{authorization token}` from their merchant account that will be sent from headers.
	
	{
		"Authorization" : "Token 60bee87h72170f19eefb4d9d0a0fda20553ec85a"
	}

#### Cancel Endpoint

	https://api.zarinplus.com/payment/cancel

#### Body

Now please, Send `authority` and `{private token}` and the `wallet_id` back for confirm.
Send this parameters to the verify endpoint via `GET` or `POST` method.

	{
		"authority" : "f72715fe8a1bfc8895cc5dac121931f696d00b61"
	}

#### Body Schema

This schema define the each parameter's type and value.

	{
		"authority" : {
			"type" : "string"
			"description" : "Authority that you received on your success URL"
		}
	}

#### Good Response

The payment is canceled as you wished for.
If you send correct request then the response should be same thing like that, Please watch [Sample Codes](https://github.com/emtiyaz-app/samplecodes/tree/master/php/payment).

	{
    		"status": 200,
    		"message": "You have canceled merchant- id 5"
	}


#### Verify Endpoint

	https://api.zarinplus.com/payment/verify

#### Body

Now please, Send `authority` back for verfiy.
Send this parameters to the verify endpoint via `GET` or `POST` method.

	{
		"authority" : "f72715fe8a1bfc8895cc5dac121931f696d00b61",
		"token" : "storetoken
	}

#### Body Schema

This schema define the each parameter's type and value.

	{
		"authority" : {
			"type" : "string"
			"description" : "Authority that you received on your success URL"
		},
		"token" : {
		   "type" : "string
		   "description": "The store token wich is required"
		}
	}

#### Good Response

Well now, The payment is complete and verified, It's time to deliver your goods or services to buyer.
If you send correct request then the response should be same thing like that, Please watch [Sample Codes](https://github.com/emtiyaz-app/samplecodes/tree/master/php/payment).

	{
		"status" : "200",
		"message" : "Successful",
		"reference" : "46a1e78525619a25b368c32b9ba11b92f6063e0c"
		"amount" : "50000",
		"currency" : "IRR",
		"item" : "5000 Toman Voucher",
		"email" : "name@email.com",
		"cellphone" : "989121111111"
	}

### Reverse Transaction Via API

You can reverse any successful transaction. 

#### Reverse Endpoint

	https://api.zarinplus.com/payment/reverse

#### Body

Now please, Send `authority` OR `reference` back for reverse.
Send this parameters to the verify endpoint via `GET` or `POST` method.

	{
		"reference" : "46a1e78525619a25b368c32b9ba11b92f6063e0c",
		"authority" : "f72715fe8a1bfc8895cc5dac121931f696d00b61",
		"token" : "storetoken
	}

#### Body Schema

This schema define the each parameter's type and value.

	{
		"reference" : {
			"type" : "string"
			"description" : "The reference that you received on verify method"
		},
		"authority" : {
			"type" : "string"
			"description" : "Authority that you received on your success URL"
		},
		"token" : {
		   "type" : "string
		   "description": "The store token wich is required"
		}
	}

#### Good Response

Well now, The transaction successfully reversed.

	{
		"status" : "500",
		"message" : "Reversed",
		"amount" : "50000",
		"currency" : "IRR",
		"item" : "5000 Toman Voucher",
		"email" : "name@email.com",
		"cellphone" : "989121111111"
	}


## List of Response Codes

|  Code	    | Message       |
| ------------- |:-------------:|
| 401 | Invalid point value | 
| 402 | Invalid amount or currency |
| 403 | Invalid token |
| 200 | Successful |
| 471 | Unknown error |
| 481 | Invalid authority |
| 482 | Invalid authority |
| 300 | Used authority |



