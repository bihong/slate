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