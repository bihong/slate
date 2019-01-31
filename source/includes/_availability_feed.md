## Restaurant Table Availability Feed

This feed provides availability of the restaurant for the next 30 days.

### Base file

File path: `/chope/partners_dining/{country}/{country}_restaurant_table_availability_feed_base.json`

This json file acts as a sitemap with the file path of the all availability feed file stored under this directory.
Availability feed file is updated every 5 hours.

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

<aside class="notice">Partner can monitor the "lastmod" value in the base file and trigger an update process when the timestamp changes.
</aside>

### Availability File

File path: `/chope/partners_dining/{country}/{country}_restaurant_table_availability_feed_{offset}_50.json`

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