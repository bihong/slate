## GET /bookings/timeslots

Retrieve availability. The endpoint will returns: 

- Time slots availability for the selected date and party size.
- Restaurant questionnaire, questionnaire response key and follow-up questions based on the response.

Call this endpoint when: 

- User adjusts the party size.
- User adjust the date.
- When attempt booking resulted in not available error, use this endpoint to retrieve the latest availability.

> Sample Request

```shell
curl 'http://chopeapi.chope.info/bookings/timeslots?restaurant_id=sandboxreztype1&date_time=1545980400&adults=5&children=1&language=en_US&region_code=SG'
-X GET  
-H 'Authorization: Bearer YOUR-TOKEN-CODE'
```

### Query Parameters
Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
restaurant_id | string | yes | Unique Chope restaurant identifier.
date_time | integer | yes | Specific booking date and time, in UNIX time.
adults | integer | no | Number of adults in the party. Default to `2` if empty.
children | integer | no | Number of children in the party. Default to `0` if empty.
langauage |string | no | Preferential language of the questionnaire. Default to `en_US` if empty.
region_code | string | no | Default to `SG` if empty.

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
        "status": "OPEN",
        "timeslots": [
            {
                "time": 1545953400,
                "waitlist": false,
                "note": ""
            },
            {
                "time": 1545954300,
                "waitlist": false,
                "note": ""
            },
            {
                "time": 1545955200,
                "waitlist": false,
                "note": ""
            },
            {
                "time": 1545956100,
                "waitlist": false,
                "note": ""
            }
        ],
        "selected_values": {
            "selected_adults": "5",
            "selected_children": "1",
            "selected_date_time": "1545980400"
        },
        "questionnaire": [
            {
                "id": "654",
                "answer_keyword": [
                    "checked"
                ],
                "question_text": "Do you need any baby assistance? ",
                "mandatory": "0",
                "input_type": "checkbox",
                "question_order": "1.00",
                "sub_questions": [
                    {
                        "parent_answer_trigger": "checked",
                        "id": "655",
                        "answer_keyword": [
                            "Highchair",
                            "Booster"
                        ],
                        "question_text": "What type?",
                        "mandatory": "0",
                        "input_type": "select",
                        "question_order": "3.00"
                    }
                ]
            },
            {
                "id": "653",
                "answer_keyword": [
                    "napkin(s)"
                ],
                "question_text": "*",
                "mandatory": "1",
                "input_type": "number",
                "question_order": "2.00"
            }
        ],
        "timezone": "Asia/Singapore"
    }
}
```

### Response Parameters
Parameter | Type | Description 
--------- | ---- | -----------
status | string | Availability status. <br> OPEN: available for booking. <br> CLOSE: Full or not available to accept online booking.
timeslots || 
&nbsp;&nbsp; time | timestamp | Time slot available on specific date for booking in UNIX time.
&nbsp;&nbsp; wailtlist | boolean | Waitlist availability.
&nbsp;&nbsp; ~~capacity_limit~~ | ~~string~~ | ~~Indicated the capacity for this time slot if available.~~ **Depreciated**
&nbsp;&nbsp; note | string | Specific note for this time slot. 
selected_values ||
&nbsp;&nbsp;selected_adults | integer | Selected adult party size or default value = 2
&nbsp;&nbsp;selected_children | integer | Selected children party size or default value = 0
&nbsp;&nbsp;selected_date_time | integer | Selected date and time in UNIX time. 
questionnaire ||
&nbsp;&nbsp; id | integer | Unique ID for a single question. Being used as the key in a key-value pair to submit the answer using `bookings/create` endpoint.
&nbsp;&nbsp; answer_keyword | string | Unique keyword as answer. Being used as the value in a key-value pair to submit the answer using `bookings/create` endpoint. <br>Please refer to the “Type of `questionnaire.input_type`”
&nbsp;&nbsp; question_text | string | Question subject, for `input_type`=number, `question_text` is null.
&nbsp;&nbsp; mandatory | integer | 0: voluntary <br> 1: mandatory
&nbsp;&nbsp; input_type | string | Available options: checkbox, select, number.
&nbsp;&nbsp; question_order | string | Display order of the questionnaire.
&nbsp;&nbsp; sub_question ||
&nbsp;&nbsp;parent_answer_trigger | string | Child question that should be triggered when `parent_answer_trigger` = `questionnaire.answer_keyword`.


### Type of `questionnaire.input_type`
![Sample_cq](sample_cq.png)

**checkbox**: 1 line of statement, usually requires user to check the box for acknowledgement. You must display a checkbox in front of the `question_text`. The answer should be represented in `answer_keyword` as "checked" or "null" for unchecked response. 
Refer to (1) in the image where the restaurant is asking if the diner needs a baby assistance, in this case, `question_text` return the text "Do you need any baby. assistance?".

**select**: Questionnaire with multiple selections that user must choose 1 answer. You must offer the `answer_keyword` as multiple choices to the user. The selected choice should be pass as the answer.
Refer to (2) in the image where the restaurant is asking what type of baby assistance diner needs, in this case, `question_text` return the text "What type?", `answer_keyword` return the available selection as array ["Highchair" , "Booster"].

**number**: It stars with a statement "I need {number} `answer_keyword`". The {number} is a field for the user to input an integer to indicate the volume of `answer_keyword` that he needs for the reservation. Please pass the whole string "I need {number} `answer_keyword`" as the answer.
Refer to (3) in the image where the restaurant is asking how many napkins diner needs, in this case, `answer_keyword` return the text "napkin(s)". 




