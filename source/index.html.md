---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='https://developer.chope.co/login/logout'>Log Out</a>
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
 - restaurant_deal
 - restaurant_nearby
 - restaurant
 - errors
 - feed_overview
 - information_feed
 - availability_feed
 - feed_update

search: true
---

# Chope Partner Bookings API 

Important Update: 25th June 2019

We have upgraded our sandbox environment with mirrored production data. Please make sure to update the domain for any new developement. 

# Overview

Welcome to the Chope Partner Booking API! This is a quick starting guide to allow Chope partners and affiliates to utilize and implement Chope Booking API to create customize and native online booking widget for users.

The current Chope Booking API is organized around REST and JSON is returned by all API responses. Please keep in mind the Chope Booking API may change overtime as Chope continues to open more API endpoints for external consumption.

# Chope API endpoint

##Production
`https://chopeapi.chope.co/` (outside China)

`https://chopeapi.chope.net.cn/` (inside China excluding HongKong)

##Sandbox
`https://chopeapi.qiubaoman.com/` (outside China)

`https://chopeapi.dingzhuo8.biz/` (inside China excluding HongKong)

# Authentication

> To authorize, use this code: 

```shell
# With shell, you can just pass the correct header with each request
curl 'api_enpoint_here' 
  -H 'Autorization: Bearer 1234ca567f4e31474ef4a0bf29z4e2b890c11'
```

> Make sure to replace `1234ca567f4e31474ef4a0bf29z4e2b890c11` with your API key to access the production data.

Chope API uses token-based verification in the header. An access token will be assigned by Chope to partners once commercial term has been signed. 

To perform test on Chope Sandbox environment, please [reach out to us](mailto:product-team@chope.co) for arrangement. 

# Market Availability

Chope operates in 8 cities, please use the following `region_code` while working with each specific market. In all cities we support English as default language and the local language of each city. 

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

# Important Notes

## Restaurant with Special Setup
Most reservation on Chope can be made simply by submitting diner's particulars, but some restaurants have special configurations that require additional information and process, for example: 

1. Restaurant with custom checkboxes to obtain users' consent  
  The content may be restaurant's terms or special notes e.g. dress code etc. 

2. Restaurant with Customized Questionnaire (CQ)  
  This is question set up by the restaurant to diners, e.g. seating preference (in-door/outdoor) [optional or required] 

3. Restaurant with Deposit Requirement  
  Some restaurants have up-front deposit payment required to prevent no-show.

Please take extra notice when implementing the booking flows and calling `bookings/create` API to cover all scenarios to prevent failed reservation.

## Email Communication with Users
By default, Chope will send out Chope branded booking confirmation and reminder email to users. We provide 2 options for partners:

1. Co-branded email manage by Chope based on standard template

2. Co-branded white label email manage by partners 

Please indicate your choice of user communication preference in the commercial term.

### Type of Emails
1. Confirmation email upon successful booking
2. Reminder email (within 12am-5am on the reservation day)
3. Edited reservation 
4. Cancelled reservation


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