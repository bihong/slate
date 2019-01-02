---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
 - errors

search: true
---

# Chope Partner API version 2

version 1: January 26th, 2018

version 2: Updated on December 6th, 2018

# Overview

Welcome to the Chope Partner Booking API! This is a quick starting guide to allow Chope partners and affiliates to utilize and implement Chope Booking API to create customize and native online booking widget for users.

The current Chope Booking API is organized around REST and JSON is returned by all API responses. Please keep in mind the Chope Booking API may change overtime as Chope continues to open more API endpoints for external consumption.

Currently Chope operate in 8 cities, please use the following `region_code` while working with each specific market. In all cities we support English as default language and the local language of each city. 

Cities | region_code | language 
------ | ----------- | --------
Singapore | SG | en_US
Hong Kong | HK | zh_HK
Jakarta | JAKARTA | id_ID
Bali | BALI | id_ID
Bangkok | BANGKOK | th_TH
Phuket | PHUKET | th_TH
Shanghai | SHANGHAI | zh_CN
Beijing | BEIJING | zh_CN

## Chope API endpoint

`https://chopeapi.chope.co/` (Production)

`http://chopeapi.chope.info/` or something else shared by Chope team (Sandbox)

# Authentication

> To authorize, use this code: 

```shell
# With shell, you can just pass the correct header with each request
curl 'api_enpoint_here' 
  -H 'Autorization: Bearer 6987ca1086f4e31474bc4a0bf291d4e2b7926c52'
```

> Make sure to replace `6987ca1086f4e31474bc4a0bf291d4e2b7926c52` with your API key to access the production data.

Chope API uses token-based verification in the header in a query. An access token will be assigned by Chope to our partners once commercial term has been signed. 

To perform test on Chope Sandbox environment, please use the following token: 

`Autorization: Bearer 6987ca1086f4e31474bc4a0bf291d4e2b7926c52`


# Prerequisite

Most reservation on Chope can be made simply by submitting diner's particulars, but some restaurants have special configurations that require additional information and process, for example: 

1. Restaurant with Customized Questionnaire (CQ)  
  This is question set up by the restaurant to diners, e.g. seating preference (in-door/outdoor) [optional or required] 

2. Restaurant with Deposit Requirement  
  Some restaurants have up-front deposit payment required to prevent no-show.

Please take extra notice when implementing the `bookings/create` API to cover all scenarios to prevent failed reservation.

# Flow
## Customer Facing Flow
![customer_facing_flow](customer_facing_flow.png)

- The above steps are to illustrate the Chope flow, UI and UX elements on each step can differ and can be rearranged.
- Depends on restaurant setting, additional fields such as CQ, checkbox, and event notes might be present.

## Back-end Flow
### New Booking
![API_Backends_Flow_Create_Booking](API_Backends_Flow_Create_Booking.png)

### Edit Booking
![API_Backends_Flow_Edit_Booking](API_Backends_Flow_Edit_Booking.png)

### Cancel Booking
![API_Backends_Flow_Cancel_Booking](API_Backends_Flow_Cancel_Booking.png)

# Response Format

> Success Response

```json
{
  "status":{
    "code": "200",
    "msg":"success messages"
  },
  "result":{
    ...
  }
}
```

> Error Response

```json
{
  "status":{
    "code": "400",
    "msg":"error messages"
  },
  "result":{
    ...
  }
}
```

Response are delivered in JSON, and may have the following fields:

- `status`: Response metadata. Always present.
  - `code`: error code.
  - `msg`: error messages.
- `result`: result that return from the endpoints.

 
# Booking API

## GET /bookings/calendar

Retrieve calendar information. The endpoint returns:

- Date availability for each day of the month from the specific `start_date` to the end of the month if `end_date` is not specified
- Restaurant date specific event notes for each date if it’s available
- The minimum and maximum number of acceptable adults (for the restaurant); maximum and minimum number of children is always 1 less than its adult counterpart
- Restaurant specific checkbox information

Call this endpoint when:

- User first enter the booking flow, retrieving date availability for current month.
- User switches from one month to another (forward and backward) using a calendar UI.

> Sample Request

```shell
curl 'http://chopeapi.chope.info/bookings/calendar?restaurant_id=sandboxreztype1&start_date=1543593600&end_date=1546185600&adults=2&children=0&language=en_US' \
-X GET \ 
-H 'Authorization: Bearer YOUR-TOKEN-CODE'
```

