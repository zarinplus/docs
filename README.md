![Emtiyaz Logo](https://static.emtiyaz.app/logo/png/logo-color-blacktxt-fa-small.png)

# Emtiyaz Documentation

[![MIT licensed](https://img.shields.io/badge/license-MIT-blue.svg)](./license)

### _Please_, feel free to make any contributions you feel will make Emtiyaz Documentation better.

**Submit all pull requests to the develop branch**

**Before your pull request can be merged into the develop branch, you must submit a completed CLA.**

## Table of Contents

* [Payment](#payment)
	* [Create Payment Request](#payment-request)
	* [Verify Transaction](#verify-transaction)
* [Callback](#Callback)
	* [Setup Callback Via Javascript](#callback-via-javascript)
	* [Setup Callback Via P2P](#callback-via-p2p)


<a name="payment"></a>
## Payment
If you would like to create payment requests from your server and confirm the transactions, Please use this document to implement the payment via our payment API.

<a name="payment-request"></a>
### Create Payment Request

#### Request Endpoint

	https://api.emtiyaz.app/{Merchant Token}/payment/request.json
	

#### Body

	{
		"currency" : "IRR",
		"amount" : 50000,
		"cancel" : "http://www.mystore.com/cancel/",
		"success" : "http://www.mystore.com/success/",
		"item" : "Description about item",
		"cellphone" : 989121111111,
		"email" : "name@email.com"
	}

#### Body Schema

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
  
#### Good Response
  
	{
		"status" : "200",
		"message" : "Get the authority code",
		"authority" : "f72705fe8a9bfc8895cc5dac121931f696d00b61"
	}

#### Redirect to emtiyaz
	https://emtiyaz.app/?authority={authority}

#### Bad Response

	{
		"status" : "400",
		"message" : "Invalid token"
	}


<a name="verify-transaction"></a>
### Verify Transaction

We redirect to your success or cancel URL 

	http://www.mystore.com/success/?authority={authority}

#### Verify Endpoint

	https://api.emtiyaz.app/{Merchant Token}/payment/verify.json

#### Body

	{
		"currency" : "IRR",
		"amount" : 50000,
		"authority" : "f72705fe8a9bfc8895cc5dac121931f696d00b61"
	}

#### Body Schema

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
	      "authority" : {
		  "type" : "string"
	      }
	    }
	}

#### Good Response

	{
		"status" : "100",
		"message" : "Successful"
	}


<a name="callback"></a>
## Callback
Then users complate your offer emeditel you should give them the points.

<a name="callback-via-javascript"></a>
### Setup Callback Via Javascript
Fast and simple just add this javascript tag in to your web pages.

#### Javascript Tag:

	<script type="text/javascript">
		var emtiyaz_data = emtiyaz_data || [];
		emtiyaz_data['notification'] = true;
	</script>
	
	<script type="text/javascript" src="https://static.emtiyaz.app/js/tracking.js"></script>

#### Javascript Callback Onload

	<script type="text/javascript" src="https://static.emtiyaz.app/js/tracking.js"></script>
	
	<script type="text/javascript">
	window.onload = function() {
	  emtiyaz_callback('New User');
	};
	</script>

#### Javascript Callback Onclick

	<script type="text/javascript" src="https://static.emtiyaz.app/js/tracking.js"></script>
	<button onclick="emtiyaz_callback('register')">Register</button>

<a name="callback-via-p2p"></a>
### Setup Callback Via P2P

#### API Endpoint
	https://callback.emtiyaz.app/{token}

#### Body

	    {
		"mt_click_id" : "80ee0fd84e32442122d68ce9bd3df1454f577a97",
		"mt_ip" : "46.209.239.50",
		"mt_event" : "New User"
	    }

#### Body Schema

	  {
	    "type" : "object",
	    "properties" : {
	      "mt_click_id" : {
		  "type" : "string",
		  "description" : "Click identity is required"
	      },
	      "mt_ip" : {
		  "type" : "string",
		  "description" : "The client IP address"
	      },
	      "mt_event" : {
		 "type" : "string",
		 "description" : "Keyword about the event happened, like New-user, Order, Install"
	      }
	    }
	  }

#### Good Response

	  {
	      "status" : "200",
	      "message" : "Successful"
	  }

#### Bad Response

	  {
	      "status" : "400",
	      "message" : "Invalid"
	  }

