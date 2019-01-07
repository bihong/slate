---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
 - response
 - bookings_calendar
 - bookings_timeslots
 - bookings_create
 - bookings_edit
 - bookings_cancel
 - bookings_load
 - callback
 - restaurant_list
 - restaurant
 - errors
 - sandbox
 - feed_overview
 - information_feed
 - availability_feed
 - feed_update

search: true
---

# Chope Partner API version 2

version 1: January 26th, 2018

version 2: Updated on December 6th, 2018

# Overview

Welcome to the Chope Partner Booking API! This is a quick starting guide to allow Chope partners and affiliates to utilize and implement Chope Booking API to create customize and native online booking widget for users.

The current Chope Booking API is organized around REST and JSON is returned by all API responses. Please keep in mind the Chope Booking API may change overtime as Chope continues to open more API endpoints for external consumption.

Currently Chope operate in 8 cities, please use the following `region_code` while working with each specific market. In all cities we support English as default language and the local language of each city. 

Cities | region_code | language 
------ | ----------- | --------
Singapore | SG | en_US
Hong Kong | HK | zh_HK
Jakarta | JAKARTA | id_ID
Bali | BALI | id_ID
Bangkok | BANGKOK | th_TH
Phuket | PHUKET | th_TH
Shanghai | SHANGHAI | zh_CN
Beijing | BEIJING | zh_CN

# Chope API endpoint

`https://chopeapi.chope.co/` (Production)

`http://chopeapi.chope.info/` or something else shared by Chope team (Sandbox)

# Authentication

> To authorize, use this code: 

```shell
# With shell, you can just pass the correct header with each request
curl 'api_enpoint_here' 
  -H 'Autorization: Bearer 6987ca1086f4e31474bc4a0bf291d4e2b7926c52'
```

> Make sure to replace `6987ca1086f4e31474bc4a0bf291d4e2b7926c52` with your API key to access the production data.

Chope API uses token-based verification in the header in a query. An access token will be assigned by Chope to our partners once commercial term has been signed. 

To perform test on Chope Sandbox environment, please use the following token: 

`Autorization: Bearer 6987ca1086f4e31474bc4a0bf291d4e2b7926c52`


# Prerequisite

Most reservation on Chope can be made simply by submitting diner's particulars, but some restaurants have special configurations that require additional information and process, for example: 

1. Restaurant with Customized Questionnaire (CQ)  
  This is question set up by the restaurant to diners, e.g. seating preference (in-door/outdoor) [optional or required] 

2. Restaurant with Deposit Requirement  
  Some restaurants have up-front deposit payment required to prevent no-show.

Please take extra notice when implementing the `bookings/create` API to cover all scenarios to prevent failed reservation.

# Flow
## Customer Facing Flow
![customer_facing_flow](customer_facing_flow.png)

- The above steps are to illustrate the Chope flow, UI and UX elements on each step can differ and can be rearranged.
- Depends on restaurant setting, additional fields such as CQ, checkbox, and event notes might be present.

## Back-end Flow
### New Booking
![API_Backends_Flow_Create_Booking](API_Backends_Flow_Create_Booking.png)

### Edit Booking
![API_Backends_Flow_Edit_Booking](API_Backends_Flow_Edit_Booking.png)

### Cancel Booking
![API_Backends_Flow_Cancel_Booking](API_Backends_Flow_Cancel_Booking.png)