### Query Parameters

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
restaurant_id | string | yes | Unique Chope restaurant identifier.
start_date | timestamp | yes | Start date of the query time frame, in UNIX time.
end_date | timestamp | no | End date of the query time frame, in UNIX time. Default to last day of the month if empty.
region_code | string | no | Default to `SG` if empty.
adults | integer | no | Default to 2 if empty.
children | integer | no | Default to 0 if empty.
language | string | no | Default to `en_US` if empty.

> Sample Response (formatted)

```json
{
    "status": {
        "code": "200",
        "msg": "Success"
    },
    "result": {
        "timezone": "Asia/Singapore",
        "restaurant_config": {
            "restaurant_id": "sandboxreztype1",
            "restaurant_name": "Sandbox Restaurant Type 1",
            "max_party_size": "10",
            "min_party_size": "1",
            "in_out_door": 0,
            "special_notes": "Special request is not guarantee.",
            "custom_checkboxes": [
                {
                    "id": "211",
                    "restaurant_id": "sandboxreztype1",
                    "checked_by_default": true,
                    "required": false,
                    "context": "Please acknowledge that not all request can be fulfilled",
                    "alert_message": "",
                    "name": "sandboxChecked"
                },
                {
                    "id": "212",
                    "restaurant_id": "sandboxreztype1",
                    "checked_by_default": false,
                    "required": true,
                    "context": "Agree to term & condition.",
                    "alert_message": "You must agree to term & condition before proceed.",
                    "name": "sandboxUncheckedRequired"
                }
            ],
            "selected_adults": "2",
            "selected_children": "0"
        },
        "dates_list": {
            "1546272000": {
                "status": "past"
            },
            "1546358400": {
                "status": "past"
            },
            "1546444800": {
                "status": "open",
                "note": "Reservation Hours<br>10:30 am-6:15 pm",
                "waitlist": false
            },
            "1546531200": {
                "status": "open",
                "note": "Reservation Hours<br>10:30 am-6:15 pm",
                "waitlist": false
            },
            "1546617600": {
                "status": "full",
                "note": "This restaurant is no longer taking online reservations for today.",
                "waitlist": false
            },
            "1546704000": {
                "status": "closed"
            },
            "1546790400": {
                "status": "open",
                "note": "Reservation Hours<br>10:30 am-6:15 pm",
                "waitlist": false
            }
        }
    }
}
```

### Response Parameters

Parameter | Type | Description 
--------- | ---- | -----------
timezone | string | All information is provided based on “Asia/Singapore” time zone
restaurant_config ||
&nbsp;&nbsp; restaurant_id | string | Unique Chope restaurant identifier.
&nbsp;&nbsp; restaurant_name | string | Restaurant name.
&nbsp;&nbsp; max_party_size | integer | Upper limit of number of adults selectable.
&nbsp;&nbsp; min_party_size | integer | Lower limit of number of adults selectable. 
&nbsp;&nbsp; in_out_door | integer | In door/out door options available <br>0: not available <br>1: available
&nbsp;&nbsp; special_notes | string | Restaurant special note. This note needs to be displayed to the user prior to entering any reservation flow as it might includes important announcements from the restaurant.
&nbsp;&nbsp; custome_checkboxes ||
&nbsp;&nbsp;&nbsp;&nbsp; id | integer | Unique ID of the checkbox item. 
&nbsp;&nbsp;&nbsp;&nbsp; restaurant_id | string | Unique Chope restaurant identifier.
&nbsp;&nbsp;&nbsp;&nbsp; checked_by_default | boolean | true: (opt-out version) checkbox is checked by default. <br> false: (opt-in version) checkbox is empty by default. 
&nbsp;&nbsp;&nbsp;&nbsp; required | boolean | true: checkbox must be checked before proceed to reservation flow. <br> false: check is not mandatory.
&nbsp;&nbsp;&nbsp;&nbsp; context | string | Text content to be displayed next to checkbox.
&nbsp;&nbsp;&nbsp;&nbsp; alert_message | string | Alert message to be displayed if a mandatory checkbox is not checked.
&nbsp;&nbsp;&nbsp;&nbsp; name | string | Title of the checkbox item.  
&nbsp;&nbsp; selected_adults | integer | The selected number of adults. 
&nbsp;&nbsp; selected_children | integer | The selected number of children. 
date_list | timestamp | Date on the calendar in UNIX time.
&nbsp;&nbsp; status | string | Timeslot availability status of the date. <br> open: If there is at least one timeslot available.<br> close: If no timeslot is available at all. <br> past: Past date <br> full: Restaurant is no longer taking online reservation.
&nbsp;&nbsp; note | string | Restaurant note of the date, usually shows available sessions for online reservation.
&nbsp;&nbsp; event_note |string | Restaurant with special event note eg. Live-band available.
&nbsp;&nbsp; waitlist | boolean | Waitlist availability. <br>0: not available <br>1: available

