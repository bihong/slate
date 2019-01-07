## POST /bookings/create

Use this endpoint to create a new reservation. 

- The endpoint returns booking reservation ID (unique) and booking confirmation code for a successful reservation. 
- If `callback_url` is present. Booking information and status will be postback to partner when the booking is modified by others.

> Sample Request

```shell
curl 
'http://chopeapi.chope.info/bookings/create?restaurant_id=sandboxreztype1&language=en_US&date_time=1546002900&title=MR.&first_name=XXX&last_name=YYY&phone_cc=65&mobile=12345678&email=xxx@gmail.com&adults=6&children=1&notes=Birthday celebration&content={"653":"I need 10 napkins","654":"checked","655":"Highchair"}&membership_id=9309&callback_url=http://test.chope.co/&agree_receive_email=1' 
-X POST 
-H 'Authorization: Bearer YOUR-TOKEN-CODE' 
```

### Query Parameters
Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
restaurant_id | string | yes | Unique Chope restaurant identifier.
date_time | timestamp | yes | Booking date and time in UNIX time. 
title | string | no | Title of the diner. Possible value: Mr., Miss, Ms., Mrs., Dr. Can be null.
last_name | string | yes | Last name of the diner.
first_name | string | no | First name of the diner. 
adults | integer | yes | Number of adults in the party.
children | integer | no | Number of children in the party.
email | string | yes | Email address of the diner. 
phone_cc | integer | yes | Country code of the diner's mobile number.
mobile | integer | yes | Mobile contact number of the diner. 
notes | string | no | Diner special request for the booking. Can be null.
membership_id | string | yes | Partner's unique user ID. Chope API will return error if this value is misaligned with `membership_id` stored in Chope system for future booking update queries. 
content | json | yes | Questionnaire answer in key-value set. <br>Sample: {"7":"Indoors","8":"bar seats"}. Required only if Questionnaire is mandatory. 
callback_url | string | no | URL that Chope call to post information back to partner.
language | string | no | This setting will impact the language used in any future communication with users including booking confirmation email or reminder email. Default to `en_US` if empty.
agree_receive_email | integer | no | Default value is “1” where user agrees to receive promotional email from Chope. Use “0” to indicate the opposite.

> Sample Response (formatted)

```json
{
    "status": {
        "code": "200",
        "msg": "Congratulations, your reservation is now confirmed!"
    },
    "result": {
        "status": "NEW",
        "reservation_id": 7384871,
        "confirmation_id": "10KQ"
    }
}

```

### Response Parameters
Parameter | Type | Description 
--------- | ---- | -----------
status | string | Booking status. <br> New: booking successfully created. <br> Pending: booking is pending for deposit payment.  
reservation_id | integer | Booking unique identifier. This value is used to retrieve booking information using `bookings/load` endpoint.
confrimation_id | string | Booking reference code that being shown to user. 

> Sample Response (formatted)

```json
{
    "status": {
        "code": "200",
        "msg": "You have to pay the deposit in 10 minutes"
    },
    "result": {
        "status": "PENDING",
        "pay_url": "http://bookv5.chope.info/pay/get_rez?code=10KV&telephone=86080958&phone-area=65&rid=sandboxreztype1&source=sandbox&membership_id=9309",
        "reservation_id": 7384876,
        "confirmation_id": "10KV"
    }
}
```

### Restaurant with Deposit Requirement

For a restaurant with deposit required, you would receive a separate set of response with URL link to the payment widget in order for the user to process their payment, the seat will be held for 10 minutes until the payment is made. 

### Response Parameters
Parameter | Type | Description 
--------- | ---- | -----------
msg | string | A message indicating that deposit need to be paid in 10 minutes.
pay_url | string | URL to payment widget that you should redirect your user to. 

Your should redirect user to make the deposit payment at the `pay_url`, the sample payment site looks like: 
![sample_payment_site](sample_payment_site.png)

If `callback_url` is present, booking information will be post back to partner upon status changes either successful reservation with deposit paid (`status` = NEW) or cancellation without payment (`status` = CANCELLED)