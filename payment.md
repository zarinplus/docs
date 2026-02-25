![Zarinplus Logo](https://www.zarinplus.com/wp-content/uploads/2024/02/Logo-1.png)

## Zarinplus  Docs 

Please read this document to understand how to implement payment via request endpoint.

## Table of Contents

* [Payment](#payment)
	* [Create Payment Request](#create-payment-request)
	* [Cancel Transaction](#cancel-transaction)
	* [Verify Transaction](#verify-transaction)
	* [Reverse Transaction](#reverse-transaction)
	* [List of Status Codes](#list-of-status-codes)


## Payment
## Create Payment Request
To make a request transaction merchant should use this APIs. 

#### Request Endpoint

	https://api.zarinplus.com/payment/v2/request/
	

#### Body

Send these parameters to the request endpoint via `POST` method.

	{
		"amount" : 50000,
		"cancel" : "http://www.mystore.com/cancel/",
		"success" : "http://www.mystore.com/success/",
		"item" : "5000 Toman Voucher",
		"cellphone" : "09121111111",
		"email" : "yourname@domain.com",
		"token" : "9a1bfc8895cc5df72715fe81f6ac121936d00b61",
		"gateway: "zarinplus"
	}

#### Body Schema

This schema define the each parameter's type and value.

	{
		"amount" : {
			"type" : "int"
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
			"description" : "Buyer cellphone(optional)"
		},
		"email" : {
			"type" : "string"
			"description" : "Buyer email address(optional)"
		},
		"token" : {
			"type" : "string"
			"description" : "Merchant Token is required"
		},
		"gateway": {
			"type": "string"
			"description": "default is zarinplus (optional)"
		}
	}
  
#### Good Response

If you send correct request then the response should be same thing like that.

	{
		"status" : true,
		"message" : "Get the authority code",
		"authority" : "f72705fe8a9bfc8895cc5dac121931f696d00b61",
  		"redirect_ur": "https://pwa.zarinplus.com/authority=f72705fe8a9bfc8895cc5dac121931f696d00b61&phone=09121111111"
	}


 ### After get `authority` for process and payment, you must direct the user to the link below
 #### https://pwa.zarinplus.com/authority={`authority`}


## Cancel Transaction

Users can cancel the transaction. Thwy should confirm the payment transaction in-app.


#### Cancel Endpoint

	https://api.zarinplus.com/payment/v2/cancel/
 
#### Body

Now please, Send `authority` back to cancel.
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
    		"status": true,
    		"message": "You have canceled transaction"
	}

## Verify Transaction

Merchant should verify transaction after passed by user.

#### Verify Endpoint

	https://api.zarinplus.com/payment/v2/verify/

#### Body

Send this parameters to the verify endpoint via `POST` method.

	{
		"authority" : "f72715fe81f69a1bfc8895cc5dac121936d00b61",
		"token" : "9a1bfc8895cc5df72715fe81f6ac121936d00b61",
  		"amount": 50000
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


```json
{
    "status": true,
    "message": "Successful",
    "data": {
        "code": 200,
        "message": "Successful",
        "amount": 50000,
        "currency": "IRR",
        "reference": "512c773635c2a716340ce70bdddad25ace806378",
        "cellphone": "09226521257",
        "user_cellphone": "989337679420"
    }
}
```

### 🔴 In this API, it is necessary to check response['data']['code'], because if an authority is verified again, the code number will change to 201, and you must be careful to only make the user's factor successful and final on the number 200. Unless in special circumstances, the implementation is also necessary on the number 201.

## Reverse Transaction

You can reverse any successful transaction. 

#### Reverse Endpoint

	https://api.zarinplus.com/payment/v2/reverse/

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

```json
{
    "status": true,
    "message": "success",
    "data": {
        "code": 101,
        "message": "Reversed",
        "amount": 1000000,
        "currency": "IRR",
        "item": "Item description",
        "email": "example@example.com",
        "cellphone": "09226521257"
    }
}
```


## List of Status Codes

|  Code	    | Message       |
| ------------- |:-------------:|
| 400 | Invalid authority |
| 401 | Invalid token | 
| 403 | Forbidden request |


