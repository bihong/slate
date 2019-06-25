## GET /restaurant/deal

Retrieve restaurant list with cash voucher. Use this endpoint to retrieve a list of restaurants available on ChopeDeal Network. The API returns key information about the restaurants for listing purpose. 

> Sample Request

```shell
curl 'http://chopeapi.chope.info/restaurant/deal?language=en_US&region_code=SG&offset=1&limit=2'  
-X GET 
-H 'Authorization: Bearer YOUR-TOKEN-CODE'
```

### Query Parameter
Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
region_code | string | no | currently only `SG` & `HK` market support ChopeDeal.
language | string | no | Default to `en_US` if empty.
offset | integer | no | The offset to fetch results at.
limit | integer | no | The maximum number of results to return. Default to 100 if empty.

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
                "booking_url": "https://bookv5.chope.co/booking?rid=gia1603jkt&source=sandbox",
                "shopify_url": "https://shop.chope.co/products/abcd"
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
                "booking_url": "https://bookv5.chope.co/booking?rid=cleartest&source=sandbox",
                "shopify_url": "https://shop.chope.co/products/efgh"
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
&nbsp;&nbsp; shopify_url | string | URL to purchase voucher on ChopeDeal e-commerce site.