## GET /bookings/timeslots

Retrieve availability. The endpoint will returns: 

- Time slots availability for the selected date and party size.
- Restaurant questionnaire, questionnaire response key and follow-up questions based on the response.

Call this endpoint when: 

- User adjusts the party size.
- User adjust the date.
- When attempt booking resulted in not available error, use this endpoint to retrieve the latest availability.

> Sample Request

```shell
curl 'http://chopeapi.chope.info/bookings/timeslots?restaurant_id=sandboxreztype1&date_time=1545980400&adults=5&children=1&language=en_US&region_code=SG'
-X GET  
-H 'Authorization: Bearer YOUR-TOKEN-CODE'
```

### Query Parameters
Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
restaurant_id | string | yes | Unique Chope restaurant identifier.
date_time | integer | yes | Specific booking date and time, in UNIX time.
adults | integer | no | Number of adults in the party. Default to `2` if empty.
children | integer | no | Number of children in the party. Default to `0` if empty.
langauage |string | no | Preferential language of the questionnaire. Default to `en_US` if empty.
region_code | string | no | Default to `SG` if empty.

> Sample Response (formatted)

```json
{
    "status": {
        "code": "200",
        "msg": "Success"
    },
    "result": {
        "status": "OPEN",
        "timeslots": [
            {
                "time": 1545953400,
                "waitlist": false,
                "capacity_limit": "2-6",
                "note": ""
            },
            {
                "time": 1545954300,
                "waitlist": false,
                "capacity_limit": "2-6",
                "note": ""
            },
            {
                "time": 1545955200,
                "waitlist": false,
                "capacity_limit": "2-6",
                "note": ""
            },
            {
                "time": 1545956100,
                "waitlist": false,
                "capacity_limit": "2-6",
                "note": ""
            }
        ],
        "selected_values": {
            "selected_adults": "5",
            "selected_children": "1",
            "selected_date_time": "1545980400"
        },
        "questionnaire": [
            {
                "id": "654",
                "answer_keyword": [
                    "checked"
                ],
                "question_text": "Do you need any baby assistance? ",
                "mandatory": "0",
                "input_type": "checkbox",
                "question_order": "1.00",
                "sub_questions": [
                    {
                        "parent_answer_trigger": "checked",
                        "id": "655",
                        "answer_keyword": [
                            "Highchair",
                            "Booster"
                        ],
                        "question_text": "What type?",
                        "mandatory": "0",
                        "input_type": "select",
                        "question_order": "3.00"
                    }
                ]
            },
            {
                "id": "653",
                "answer_keyword": [
                    "napkin(s)"
                ],
                "question_text": "*",
                "mandatory": "1",
                "input_type": "number",
                "question_order": "2.00"
            }
        ],
        "timezone": "Asia/Singapore"
    }
}
```

### Response Parameters
Parameter | Type | Description 
--------- | ---- | -----------
status | string | Availability status. <br> OPEN: available for booking. <br> CLOSE: Full or not available to accept online booking.
timeslots || 
&nbsp;&nbsp; time | timestamp | Time slot available on specific date for booking in UNIX time
&nbsp;&nbsp; wailtlist | boolean | Waitlist availability.
&nbsp;&nbsp; capacity_limit | string | Indicated the capacity fir this time slot if available.
&nbsp;&nbsp; note | string | Specific note for this time slot. 
selected_values ||
&nbsp;&nbsp;selected_adults | integer | Selected adult party size or default value = 2
&nbsp;&nbsp;selected_children | integer | Selected children party size or default value = 0
&nbsp;&nbsp;selected_date_time | integer | Selected date and time in UNIX time. 
questionnaire ||
&nbsp;&nbsp; id | integer | Unique ID for a single question. Being used as the key in a key-value pair to submit the answer using `bookings/create` endpoint.
&nbsp;&nbsp; answer_keyword | string | Unique keyword as answer. Being used as the value in a key-value pair to submit the answer using `bookings/create` endpoint. <br>Please refer to the “Type of `questionnaire.input_type`”
&nbsp;&nbsp; question_text | string | Question subject, for `input_type`=number, `question_text` is null.
&nbsp;&nbsp; mandatory | integer | 0: voluntary <br> 1: mandatory
&nbsp;&nbsp; input_type | string | Available options: checkbox, select, number.
&nbsp;&nbsp; question_order | string | Display order of the questionnaire.
&nbsp;&nbsp; sub_question ||
&nbsp;&nbsp;parent_answer_trigger | string | Child question that should be triggered when `parent_answer_trigger` = `questionnaire.answer_keyword`.


