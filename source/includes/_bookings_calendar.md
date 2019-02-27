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

<aside class="notice">Use the correct combination of <code>region_code</code> and <code>language</code> to retrieve contents in local languge. Wrong combination will return contents in en_US by default.
</aside>

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

<aside class="warning">You MUST implement <code>custome_checkboxes</code> with <code>required</code> value equal true - as this is extremely important for restuarant with certain terms and privacy policy that users must consent in order to make an online reservation.
</aside>