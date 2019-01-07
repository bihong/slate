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