### Type of `questionnaire.input_type`

checkbox: 1 line of statement, usually requires user to check the box for acknowledgement. You must display a checkbox in front of the `question_text`. The answer should be represented in `answer_keyword` as "checked" or "null" for unchecked response.

select: Questionnaire with multiple selections that user must choose 1 answer. You must offer the `answer_keyword` as multiple choices to the user. The selected choice should be pass as the answer.

number: It stars with a statement "I need {number} `answer_keyword`". The {number} is a field for the user to input an integer to indicate the volume of `answer_keyword` that he needs for the reservation. Please pass the whole string "I need {number} `answer_keyword`" as the answer.



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

## POST /bookings/cancel

Use this endpoint to cancel existing reservation. 

- Upon successful cancel, the API returns booking reservation ID (unique) and booking confirmation code.

> Sample Request

```shell
curl 
'http://chopeapi.chope.info/bookings/cancel?reservation_id=7384847&restaurant_id=sandboxreztype1&membership_id=9308' 
-X POST 
-H 'Authorization: Bearer YOUR-TOKEN-CODE' 
```

### Query Parameters
Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
reservation_id | integer | yes | Unique Chope booking identifier.
restaurant_id | string | yes | Unique Chope restaurant identifier.
membership_id | string | yes | Partner’s unique user ID.

> Sample Response (formatted)

```json
{
    "status": {
        "code": "200",
        "msg": "Your reservation has been canceled."
    },
    "result": {
        "reservation_id": "9077092",
        "confirmation_id": "ABCD",
        "status": "CANCELLED"
    }
}
```

### Response Parameters
Parameter | Type | Description 
--------- | ---- | -----------
status | string | Booking status. <br> CANCELED: booking successfully canceled.
reservation_id | integer | Booking unique identifier. This value is used to retrieve booking information using `bookings/load` endpoint.
confrimation_id | string | Booking reference code that being shown to user.

<aside class="warning">While the reservation is placed real time, there is a queue process to sync up 3rd party membership data back to our master database which created a small time lag (production: less than 1 min ; sandbox: around 10 mins) before the reservation record can be canceled through “bookings/cancel” API. Please notice the limitation while performing test on sandbox environment. 
</aside>

## GET /bookings/load

Retrieve booking information. 

- This endpoint is used to retrieve a single booking information based on partner’s `membership_id` and `unique reservation_id`.
- It returns diner and booking information for a specific booking.

> Sample Request

```shell
curl 'http://chopeapi.chope.info/bookings/load?reservation_id=7384847&restaurant_id=sandboxreztype1&membership_id=9308'  
-X GET 
-H 'Authorization: Bearer TOKEN-CODE'
```

### Query Parameters
Parameter | Type | Required | Description
reservation_id | integer | yes | Unique Chope booking identifier.
restaurant_id | string | yes | Unique Chope restaurant identifier. 
membership_id | string | yes | Partner’s unique user ID.

> Sample Response (formatted)

```json
{
    "status": {
        "code": "200",
        "msg": "Success"
    },
    "result": {
        "rez_status": "CANCELED",
        "reservation_id": "7399961",
        "confirmation_id": "1B8Y",
        "title": "MR.",
        "first_name": "Su",
        "last_name": "Leon",
        "email": "suhang070@sina.cn",
        "phone_cc": "86",
        "mobile": "13321179308",
        "notes": "It's my birthday+tea+YES+no problem",
        "date_time": "1546228800",
        "adults": "5",
        "children": "1",
        "membership_id": "9308",
        "restaurant_name": "A leon restaurant MR test",
        "restaurant_id": "mr-10",
        "timezone": "Asia/Singapore",
        "questionnaire": ""
    }
}
```

### Response Parameters
Parameter | Type | Description 
--------- | ---- | -----------
rez_status | string | Booking status. <br> NEW: Reservation created.<br> CANCELED: Reservation cancelled. <br> EDITED: Reservation edited. <br> REJECTED: Reservation rejected by restaurant. 
reservation_id | integer | Unique Chope booking identifier
confirmation_id | string | Booking reference code. 
title | string | Title of the diner.
first_name | string | First name of the diner.
last_name | string | Last name of the diner.
email | string | Email address of the diner.
phone_cc | integer | Country code of the diner's mobile number.
mobile | integer | Mobile contact number of the diner.
notes | string | Diner special request for the booking.
date_time | timestamp | Booking date and time in UNIX time.
adults | integer | Number of adults in the party.
children | integer | Number of children in the party.
membership_id | string | Partner's unique user ID. 
restaurant_name | string | Restaurant Name. 
restaurant_id | string | Unique Chope restaurant identifier.

