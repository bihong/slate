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
--------- | ---- | -------- | -----------
reservation_id | integer | yes | Unique Chope booking identifier.
restaurant_id | string | yes | Unique Chope restaurant identifier. 
membership_id | string | yes | Partner’s unique user ID.

<aside class="notice">
Although <code>membership_id</code> is in the string type, remember to encode for special character when making the request
</aside>

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
questionnaire | string | Depreciated.

<aside class="warning">While the reservation is placed real time, there is a queue process to sync up 3rd party membership data back to our master database which created a small time lag (production: less than 1 min ; sandbox: around 10 mins) before the reservation record can be retrieved through “bookings/load” API. Please notice the limitation while performing test on sandbox environment. 
</aside>