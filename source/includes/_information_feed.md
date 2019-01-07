## Restaurant Information Feed

This feed provides Chope's restaurant basic data.

### Base file

File path: `/chope/partners_dining/{country}/{country}_restaurant_information_feed_base.json`

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
File path: `/chope/partners_dining/{country}/{country}_restaurant_information_feed_{offset}_500.json`

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