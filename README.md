![Emtiyaz Logo](https://static.emtiyaz.app/logo/png/logo-color-blacktxt-fa-small.png)

# Emtiyaz Documentation

[![MIT licensed](https://img.shields.io/badge/license-MIT-blue.svg)](./license)

### _Please_, feel free to make any contributions you feel will make Emtiyaz Documentation better.

**Submit all pull requests to the develop branch**

**Before your pull request can be merged into the develop branch, you must submit a completed CLA.**

## Table of Contents

* [Payment](#payment)
	* [Request](#payment-request)
	* [Verify](#payment-verify)
* [Callback](#Callback)
	* [Client Side](#callback-client-side)
	* [Server Side](#callback-server-side)



<a name="payment"></a>
## payment
If you would like to create payment requests from your server and confirm the transactions, Please use this document to implement the payment via our payment API.

### Create Payment Request

#### API Request Endpoint

	https://api.emtiyaz.app/{Merchant Token}/payment/request.json
	

#### API Request Body

	{
		"currency" : "IRR",
		"amount" : 50000,
		"cancel" : "http://www.mystore.com/cancel/",
		"success" : "http://www.mystore.com/success/",
		"item" : "Description about item",
		"cellphone" : 989121111111,
		"email" : "name@email.com"
	}

#### API Request Body Schema

	{
	    "type" : "object",
	    "properties" : {
	      "currency" : {
		  "type" : "string",
		  "description" : "Those currency codes are supported IRR, IRT, POT, USD"
	      },
	      "amount" : {
		  "type" : "float"
	      },
	      "cancel" : {
		  "type" : "string"
	      },
	      "success" : {
		  "type" : "string"
	      },
		"item" : {
		  "type" : "string"
	      },
		"cellphone" : {
		  "type" : "integer"
	      },
		"email" : {
		  "type" : "string"
	      }
	    }
	}
  
#### API Request Good Response
  
	{
		"status" : "200",
		"message" : "Get the authority code",
		"authority" : "f72705fe8a9bfc8895cc5dac121931f696d00b61"
	}

#### Redirect to emtiyaz
	https://emtiyaz.app/?authority={authority}

#### API Request Bad Response

	{
		"status" : "400",
		"message" : "Invalid token"
	}
