---
layout: guides
title: Giving feedback to an order
categories: 
- Listing
- Real Estate
- Manage Listings
- Orders
- Manage Questions
- Notifications
menu: 
- Listing &amp; Selling
tags: 
- Orders
---

##Overview {#overview}

After completing a sale and a purchase, according to MercadoLibre’s rules, each part has to provide feedback about the transaction (an order).
This feedback says if the order was fulfilled and the seller and buyer rate each other respectively.
Seller’s and buyer’s score on MercadoLibre platform is calculated based on these ratings.

<div class="contents">
  <h5>Table of Contents</h5>
  <dl>
    <dt><a href="javascript:void(0)" onClick="goToByScroll('parameters')">Valid Parameters</a></dt>
    <dt><a href="javascript:void(0)" onClick="goToByScroll('possible-reasons')">Possible values for reason</a></dt>
    <dt><a href="javascript:void(0)" onClick="goToByScroll('post-feedback-seller')">Post feedback as a Seller</a></dt>
    <dt><a href="javascript:void(0)" onClick="goToByScroll('post-feedback-buyer')">Post feedback as a Buyer</a></dt>
    <dt><a href="javascript:void(0)" onClick="goToByScroll('reply-feedback')">Reply on feedback you received</a></dt>
    <dt><a href="javascript:void(0)" onClick="goToByScroll('change-feedback')">Changing a previous feedback</a></dt>
  </dl>
</div>

##Parameters {#parameters}
- `fulfilled` — If the transaction was fulfilled. Paid, shipped and accepted by the buyer. Must be true or false. (REQUIRED)
- `rating` —  Rating given to the other party. Can be negative, neutral or positive. (REQUIRED)
- `reason` — Reason for giving negative rating. Only accepted when rating is negative.  (REQUIRED if fulfilled=false)
- `message` — A free text that adds to the rating. Maximum 160 characters. (REQUIRED)

## Possible Reasons for negative rating {#possible-reasons}
* THEY_DIDNT_ANSWER
* I_COULDNT_ANSWER  (only for buyers)
* DESCRIPTION_DIDNT_MATCH_ARTICLE (only for buyers)
* BUYER_REGRETS
* SELLER_REGRETS
* SELLER_OUT_OF_STOCK,
* SELLER_DIDNT_TRY_TO_CONTACT_BUYER
* BUYER_NOT_ENOUGH_MONEY
* THEY_NOT_HONORING_POLICIES
* OTHER_MY_RESPONSIBILITY
* OTHER_THEIR_RESPONSIBILITY



## POST a feedback (seller) {#post-feedback-seller}
Posting a positive feedback after a seller ships a product to the buyer.

<pre class="terminal">
curl -X POST -H "Content-Type: application/json" -d
'{
  "rating":"positive",  
  "fulfilled":true, 
  "message":"The product was paid in time and shipped to the buyer."
}'

https://api.mercadolibre.com/orders/$ORDER_ID/feedback?access_token=$ACCESS_TOKEN  
</pre>

###Valid response

<pre class="terminal">
 HTTP/1.1 201 Created
 Date: Tue, 08 May 2012 19:23:28 GMT
 Content-Type: application/json;charset=UTF-8
 X-MLAPI-Version: 1.9.0
</pre>



##POST feedback (buyer) {#post-feedback-buyer}
Suppose the product size is not what the buyer wanted, thus he rates negative the transaction.

<pre class="terminal">
curl -X POST -H "Content-Type: application/json" -d
'{
  "rating":"NEGATIVE",
  "fulfilled":false,
  "reason": "DESCRIPTION_DIDNT_MATCH_ARTICLE",
  "message":"The product is not what I expected. It is too small."
}'

https://api.mercadolibre.com/orders/$ORDER_ID/feedback?access_token=$ACCESS_TOKEN  
</pre>

##Reply a feedback {#reply-feedback}

You can reply to some feedback that you received from the other part, to explain your reasons or give additional information to the other part.

A reply is a POST request to this URL:


<pre class="terminal">
curl -X POST -H "Content-Type: application/json" -d
'{
  "rating":"NEGATIVE",
  "fulfilled":false,
  "reason": "DESCRIPTION_DIDNT_MATCH_ARTICLE",
  "message":"The product is not what I expected. It is too small."
}'

https://api.mercadolibre.com/orders/$ORDER_ID/feedback/$EXPERIENCE
</pre>



###IMPORTANT NOTE:
Experience can be: **sale** or **purchase**, depending on what is the other part ROLE. Because you are replying to some feedback you received previously.

EXAMPLES:

- As a **SELLER** you can send a POST request to a purchase feedback:


<pre class="terminal">
curl -X POST -H "Content-Type: application/json" -d
'{
  "reply": "The size was detailed in the description, however if you send it back we can refund you the money." 
}'

https://api.mercadolibre.com/orders/$ORDER_ID/feedback/purchase?access_token={aSellersAccessToken}
</pre>


- As a **BUYER** you can send a POST request to a sale feedback:

<pre class="terminal">
curl -X POST -H "Content-Type: application/json" -d
'{
  "reply": "I expected you to send me the correct size from the start." 
}'

https://api.mercadolibre.com/orders/$ORDER_ID/feedback/sale?access_token={aBuyersAccessToken}
</pre>


##Changing a feedback {#change-feedback}

You can change a feedback if you change your mind.

Example: Upon receiving the money back from the seller you decide to change a negative feedback to neutral.

A buyer should send a PUT request like this:

<pre class="terminal">
curl -X PUT -H "Content-Type: application/json" -d
'{
  "rating":"NEUTRAL","fulfilled":false,"message":"I received the money back"
}'

https://api.mercadolibre.com/orders/$ORDER_ID/feedback/purchase?access_token={aBuyersAccessToken}
</pre>

A seller, on the contrary should send a PUT request to:

<pre class="terminal">
curl -X PUT -H "Content-Type: application/json" -d
'{
  "rating":"NEUTRAL","fulfilled":false,"message":"OK I will change the feedback"
}'

https://api.mercadolibre.com/orders/$ORDER_ID/feedback/sale?access_token={aSellersAccessToken}
</pre>
