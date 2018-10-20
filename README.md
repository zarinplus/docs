![Emtiyaz Logo](https://static.emtiyaz.app/logo/png/logo-color-blacktxt-fa-small.png)

# Emtiyaz Documentation

[![MIT licensed](https://img.shields.io/badge/license-MIT-blue.svg)](./license)

## Table of Contents

* [Payment](#payment)
	* [Create Payment Button Via Javascript SDK](#create-payment-button-via-javascript-sdk)
	* [Create Payment Request Via API](#create-payment-request-via-api)
	* [Verify Transaction Via API](#verify-transaction-via-api)
* [Callback](#callback)
	* [Callback Via Javascript SDK](#callback-via-javascript-sdk)
	* [Callback Via API Endpoint](#callback-via-api-endpoint)
	* [Callback Via Mobile Attribution Platforms](#callback-via-mobile-attribution-platforms)


## Payment

This part of document is for merchants _(Who accept payments via points)_, If you would like to create payment requests from your server and verify the transactions, Please read this document to understand how to implement payment via request endpoint.

### Create Payment Button Via Javascript SDK
Get your `{public token}` from your merchant account and replace it in the javascript.

#### Javascript Tag

	<script type="text/javascript" src="https://static.emtiyaz.app/js/button.js"></script>

#### Javascript Setup Button

Create a new object from `emtiyaz_button()`, Please visit [Live Demo](https://emtiyaz-app.github.io/samplecodes/javascript/payment/checkout-popup.html).

	<script type="text/javascript" src="https://static.emtiyaz.app/js/button.js"></script>
	<script>
	    var button_popup = new emtiyaz_button('button_popup');
	    button_popup.public_token = '{public token}';
	    button_popup.success_url = 'https://emtiyaz-app.github.io/samplecodes/javascript/payment/success.html';
	    button_popup.cancel_url = 'https://emtiyaz-app.github.io/samplecodes/javascript/payment/cancel.html';
	    button_popup.amount = 5000;
	    button_popup.currency = 'IRR';
	    button_popup.cellphone = 989124966428;
	    button_popup.email = 'pooya@emtiyaz.app';
	    button_popup.item = 'Voucher 50000 IRR';
	    button_popup.width = 380;
	    button_popup.height = 680;
	    button_popup.text = 'Buy Now!'
	    button_popup.show();
	</script>

### Create Payment Request Via API
Get your `{private token}` from your merchant account and replace it in the request endpoint. 

#### Request Endpoint

	https://api.emtiyaz.app/{private token}/payment/request.json
	

#### Body

Send these parameters to the request endpoint via `GET` or `POST` method.

	{
		"currency" : "IRR",
		"amount" : 50000,
		"cancel" : "http://www.mystore.com/cancel/",
		"success" : "http://www.mystore.com/success/",
		"item" : "5000 Toman Voucher",
		"cellphone" : 989121111111,
		"email" : "name@email.com"
	}

#### Body Schema

This schema define the each parameter's type and value.

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
			"description" : "buyer cellphone"
		},
		"email" : {
			"type" : "string"
			"description" : "buyer email address"
		}
	}
  
#### Good Response

If you send correct request then the response should be same thing like that, Please watch [Sample Codes](https://github.com/emtiyaz-app/samplecodes/tree/master/php/payment).

	{
		"status" : "200",
		"message" : "Get the authority code",
		"authority" : "f72705fe8a9bfc8895cc5dac121931f696d00b61"
	}

#### Redirect To Offerwall

Now you should build the offerwall's URL by replacing `{authority}` with the authority you received from previous step and then redirect to offerwall.

Also you must store the authority value for this transaction in your database for next action.

	https://offerwall.emtiyaz.app/?authority={authority}


### Verify Transaction Via API

After the buyers make the payments with their points, We redirect them to the `success` URL you previously provided to us.
Now you should verfiy the payment transaction.

	http://www.mystore.com/success/?authority={authority}

#### Verify Endpoint

	https://api.emtiyaz.app/{private token}/payment/verify.json

#### Body

Now please, Send `authority` back for verfiy.
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

Well now, The payment is complete and verified, It's time to deliver your goods or services to buyer.
If you send correct request then the response should be same thing like that, Please watch [Sample Codes](https://github.com/emtiyaz-app/samplecodes/tree/master/php/payment).

	{
		"status" : "200",
		"message" : "Successful",
		"amount" : "50000",
		"currency" : "IRR",
		"item" : "5000 Toman Voucher",
		"email" : "name@email.com",
		"cellphone" : "989121111111"
	}

## Callback
This part of document is for bidders _(Advertisers)_, When users complete your offer you should immediately inform us that a conversion happened.

### Callback Via Javascript SDK

Easily put this javascript tag to your landing page.

#### Javascript Tag

	<script type="text/javascript" src="https://static.emtiyaz.app/js/tracking.js"></script>

#### Javascript Callback On Page Load

Call `emtiyaz_callback()` method when a conversion happened by visiting a web page, _For example_ when `/thankyou.html` page visited.

_Attention: Callback via Javascript is not secure._

	<script type="text/javascript" src="https://static.emtiyaz.app/js/tracking.js"></script>
	<script type="text/javascript">
	window.onload = function() {
		emtiyaz_callback('Order');
	};
	</script>

#### Javascript Callback On click

Call `emtiyaz_callback()` method when a conversion happened by clicking on a button, _For example_ when user click on _Register_ button, Please watch [Sample Codes](https://github.com/emtiyaz-app/samplecodes/tree/master/javascript/callback).

_Attention: Callback via Javascript is not secure._

	<script type="text/javascript" src="https://static.emtiyaz.app/js/tracking.js"></script>
	<button onclick="emtiyaz_callback('register')">Register</button>

### Callback Via API Endpoint

Callback via API endpoint is too secure, We recommend implement this method instead of javascript callback.

#### API Endpoint

Get your `{private token}` from your partner account _(User settings)_ and replace it in URL.

	https://callback.emtiyaz.app/{private token}

#### Body

We add `emtiyaz_click` parameter to your offer's landing page via `GET` method you should send it back to API endpoint.

_Attention: You should store emtiyaz_click value to their users cookie by yourself or just add the javascript tag to your landing page._

Read these parameters from users cookie and then send them to the API endpoint via `GET` or `POST` method.

	{
		"emtiyaz_click" : "80ee0fd84e32442122d68ce9bd3df1454f577a97",
		"emtiyaz_ip" : "46.209.239.50",
		"emtiyaz_event" : "Order"
	}

#### Body Schema

This schema define the each parameter's type and value.

	{
		"emtiyaz_click" : {
			"type" : "string",
			"description" : "Click identity, The parameter emtiyaz_click is required"
		},
		"emtiyaz_ip" : {
			"type" : "string",
			"description" : "The client IP address"
		},
		"emtiyaz_event" : {
			"type" : "string",
			"description" : "The event type, like Register, Order, Install"
		},
		"emtiyaz_ios_idfa" : {
			"type" : "string",
			"description" : "IOS device ID"
		},
		"emtiyaz_google_ad_id" : {
			"type" : "string",
			"description" : "Google advertising ID"
		},
		"emtiyaz_android_device_id" : {
			"type" : "string",
			"description" : "Android device ID"
		}
	}

#### Good Response

Well now, The user get the points immediately from you.
If you send correct request then the response should be same thing like that, Please watch [Sample Codes](https://github.com/emtiyaz-app/samplecodes/tree/master/php/callback).

	{
		"status" : "200",
		"message" : "Successful"
	}

#### Bad Response

	{
		"status" : "400",
		"message" : "Invalid"
	}

### Callback Via Mobile Attribution Platforms 

Emtiyaz is integrated and listed as partner with [Adjust](https://www.adjust.com/technology-partners/), [Tapstream](https://tapstream.com/ad-networks/), [Tune](https://help.tune.com/marketing-console/integrated-ad-networks-and-publishers/). Using mobile attribution platforms are strongly recommended for tracking app install event.


You just need use your campaign url as your offer url to track app install event, Mobile attribution platforms are automatically run our callback API endpoint to inform us a conversion.

