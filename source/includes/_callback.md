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