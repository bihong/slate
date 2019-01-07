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