<aside class="warning">While the reservation is placed real time, there is a queue process to sync up 3rd party membership data back to our master database which created a small time lag (production: less than 1 min ; sandbox: around 10 mins) before the reservation record can be retrieved through “bookings/load” API. Please notice the limitation while performing test on sandbox environment. 
</aside>

# Callback Mechanism

> Sample Request Body (EDITED)

```json
{
        "reservation_id": "9077092",
        "status": "EDITED",
        "restaurant_name": "(test3a-4)Chope Test Restaurant",
        "restaurant_id": "test3a-4",
        "confirmation_id": "2PLEQ",
        "title": "Mr.",
        "first_name": "Zhao",
        "last_name": "Sam",
        "email": "sam@chope.com",
        "phone_cc": "65",
        "mobile": "96323155",
        "notes": "0",
        "date_time": "1517999400",
        "adults": "3",
        "children": "1",
        "membership_id": "123"
}
```

> Sample Request Body (CANCELED)

```json
{
        "reservation_id": "9077092",
        "status": "CANCELED",
        "restaurant_name": "",
        "restaurant_id": "",
        "confirmation_id": "",
        "title": "",
        "first_name": "",
        "last_name": "",
        "email": "",
        "phone_cc": "",
        "mobile": "",
        "notes": "",
        "date_time": "",
        "adults": "",
        "children": "",
        "membership_id": ""
}
```
> Please note that the callback contents for CANCELED will only includes `reservation_id`, other parameter will be empty.

POST “callback_url” that specified by the content provider when calling `/bookings/create` API would be triggered by Chope when the booking status changes.

- The mechanism is designed to post a single booking to the content provider based on partner’s membership_id, the booking’s unique reservation_id, and the restaurant_id once the booking updates (edit or cancel) by the other party.
- The API returns diner and booking information for a specific booking.

### Expected Response
Parameter | Type | Description 
--------- | ---- | -----------
status | string | Chope is expecting either “SUCCEED” or “FAILED” in the callback_url response. Chope would query again in the next life cycle that defined by Chope when “FAILED” or nothing returned. Chope would try and query this callback_url twice at the most.

# Restaurant API

## GET /restaurant/list

Retrieve restaurant list. Use this endpoint to retrieve a list of restaurants available on Chope Network. The API returns key information about the restaurants for listing purpose. 

> Sample Request

```shell
curl 'http://chopeapi.chope.info/restaurant/list?language=en_US&region_code=SG&offset=1&limit=2'  
-X GET 
-H 'Authorization: Bearer YOUR-TOKEN-CODE'
```

### Query Parameter
Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
country_code | string | no | Default to `SG` if empty.
language | string | no | Default to `en_US` if empty.
offset | integer | no | The offset to fetch results at.
limit | integer | no | The maximum number of results to return. If this parameter is not specified the API will return all results per call.

> Sample Response (formatted)

```json
{
    "status": {
        "code": "200",
        "msg": "Success"
    },
    "result": {
        "total_record": 166,
        "offset": 1,
        "limit": 2,
        "restaurants": [
            {
                "restaurant_id": "gia1603jkt",
                "restaurant_name": "7Adamssssss",
                "cuisine": "American EN, Chope-Dollars, Buffet, Modern European, Chinese, Japanese, Korean, Indian, Indonesian, Malaysian, Thai, Vietnamese, French, Italian, Spanish, Australian, Cuban, Fusion, International, Mediterranean, Middle Eastern, Moroccan, Seafood, Steakhouse, British, South American, Greek, Vegetarian Friendly, Local, German, Brazilian, Dim Sum, Southeast Asian, BTest",
                "latitude": "1.270785",
                "longitude": "103.824144",
                "address": "7 Adam Park <br>Singapore (289926)",
                "region_code": "SG",
                "postal_code": null,
                "profile_link": "https://www.chope.co/singapore-restaurants/7adam",
                "booking_url": "https://bookv5.chope.co/booking?rid=gia1603jkt&source=sandbox"
            },
            {
                "restaurant_id": "cleartest",
                "restaurant_name": "A handsome boy named Clear :)",
                "cuisine": "",
                "latitude": "22.285547",
                "longitude": "114.152096",
                "address": "81 Tras Street<br>Singapore (079020)",
                "region_code": "SG",
                "postal_code": null,
                "profile_link": "https://www.chope.co/singapore-restaurants/cleartest",
                "booking_url": "https://bookv5.chope.co/booking?rid=cleartest&source=sandbox"
            }
        ]
    }
}
```

