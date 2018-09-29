![Emtiyaz Logo](https://static.emtiyaz.app/logo/png/logo-color-blacktxt-fa-small.png)

# Emtiyaz Documentation

[![MIT licensed](https://img.shields.io/badge/license-MIT-blue.svg)](./license)

## Table of Contents

* [Payment](#payment)
	* [Create Payment Request](#payment-request)
	* [Verify Transaction](#verify-transaction)
* [Callback](#callback)
	* [Setup Callback Via Javascript](#callback-via-javascript)
	* [Setup Callback Via P2P](#callback-via-p2p)


<a name="payment"></a>
## Payment

This part of document is for merchants (Who accept payments via points), If you would like to create payment requests from your server and verify the transactions, Please read this document to implement the payment via our payment API.

<a name="payment-request"></a>
### Create Payment Request
Get your {Private Token} from your partner merchant account and replace it in URL.

#### Request Endpoint

	https://api.emtiyaz.app/{Private Token}/payment/request.json
	

#### Body

Send these parameters to the endpoint via GET or POST request.

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

This schema define the each parameter's values and type.

	{
		"currency" : {
			"type" : "string",
			"description" : "Currency is required, Those currency codes are supported IRR, IRT, POT, USD"
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
			"description" : "User cellphone"
		},
		"email" : {
			"type" : "string"
			"description" : "User email address"
		}
	}
  
#### Good Response

If you send correct request then the response should be same thing like that.

	{
		"status" : "200",
		"message" : "Get the authority code",
		"authority" : "f72705fe8a9bfc8895cc5dac121931f696d00b61"
	}

#### Redirect to emtiyaz

Now you should build the URL by replacing {authority} with the authority you received from previous step and then redirect to offerwall.

Also you must store the authority value for this transaction in your database for next actions.

	https://offerwall.emtiyaz.app/?authority={authority}


<a name="verify-transaction"></a>
### Verify Transaction

After the users make the payment with their points we redirect them to the success URL you previously provided to us.
Now you should verfiy the payment transaction.

	http://www.mystore.com/success/?authority={authority}

#### Verify Endpoint

	https://api.emtiyaz.app/{Private Token}/payment/verify.json

#### Body

Now please find the transaction infomations by authority in your database and send them back for verfiy.
Send these parameters to the endpoint via GET or POST request.

	{
		"currency" : "IRR",
		"amount" : 50000,
		"authority" : "f72705fe8a9bfc8895cc5dac121931f696d00b61"
	}

#### Body Schema

This schema define the each parameter's values and type.

	{
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

#### Good Response

Well now, the payment in complete and verified.
If you send correct request then the response should be same thing like that.

	{
		"status" : "100",
		"message" : "Successful"
	}

<a name="callback"></a>
## Callback
This part of document is for bidders (Advertisers), Then users complete your offer you should immediately inform us the conversion.

<a name="callback-via-javascript"></a>
### Setup Javascript Tag

Easily just add this javascript tag in to your landing page.

#### Javascript Tag

	<script type="text/javascript" src="https://static.emtiyaz.app/js/tracking.js"></script>

#### Javascript Callback On Load

Call the emtiyaz_callback() method when a conversion happened, For example when (/thankyou.html) thank for your purchase page loaded.

**_Attention:Callback via Javascript is not secure._**

	<script type="text/javascript" src="https://static.emtiyaz.app/js/tracking.js"></script>
	<script type="text/javascript">
	window.onload = function() {
		emtiyaz_callback('');
	};
	</script>

#### Javascript Callback On click

Call the emtiyaz_callback() method when a conversion happened, For example when user click on _Register_ button.

**_Attention:Callback via Javascript is not secure._**


	<script type="text/javascript" src="https://static.emtiyaz.app/js/tracking.js"></script>
	<button onclick="emtiyaz_callback('')">Register</button>

<a name="callback-via-p2p"></a>
### Callback Via API Endpoint

Callback via API endpoint is too secure, We recommend implement this method instead of javascript callback.

#### API Endpoint

Get your {Private Token} from your partner account _(User settings)_ and replace it in URL.

	https://callback.emtiyaz.app/{Private Token}

#### Body

We add **mt_click_id** parameter to your offer's landing page via GET method please send it back to our endpoint.

**_Attention: You can store mt_click_id value to their users cookie by youself or just add javascript tag to your landing page_**

Callback these parameters to the endpoint via GET or POST request When conversion happened.

	{
		"mt_click_id" : "80ee0fd84e32442122d68ce9bd3df1454f577a97",
		"mt_ip" : "46.209.239.50",
		"mt_event" : "New User"
	}

#### Body Schema

	{
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

#### Good Response

Well now the user get the point immediately from you.
If you send correct request then the response should be same thing like that.

	{
		"status" : "200",
		"message" : "Successful"
	}

#### Bad Response

	{
		"status" : "400",
		"message" : "Invalid"
	}


