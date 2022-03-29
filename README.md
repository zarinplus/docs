![Zarinplus Logo](https://www.zarinplus.com/wp-content/uploads/2021/10/ZarinPlus-logo-light.svg?nocache)

## Zarinplus Payment Docs 

This part of document is for merchants _(Who accept payments via EMZ)_, If you would like to create payment requests from your server and verify the transactions, Please read this document to understand how to implement payment via request endpoint.

## Table of Contents

* [Payment](#payment)
	* [Create Payment Request](#create-payment-request)
	* [Process Transaction](#process-transaction)
	* [Cancel Transaction](#cancel-transaction)
	* [Verify Transaction](#verify-transaction)
	* [Reverse Transaction](#reverse-transaction)
	* [List of Status Codes](#list-of-status-codes)

### Create Payment Request
To make a request transaction merchant should use this APIs. 

#### Request Endpoint

	https://api.zarinplus.com/payment/request
	

#### Body

Send these parameters to the request endpoint via `POST` method.

	{
		"currency" : "IRR",
		"amount" : 50000,
		"cancel" : "http://www.mystore.com/cancel/",
		"success" : "http://www.mystore.com/success/",
		"item" : "5000 Toman Voucher",
		"cellphone" : 989121111111,
		"email" : "yourname@domain.com",
		"token" : "9a1bfc8895cc5df72715fe81f6ac121936d00b61",
		"custom_logo" : "https://www.mystore.com/logo.png",
		"custom_name" : "Mystore",
		"custom_domain" : "https://www.mystore.com",
		"redirect" : 1,
	}

#### Body Schema

This schema define the each parameter's type and value.

	{
		"currency" : {
			"type" : "string",
			"description" : "Currency is required, Those currency codes are supported `IRR`, `EMZ` "
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
			"description" : "Merchant Token is required"
		},
		"custom_logo" : {
		   "type" : "string"
		   "description": "The Merchant logo image URL"
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

If you send correct request then the response should be same thing like that.

	{
		"status" : "200",
		"message" : "Get the authority code",
		"authority" : "f72705fe8a9bfc8895cc5dac121931f696d00b61"
	}



### Process Transaction

We asked user to process the transaction. User authentication send via header request and user should confirm the payment transaction in-app.

	{
		"Authorization" : "Token 60bee87h72170f19eefb4d9d0a0fda20553ec85a"
	}


#### Process Endpoint

	https://api.zarinplus.com/payment/process

#### Body

Now please, Send `authority` and the `wallet_id` back for confirm.
Send this parameters to the verify endpoint via `POST` method.

	{
		"authority" : "f72715fe8a1bfc8895cc5dac121931f696d00b61",
		"wallet_id" : "1"
	}

#### Body Schema

This schema define the each parameter's type and value.

	{
		"authority" : {
			"type" : "string"
			"description" : "Authority key which you received from payment request"
		},
		"wallet_id" : {
		   "type" : "integer"
		   "description": "The currency that you have chosen"
		},
	}

#### Good Response

Well now, The payment is confirmed. It's time to waite for your transaction to be confirmed.
If you send correct request then the response should be same thing like that.

	{
    		"status": 200,
    		"message": "Successful merchant"
	}

### Cancel Transaction

Users can cancel the transaction. User authentication send via header request and user should confirm the payment transaction in-app.
	
	{
		"Authorization" : "Token 60bee87h72170f19eefb4d9d0a0fda20553ec85a"
	}

#### Cancel Endpoint

	https://api.zarinplus.com/payment/cancel

#### Body

Now please, Send `authority` and `{private token}` and the `wallet_id` back for confirm.
Send this parameters to the verify endpoint via `POST` method.

	{
		"authority" : "f72715fe8a1bfc8895cc5dac121931f696d00b61"
	}

#### Body Schema

This schema define the each parameter's type and value.

	{
		"authority" : {
			"type" : "string"
			"description" : "Authority key which you received from payment request"
		}
	}

#### Good Response

The payment is canceled as you wished for. If you send correct request then the response should be same thing like that.

	{
    		"status": 200,
    		"message": "You have canceled merchant"
	}

### Verify Transaction

Merchant should verify transaction after passed by user.

#### Verify Endpoint

	https://api.zarinplus.com/payment/verify

#### Body

Send this parameters to the verify endpoint via `POST` method.

	{
		"authority" : "f72715fe81f69a1bfc8895cc5dac121936d00b61",
		"token" : "9a1bfc8895cc5df72715fe81f6ac121936d00b61"
	}

#### Body Schema

This schema define the each parameter's type and value.

	{
		"authority" : {
			"type" : "string"
			"description" : "Authority key which you received from payment request"
		},
		"token" : {
		   "type" : "string
		   "description": "The merchant token is required"
		}
	}

#### Good Response

Well now, The payment is completed and verified, It's time to deliver your goods or services to your buyer.
If you send correct request then the response should be same thing like that.

	{
		"status" : "200",
		"message" : "Successful",
		"reference" : "46a1e78525619a25b368c32b9ba11b92f6063e0c"
		"amount" : "50000",
		"currency" : "IRR",
		"item" : "5000 Toman Voucher",
		"email" : "yourname@domain.com",
		"cellphone" : "989121111111"
	}

### Reverse Transaction

You can reverse any successful transaction. 

#### Reverse Endpoint

	https://api.zarinplus.com/payment/reverse

#### Body

Send this parameters to the verify endpoint via `POST` method.

	{
		"reference" : "46a1e78525619a25b368c32b9ba11b92f6063e0c",
		"authority" : "f72715fe8a1bfc8895cc5dac121931f696d00b61",
		"token" : "9a1bfc8895cc5df72715fe81f6ac121936d00b61"
	}

#### Body Schema

This schema define the each parameter's type and value.

	{
		"reference" : {
			"type" : "string"
			"description" : "The reference that you received on verify"
		},
		"authority" : {
			"type" : "string"
			"description" : "Authority key which you received from payment request"
		},
		"token" : {
		   "type" : "string
		   "description": "The merchant token is required"
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
		"email" : "yourname@domain.com",
		"cellphone" : "989121111111"
	}


## List of Status Codes

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