### Response Parameters

Parameter | Type | Description 
--------- | ---- | -----------
total_record | integer | Total record of the list.
offset | integer | Selected offset to fetch results at.
limit | integer | Selected maximum number of results to return.
restaurants || 
&nbsp;&nbsp; restaurant_id | string | Unique Chope restaurant identifier.
&nbsp;&nbsp; restaurant_name | string | Unique Chope Restaurants name (English).
&nbsp;&nbsp; cuisine | string | Type of cuisine served in the restaurant.
&nbsp;&nbsp; latitude | string | Latitude of the restaurant.
&nbsp;&nbsp; longitude | string | Longitude of the restaurant.
&nbsp;&nbsp; address | string | Address of the restaurant with line break for formatting purpose. 
&nbsp;&nbsp; region_code | string | Region of the restaurant it operates in.
&nbsp;&nbsp; postal_code | integer | Postal code of the restaurant if available.
&nbsp;&nbsp; profile_link | string | Restaurant profile link url at Chope domain.
&nbsp;&nbsp; booking_url | string | Restaurant booking widget hosted at Chope domain.


## GET /restaurant

Retrieve information of a single restaurant based on unique `restaurant_id`. This endpoint returns key information of that particular restaurant.

> Sample Request

```shell
curl 'http://chopeapi.chope.info/restaurant?restaurant_id=sandboxreztype1'  
-X GET 
-H 'Authorization: Bearer TOKEN-CODE'
```

### Query Parameter
Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
restaurant_id | string | yes | Unique Chope restaurant identifier.

> Sample Response (formatted)

```json
{
    "status": {
        "code": "200",
        "msg": "Success"
    },
    "result": {
        "restaurant_name_en": "Yufengtest-en_US",
        "restaurant_name_cn": "Yufengtest-zh_CN",
        "address_en": "ENUS6 Tebing Lane #01-03,<br>Singapore (828835)",
        "address_cn": "ZHCN6 Tebing Lane #01-03,<br>Singapore (828835)",
        "image": "http://d2jzxcrnybzkkt.cloudfront.net/uploads/2015/03/logo_jpg_1426740171.jpg",
        "menu_link": "",
        "profile_link": "https://www.chope.co/singapore-restaurants/yufengtestuid",
        "latitude": "13.7443608",
        "longitude": "100.5543298",
        "phone": "+65 12356789"
    }
}
```

### Response Parameters
Parameter | Type | Description 
--------- | ---- | -----------
restaurant_name_en | string | Unique Chope Restaurants name (English).
restaurant_name_cn | string | Unique Chope Restaurants name (Chinese).
address_en | string | Restaurant address (English).
address_cn | string | Restaurant address (Chinese).
image | string | Restaurant image. 
menu_link | string | Restaurant menu link url.
profile_link | string | Restaurant profile link url at Chope domain. 
latitude | string | Latitude of the restaurant.
longitude | string | Longitude of the restaurant. 
phone | string | Phone number of the restaurant.

# Reference: Errors

Code | Messages | Description 
---- | -------- | -----------
A121 | We're sorry, but to prevent double bookings, until you cancel your reservation at {restaurant name} on {date} at {time}, this reservation cannot be confirmed for you. | If the same user (same membership_id) tried to make another booking at the same day.
A135 | We're sorry, but your requested time is unavailable. Please try an alternative time or contact the restaurant directly for more information. | Self-explanatory.
A156 | Sorry, you have to answer the question first | If the mandatory fields for questionnaire is not supplied in content request parameter.
A165 | Parameter error | Missing required parameter.
A176 | We're sorry, but your selected time is not available. Please select another time. | Self-explanatory.
A178 | Oops! We've detected multiple attempts to complete this reservation! Please do check your inbox or My Reservations tab to be sure that this reservation was saved correctly, and if it wasn't, please try again later. | Self-explanatory. 
A404 | No reservation found | Self-explanatory.


# Reference: Sandbox Restaurant for testing

We have set up a test restaurant for testing purpose:
`restaurant_id` = sandboxreztype1 

This restaurant has this following setting that can assist you in testing different scenarios.

- Make a booking of more than 3 adults to trigger deposit payment.
- Make a booking with children to trigger custom questionnaire.


# Utilizing Data Feed
For partner who would like to store restaurant availability on their own server instead of real-time fetching to improve user experience, you can choose the data feed integration method to cache the data instead of retrieving them real time through `bookings/calendar` and `bookings/timeslots` endpoints. Please reach out to Chope for proper system set up if you would like to explore such arrangement.

