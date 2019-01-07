## POST /bookings/edit

Use this endpoint to edit existing reservation. 

- Upon successful edit, the API returns booking reservation ID (unique) and booking confirmation code.
- If `callback_url` is present, booking information will be post back to partner once the booking is updated.

> Sample Request

```shell
curl 
'http://chopeapi.chope.info/bookings/edit?restaurant_id=sandboxreztype1&language=en_US&region_code=SG&date_time=1546001100&title=MR.&first_name=XXX&last_name=YYY&phone_cc=65&mobile=12345678&email=xxx@gmail.com&adults=5&children=1&notes="It's my birthday&content"={"199":"tea"}&membership_id=9308&reservation_id=7384847&callback_url=http://leon.chope.co/' 
-X POST 
-H 'Authorization: Bearer TOKEN-CODE' 
```

### Query Parameters
Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
reservation_id | integer | yes | Unique Chope booking identifier.
restaurant_id | integer | yes | Unique Chope restaurant identifier.
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
        "msg": "Your reservation has been successfully updated!"
    },
    "result": {
        "reservation_id": "9077092",
        "confirmation_id": "ABCD",
        "status": "EDITED",
    }
}
```

### Response Parameters
Parameter | Type | Description 
--------- | ---- | -----------
status | string | Booking status. <br> EDITED: booking successfully updated. <br> Pending: booking is pending for deposit payment.  
reservation_id | integer | Booking unique identifier. This value is used to retrieve booking information using `bookings/load` endpoint.
confrimation_id | string | Booking reference code that being shown to user. 


### Edit Booking for a restaurant with deposit payment

Some restaurant only impose deposit payment requirement for party size above a certain threshold, while editing the booking, if there are changes in party size that crossed the threshold, the system will response with the payment widget URL (refer to Restaurant with Deposit Requirement) , user need to complete the payment before the changes are being committed and reflected. 

> Sample Response (formatted)

```json
{
    "status": {
        "code": "200",
        "msg": "Your reservation update is pending. Please pay the deposit in 10 minutes."
    },
    "result": {
        "status": "PENDING",
        "pay_url": "http://bookv5.chope.cc/pay/get_rez?code=1BHQ&telephone=13321179308&phone-area= 86&rid=mr-10&source=sandbox&membership_id=9308",
        "reservation_id": "9077092",
        "confirmation_id": "ABCD",
        
    }
}
```

<aside class="warning">While the reservation is placed real time, there is a queue process to sync up 3rd party membership data back to our master database which created a small time lag (production: less than 1 min ; sandbox: around 10 mins) before the reservation record can be edited through “bookings/edit” API. Please notice the limitation while performing test on sandbox environment. 
</aside>