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
	* [Wallet List](#wallet-list)
	* [List of Status Codes](#list-of-status-codes)

* [White-label](#White-label)
	* [Token](#token)
	* [Collect](#collect)


## Payment
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

We asked users to process the transaction. They should confirm the payment transaction in-app.


#### Process Endpoint

	https://api.zarinplus.com/payment/process

#### Body

Now please, Send `authority`, `access_token` and the `wallet_id` back for confirm.
Send this parameters to the verify endpoint via `POST` method.

	{
		"authority" : "f72715fe8a1bfc8895cc5dac121931f696d00b61",
		"wallet_id" : "1",
		"access_token" : "Zarinpalusertoken"
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
		"access_token" : {
		  	"type" : "string"
			"description" : "The access token from Zainpal"
		}
	}

#### Good Response

Well now, The payment is confirmed. It's time to waite for your transaction to be confirmed.
If you send correct request then the response should be same thing like that.

	{
    		"status": 200,
    		"message": "Successful merchant"
	}

### Cancel Transaction

Users can cancel the transaction. Thwy should confirm the payment transaction in-app.


#### Cancel Endpoint

	https://api.zarinplus.com/payment/cancel

#### Body

Now please, Send `authority` and `access_token` back to cancel.
Send this parameters to the verify endpoint via `POST` method.

	{
		"authority" : "f72715fe8a1bfc8895cc5dac121931f696d00b61",
		"access_token" : "Zarinpalusertoken"
	}

#### Body Schema

This schema define the each parameter's type and value.

	{
		"authority" : {
			"type" : "string"
			"description" : "Authority key which you received from payment request"
		},
		"access_token" : {
		  	"type" : "string"
			"description" : "The access token from Zainpal"
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



### Wallet List

Shows the list of active wallets. 

#### Reverse Endpoint

	https://api.zarinplus.com/wallet/list

#### Body

Send `access_token` to the wallet list endpoint via `POST` method to see the list of active wallets.

	{
		"access_token" : "Zarinpalusertoken",
	}

#### Body Schema

This schema define the each parameter's type and value.

	{
		"access_token" : {
		  	"type" : "string"
			"description" : "The access token from Zainpal"
		}
	}	

#### Good Response

	{
	    "status": true,
	    "message": "successful",
	    "data": [
		{
		    "walletId": 1,
		    "name": "امتیاز",
		    "symbol": "EMZ",
		    "color": "FFB703",
		    "balance": 0.0
		}
	    ]
	}
	


## List of Status Codes

|  Code	    | Message       |
| ------------- |:-------------:|
| 400 | Invalid authority |
| 401 | Invalid token | 
| 402 | Invalid currency |
| 403 | Forbidden request |





## White-label
### Token

Gives you your token. 

#### Reverse Endpoint

	https://api.zarinplus.com/user/get-token

#### Body

Send `phone_number`, `first_name`, `last_name`, `national_code` and `client_id` to token endpoint via `POST` method in order to get your token.

	{
		"phone_number" : "989324567890",
		"first_name" : "YourFirstName",
		"last_name" : "YourLastName",
		"national_code" : "YourNationalCode",
		"client_id" : 1
		
	}

#### Body Schema

This schema define the each parameter's type and value.

	{
		"phone_number" : {
		  	"type" : "string"
			"description" : "Must start with 98 and is necessary"
		},
		"first_name" : {
		  	"type" : "string"
			"description" : "Your first name"
		},
		"last_name" : {
		  	"type" : "string"
			"description" : "Your last name"
		},
		"national_code" : {
		  	"type" : "string"
			"description" : "Your national code"
		},
		"client_id" : {
		  	"type" : "integer"
			"description" : "The client id"
		}
		
	}	

#### Good Response

	{
	    "status": true,
	    "message": "Successful",
	    "data": "Token [TOKEN]"
	}
	
	

### Collect

Collects data. 

#### Reverse Endpoint

	https://api.zarinplus.com/cards/collect

#### Body

Send `mask`, `hash`, `phone_number`, `source`, `merchant_id`, `session_id`, `amount` and `date` to collect endpoint via `GET` method in send data.

	{
		"mask" : "603770******6324",
		"hash" : "17DB16466F17A5A5946BFA3BBB4BB472B2259447AE57814AF15EFCFA94C4C83A",
		"phone_number" : "989324567890",
		"source" : "bargheman",
		"merchant_id" : 23,
		"session_id" : "23",
		"amount" : 1000.0,
		"date" : "2021-07-17"
	}

#### Body Schema

This schema define the each parameter's type and value.

	{
		"mask" : {
		  	"type" : "string"
			"description" : "The mask of the card number which is required"
		},
		"hash" : {
		  	"type" : "string"
			"description" : "The mask of the card number which is required"
		},
		"phone_number" : {
		  	"type" : "string"
			"description" : "Must start with 98 which is required"
		},
		"source" : {
		  	"type" : "string"
			"description" : "Source of data which is required"
		},
		"merchant_id" : {
		  	"type" : "integer"
			"description" : "The merchant id of the transaction which is optional"
		},
		"session_id" : {
		  	"type" : "string"
			"description" : "The session id of the transaction which is optional"
		},
		"amount" : {
		  	"type" : "float"
			"description" : "The amount of the transaction which is optional"
		},
		"merchant_id" : {
		  	"type" : "date"
			"description" : "2021-07-17 which is optional"
		},
		
	}	

#### Good Response

	{
	    "status": true,
	    "message": "Successful",
	    "data": ""
	}
	


## List of Status Codes

|  Code	    | Message       |
| ------------- |:-------------:|
| 400 | Invalid Phone Number |
| 403 | Forbidden request |