## Overview of the Integration Framework
![3rd Party Partnership with Feed Data](3rd_Party_Partnership_with_Feed_Data.png)

Key flows: 

1. Chope will generate all feed files every 5 hours and store them on AWS file storage.
2. Partner will retrieve those feed file and insert into their own server as cache data.
3. Restaurants availability changes from time to time. Whenever there are changes in Chope’s inventory, Chope will trigger an API call to partner endpoint to update the inventory.

## Feed Folder Structure

Feed data is categorized by country and store under folder with the following name: 

Folder Name | Country 
----------- | -------
SG | Singapore
ID | Indonesia
TH | Thailand

Please replace the placeholder `{country}` in the file path into respective folder name listed in the table.

## Restaurant Information Feed

This feed provides Chope's restaurant basic data.

### Base file

File path: `/chope/partners_dining/{country}_restaurant_information_feed_base.json`

This json file acts as a sitemap with the file path of the all availability feed file stored under this directory.

> Sample Feed

```xml
<sitemapindex xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
<sitemap>
    <loc>
    https://example.com/chope/partners_dining/TH_restaurant_information_feed_0_500.json
    </loc>
    <lastmod>2018-12-17T03:00:28+00:00</lastmod>
</sitemap>
<sitemap>
    <loc>
    https://example.com/chope/partners_dining/TH_restaurant_information_feed_500_500.json
    </loc>
    <lastmod>2018-12-17T03:00:29+00:00</lastmod>
</sitemap>
</sitemapindex>

```

Parameter | Description
--------- | -----------
loc | File path of the feed file.
lastmod | Last modification dates of the feed file. 


### Restaurant Information File
File path: `/chope/partners_dining/{country}_restaurant_information_feed_{offset}_500.json`

Each feed file holds information for 500 restaurants.

> Sample Feed

```json
"@type": "DataFeed",
"dateModified": "2018-12-17T03:00:02+00:00",
"dataFeedElement": 
[{
    "uid": "rubato8gghs6a-4",
    "name": "RUBATO Italian Restaurant & Wine Bar",
    "description": "A modern Italian trattoria in Greenwood",
    "geo": {
        "latitude": "1.33125",
        "longitude": "103.8069763"
    },
    "address": {
        "addressLocality": "Singapore",
        "addressRegion": "Singapore",
        "postalCode": "289204",
        "streetAddress": "12 Greenwood Avenue",
        "addressCountry": "SG"
    },
    "telephone": "+6562523200"
},{
    "uid": "skyve5tthka-4",
    "name": "Skyve Wine Bistro",
    "description": "A delightful break from the ordinary at this Newton hideout",
    "geo": {
        "latitude": "1.3098446",
        "longitude": "103.8413358"
    },
    "address": {
        "addressLocality": "Singapore",
        "addressRegion": "Singapore",
        "postalCode": "227977",
        "streetAddress": "10 Winstedt Road, Blk E #01-17",
        "addressCountry": "SG"
    },
    "telephone": "+6562256690"
    }
}]
```

Parameter | Description
--------- | -----------
uid | Unique Chope restaurant identifier.
name | Restaurant Name.
description | Short Description about the restaurant.
geo |
&nbsp;&nbsp; latitude | Latitude of the restaurant.
&nbsp;&nbsp; longitude | Longitude of the restaurant.
address |
&nbsp;&nbsp; addressLocality | The city where the restaurant located.
&nbsp;&nbsp; addressRegion | The region where the restaurant located.
&nbsp;&nbsp; postalCode | The postal code for the restaurant.
&nbsp;&nbsp; streetAddress | The street address of the restaurant.
&nbsp;&nbsp; addressCountry | The country where the restaurant located.
telephone | Telephone number of the restaurant with country code.

## Restaurant Table Availability Feed

This feed provides availability of the restaurant for the next 30 days.

### Base file

File path: `/chope/partners_dining/{country}_restaurant_table_availability_feed_base.json`

This json file acts as a sitemap with the file path of the all availability feed file stored under this directory.

> Sample Feed

```xml
<sitemapindex xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
<sitemap>
    <loc>
    https://example.com/chope/partners_dining/SG_restaurant_information_feed_0_50.json
    </loc>
    <lastmod>2018-12-17T05:00:09+00:00</lastmod>
</sitemap>
<sitemap>
    <loc>
    https://example.com/chope/partners_dining/SG_restaurant_information_feed_50_50.json
    </loc>
    <lastmod>2018-12-17T05:00:16+00:00</lastmod>
</sitemap>
```

