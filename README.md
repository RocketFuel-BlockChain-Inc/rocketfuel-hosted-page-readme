
  

# RocketFuel Hosted Page

  

Follow the guide to embed RocketFuel Hosted Page on the Merchant's website.

  
### API Endpoints are as follows:-
1. prod: `https://app.rocketfuelblockchain.com/api`,
2. preprod: `https://preprod-app.rocketdemo.net/api`,
  

# Prerequistes:-

  

Merchant should send below JSON input details to display the same on RocketFuel hosted page

  

    Sample Format of JSON Data-
    {
    
	    amount: "11.00",
	    merchant_id: process.env.MERCHANT_ID,
	    cart: [
	    { 
		    id: "23", 
		    name: "Album",
		    price: "11",
		    quantity: "1",
	    },
	    ],
	    currency: "USD",
	    order: "390",
	    redirectUrl: "",   
	   }

where

  

1. Amount - total price of whole order for payment, in USD only

2. Cart -

a. ID - Article unique Id

b. Name - Article Name

c. Price - Article price

d. Quantity - Article Quantity

3. Currency - Currency in which Merchant recieved the payment, in USD only

4. Order - Unique Order Id

5. RedirectUrl - URL of the Merchant site where the Merchant wants to redirect from the Hosted page to their site after payment.

  
### Follow the steps below :-

  

1. Merchant need to be authenticated on RocketFuel by passing the input parameters - MERCHANT_EMAIL and MERCHANT_PASSWORD.

  

**Request**

    var options = {
	    method: "POST",
	    url: process.env.API_ENDPOINT + "auth/login",
	    headers: {
		    "Content-Type": "application/json",
	    },
	    body: JSON.stringify({
		    email: process.env.MERCHANT_EMAIL,
		    password: process.env.MERCHANT_PASS,
	    }),
    };

  

  

**Response**

    { "ok":true, "result"{ "access":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpX
    VCJ9.eyJpZCI6ImViMDE1NWU4 LTkwNzItNGMyYi05NWFjLTAxZDhiMWFlZDI3ZCI
    sImlhdCI6MTYyMzQwMz g0OSwiZXhwIjoxNjIzNDkwMjQ5fQ.t1wL6LYkr8y5sauC
    uOWMmGbGNDZH qXzUjo6WeT370c","refresh":"eyJhbGciOiJIUzI1NiIsInR5c
    CI6IkpXVCJ9.eyJpZC I6ImViMDE1NWU4LTkwNzItNGMyYi05NWFjLTAxZDhiMWFl
    ZDI3ZCIsImlhd CI6MTYyMzQwMzg0OSwiZXhwIjoxNjI1OTk1ODQ5fQ.A_JF1ODtc
    RCRc7Yn TcT5JBFDDMkgQtlQXYkhBFw3dgM", "status":2 }

  
  

2. Once Merchant is verified, details of the items purchased with the access token will be sent in a different request.

Note - Token needs to send in the Header and details of the items purchased in the Body.

  

**Request**

    var options = {
	    method: "POST",
	    url: process.env.API_ENDPOINT + "hosted-page",
	    headers: {
		    authorization: "Bearer " + accessToken,
		    "Content-Type": "application/json",
		},
	    body: JSON.stringify({
		    amount: "11.00",
		    merchant_id: process.env.MERCHANT_ID,
		    cart: [
		    {
			    id: "23",
			    name: "Album",
			    price: "11",
			    quantity: "1",
		    },
	    ],
	    currency: "USD",
	    order: "390",
	    redirectUrl: "",
		    }),
	    };

  

**Response** -

    {
    
    "ok":true,
    
    "result":{"url":"[https://dev.rocketdemo.net/hostedPage/f7cb4141-
    3030-4245-8aa1-](https://dev.rocketdemo.net/hostedPage/f7cb4141-3
    030-4245-8aa1-)[f5cf95b5d504](https://dev.rocketdemo.net/hostedPa
    ge/f7cb4141-3030-4245-8aa1-f5cf95b5d504)"}
    
    }

  
  

3. Once Merchant received the response as a True, then redirect the Merchant site to the URL received in the response.

  

  

***On click of [Pay for your order with RocketFuel] paste below Code Snippet.***

  

**Code Snippet--**

  

  

    const request=require('request');
    var options = {
	    method: "POST",
	    url: process.env.API_ENDPOINT + "auth/login",
	    headers: {
		    "Content-Type": "application/json",
	    },
	    body: JSON.stringify({
		    email: process.env.MERCHANT_EMAIL,
		    password: process.env.MERCHANT_PASS,
	    }),
    };
    
    request(options, function (error, response) {
    
      if (error) throw new Error(error);
	  let accessToken = JSON.parse(response.body).result.access;
	  //place the order API Call
    
	    var options = {
		    method: "POST",
		    url: process.env.API_ENDPOINT + "hosted-page",
		    headers: {
			    authorization: "Bearer " + accessToken,
			    "Content-Type": "application/json",
		    },    
		    body: JSON.stringify({
		    amount: "11.00",
		    merchant_id: process.env.MERCHANT_ID,
		    cart: [
			    {
				    id: "23",
				    name: "Album",
				    price: "11",
				    quantity: "1",
			    },
		    ],
		  currency: "USD",
		  order: "390",
		  redirectUrl: "",
	    }),
    
    };
   
    request(options, function (error, response) {
    
	    if (error) throw new Error(error);
	    let resp = JSON.parse(response.body);
	    res.redirect(resp.result.url);    
	    });
    
    });




