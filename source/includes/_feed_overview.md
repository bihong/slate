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
Please replace the `/chope/` folder to `/chopetest/` to access the feed data for sandbox environment.
For the full filepath, please reach out to your Chope's contact.