Parameter | Description
--------- | -----------
loc | File path of the feed file.
lastmod | Last modification dates of the feed file. 

### Availability File

File path: `/chope/partners_dining/{country}_restaurant_table_availability_feed_{offset}_50.json`

Each feed file holds availability of 50 restaurant for the next 30 days. 

> Sample Feed

```json
{
            "context":"ctrip_availability_data_feed",
            "type":"DataFeed",
            "dateModified":"2018-11-30T10:11:30+07:00",
            "dataFeedElement":[
            {
                        "rid":"attitude1712bkk",
                        "date":"2018-11-30",
                        "itemListElement":[{
                               "partySize":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15],
                               "startTime":"2018-11-30T18:00:00+07:00", 
                               "endTime":"2018-11-30T20:00:00+07:00"
                            },{
                               "partySize":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15],
                               "startTime":"2018-11-30T18:15:00+07:00", 
                               "endTime":"2018-11-30T20:15:00+07:00"
                            },{
                               "partySize":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15],
                               "startTime":"2018-11-30T18:30:00+07:00", 
                               "endTime":"2018-11-30T20:30:00+07:00"
                            },{
                               "partySize":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15],
                               "startTime":"2018-11-30T18:45:00+07:00", 
                               "endTime":"2018-11-30T20:45:00+07:00"
                            },{
                               "partySize":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15],
                               "startTime":"2018-11-30T19:00:00+07:00", 
                               "endTime":"2018-11-30T21:00:00+07:00"
                            },{
                               "partySize":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15],
                               "startTime":"2018-11-30T21:45:00+07:00", 
                               "endTime":"2018-11-30T23:45:00+07:00"
                            }]
                    },{
                        "rid":"attitude1712bkk",
                        "date":"2018-12-01",
                        "itemListElement":[{
                               "partySize":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15],
                               "startTime":"2018-12-01T18:00:00+07:00", 
                               "endTime":"2018-12-01T20:00:00+07:00"
                            },{
                               "partySize":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15],
                               "startTime":"2018-12-01T18:15:00+07:00", 
                               "endTime":"2018-12-01T20:15:00+07:00"
                            },{
                               "partySize":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15],
                               "startTime":"2018-12-01T18:30:00+07:00", 
                               "endTime":"2018-12-01T20:30:00+07:00"
                            },{
                               "partySize":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15],
                               "startTime":"2018-12-01T18:45:00+07:00", 
                               "endTime":"2018-12-01T20:45:00+07:00"
                            },{
                               "partySize":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15],
                               "startTime":"2018-12-01T19:00:00+07:00", 
                               "endTime":"2018-12-01T21:00:00+07:00"
                            },{
                               "partySize":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15],
                               "startTime":"2018-12-01T19:15:00+07:00", 
                               "endTime":"2018-12-01T21:15:00+07:00"
                            },{
                               "partySize":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15],
                               "startTime":"2018-12-01T21:45:00+07:00", 
                               "endTime":"2018-12-01T23:45:00+07:00"
                            }]
}
```

Parameter | Description
--------- | -----------
dateModified | Last modification dates of the feed file.
rid | Unique Chope restaurant identifier.
date | The date available for reservation.
itemListElement |
&nbsp;&nbsp; partySize | The party size that is available for the specific start time.
&nbsp;&nbsp; startTime | The starting time of the available reservation time. This is the reservable time being shown to the user.
&nbsp;&nbsp; endTime | The ending time of the reservation, usually 2 hours from the starting time. Usually this information is hidden from the user unless restaurant has strict time restriction. 

## Real time feed data update

> Sample Request

```shell
curl “http://YourAPIURL” 
-d ‘{
    "date":"2018-12-11",
    "rid":"froth1507fro",
    "sign":"2153c877054aeb75535e4255881fb48c",
    "itemListElement":[{
        "partySize":[2,4,6],
        "startTime":"2018-12-11T11:30:00+08:00",
        "endTime":"2018-12-11T13:30:00+08:00"
    },{
        "partySize":[2,4,6],
        "startTime":"2018-12-11T11:45:00+08:00",
        "endTime":"2018-12-11T13:45:00+08:00"
    },{
        "partySize":[2,4,6],
        "startTime":"2018-12-11T15:45:00+08:00",
        "endTime":"2018-12-11T17:45:00+08:00"
    }]
}'
```

Partner need to prepare an API endpoint to receive real time data update from Chope.
This is a RESTful POST method in JSON format ('Content-Type: application/json')

The data format is similar with the table availability feed.

We suggest partners to add a “sign” parameter in the API for access control purposes.

