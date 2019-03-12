---
title: TalkLift Developers API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='https://app.talklift.com/accounts/signup/' target="_blank">Need an account? Sign up here.</a>

includes:
  - errors

search: true
---

# Introduction

TalkLift uses **Webhooks and REST API** to communicate between systems. From your [TalkLift Account](https://app.talklift.com) you can configure your application backend to receive various **EVENTS** sent when users:

  - Completes a [MODULE](/#general-webhook)
  - Reaches a [SLOT](/#slot-filling-webhook)


# Authentication

```shell
curl X POST "https://app.talklift.com/api-token-auth/" 
  -H "Content-Type: application/json"
  -d '{"username":"name@example.com", "password":"xxx"}'

# Response
{ 'token' : '9944b09199c62bcf9418ad846dd0e4bbdfc6ee4b' }
```
> Note: **username** field is used in place of email field.

To access your API Token, send a **POST** request with your **email** and **password** as request payload.

<aside class="notice">
Token retrival endpoint: https://app.talklift.com/api-token-auth/
</aside>

TalkLift API uses token authentication. To authenticate your API, send your request with the Authorization header.

```shell
curl "https://app.talklift.com/api/v1/api-resource/" 
  -H 'Authorization: Token 9944b09199c62bcf9418ad846dd0e4bbdfc6ee4b'
```

<aside class="notice">
Make sure to replace `9944b09199c62bcf9418ad846dd0e4bbdfc6ee4b` with your API Token.
</aside>

# How to Setup Webhook

Login to your [account](https://app.talklift.com) and select the project you would like to work with.

Navigate to the **integrations** setting. Under the Webhook section, set your endpoint url and basic auth details if available.

![Webhook Setup Form](images/webhook-setting-form.png


## How it Works

![Webhooks Summary Diagram](images/talklift-webhook-flow.png)

## General Webhook
> Sample slot filling payload when a module conversation is completed

```shell
{  
   "event_name":"Policy renewal",
   "payload":[  
      {  
         "id":5111,
         "slot_states":[  
            {  
               "id":764210,
               "story_state":5111,
               "slot":57981,
               "order":0,
               "is_done":true,
               "waiting_response":false,
               "data":"ncsp/ik/2018/081463",
               "slot_label":"Please enter your policy number",
            }
         ],
         "created_at":"2019-02-26T19:05:45.224084+03:00",
         "updated_at":"2019-03-11T16:45:46.652175+03:00",
         "session":5849801,
         "story":14301,
         "project":1,
         "org":900
      }
   ]
}
```

This webhook request is fired when the user chats with your bot and completes the module. Remember, a module can contain general responses and/or slots (KYC request). An event is fired when the user provides a final response.

![Webhook when user completes module](images/webhook-request-module-end.png)


### Fields descriptions

Field|Description
-----|-----------
event_name|Event identifier. Note event name is derived from the **module name**.
payload|The actual payload data being sent.
payload.slot_states|A list of all slot states. What the users have actually filled.

### Slot States schema description
Field|Description
-----|-----------
id|Slot state id
slot|Link to payload.slot
order|Order of which the slot should be filled
is_done|Complete status
waiting_response|Flag to check if slot is waiting for user input
data|The actual data that the user provided. Example in this case is the email, membership number etc. Show the actual value that was added
slot_label|Slot field label for your reference


## Slot Filling Webhook

Slots are used to collect users information e.g KYC details. Think of slots just like forms that 
the users need to fill as part of an application to accomplish the set goals. During the slot filling 
process, you might want to validate users information through an external endpoint and revert to the users based on your findings.

### Slot filling use cases examples

  - Validating user membership number through a third party API
  - Check if user provided input is valid e.g supported delivery location for e-commerce delivery
  - Save provided information before the user completes the rest of the questions
  - Authenticate the user through a third party OTP

### How to set a Slot Filling Webhook

From your module, click ***Ask for customer information*** as shown in the figure below. The collected information are collected through what we call slots. Think of them as form fields with different types e.g email, number etc. Users will be asked to provide the information and in the order set. 

After setting your slot, click on the edit button to open the advance option where you can set the webhook request. Set 'Validate with Webhook' to trigger a webhook event when the user fills the slot.

![Slot Filling Part A](images/slot-filling-part-a.png "Select field input to send")


![Slot Filling Part B](images/slot-filling-part-b.png "Configure 'Validate with Webhook'")

<aside class="notice">
Here is a sample payload sent to your endpoint on a slot filling event
</aside>

> Sample slot filling payload

```shell
{  
   "event_name":"SLOT-MODULE:Make a claim",
   "payload":[  
      {  
         "slot":{  
            "id":3442,
            "label":"Membership Number",
            "field_type":"TEXT",
            "choice_options":"",
            "validation_regex":"",
            "validation_message":"Please enter a valid membership number",
            "validate_with_webhook":true,
            "show_condition":"",
            "show_condition_value":"",
            "date_gte_today":false,
            "order":3,
            "created_at":"2019-03-07T17:46:28.479306+03:00",
            "updated_at":"2019-03-07T17:46:28.479366+03:00"
         },
         "value":"209192092",
         "session":58521,
         "project":1,
         "org":900,
         "slot_state":{  
            "id":917,
            "slot_states":[  
               {  
                  "id":6976,
                  "story_state":917,
                  "slot":5444,
                  "order":1,
                  "is_done":true,
                  "waiting_response":false,
                  "data":"Mr Awesome Customer",
                  "slot_label":"Let's start with your Full Name (Please include your prefix  (e.g. Mr/Mrs/Ms/ Chief/ Dr etc."
               },
               {  
                  "id":6980,
                  "story_state":917,
                  "slot":3442,
                  "order":2,
                  "is_done":true,
                  "waiting_response":false,
                  "data":"username@gmail.com",
                  "slot_label":"Email Address"
               },
               {  
                  "id":6981,
                  "story_state":917,
                  "slot":3442,
                  "order":3,
                  "is_done":false,
                  "waiting_response":true,
                  "data":null,
                  "slot_label":"Membership Number"
               }
            ],
            "created_at":"2019-02-27T11:46:25.133162+03:00",
            "updated_at":"2019-03-07T17:52:38.144156+03:00",
            "session":58521,
            "story":1220,
            "project":1,
            "org":900
         }
      }
   ]
}
```

### Fields descriptions

Field|Description
-----|-----------
event_name|Event identifier. Note it is prefixed with **SLOT-MODULE**: before the module name.
payload|The actual payload data being sent.
payload.slot|The current slot/field under review.
payload.slot_state|List of all slots available in the module. Show how users have filled the slots.
payload.value|User input for the slot.Used to trigger the event. Use this at your endpoint to process.
payload.session|User session id.
payload.project|Your project id.
payload.org|Your organization/business id.

### Slot States schema description

This is basically a log of user input in filling the slots. It shows the progress of the user in 
the slot filling.

Field|Description
-----|-----------
id|Slot state id
slot|Link to payload.slot
order|Order of which the slot should be filled
is_done|Complete status
waiting_response|Flag to check if slot is waiting for user input
data|The actual data that the user provided. Example in this case is the email, membership number etc. Show the actual value that was added
slot_label|Slot field label for your reference

## Sending Slot Feedback

```shell
curl X POST 'https://app.talklift.com/api/v1/session-slot-states/update_from_webhook/'
  -H 'Content-Type: application/json' 
  -H 'Authorization: Token 9944b09199c62bcf9418ad846dd0e4bbdfc6ee4b'
  -d '{  
      "id":6981,
      "story_state":917,
      "slot":3442,
      "order":3,
      "is_done":false,
      "waiting_response":true,
      "data":"209192092",
      "slot_label":"Membership Number",
      "project":1,
      "org":900
    }'
```

Slot filling request help in marking the item as done and the system automatically moves the user to the next slot. On succesful validation, use this to update the user session state as done and move to the next session state.
 

# Sending Message

After processing your webhook request, you might want to send a message back to the user. The following **POST** requests examples demostrate how you can accomplish the above.

## Sending a general message

Use this to send a message back to your chatbot user.

> Sample message payload

```shell
curl X POST 'https://app.talklift.com/api/v1/sessions/send_message/'
  -H 'Content-Type: application/json' 
  -H 'Authorization: Token 9944b09199c62bcf9418ad846dd0e4bbdfc6ee4b'
  -d '{"session_id":29289,"channel":"MESSENGER","sender":"BOT", "text":"The message to send", "org_id":22121,"project_id":2022, "buttons":[{"type": "postback","title": "Back to menu","payload": "main-menu"}]}'
```

### Message request fields description

Field|Description
-----|-----------
session_id|Target user session id. To be retrieved from sent event data
channel|Available options: MESSENGER, WEB
sender|Default value: BOT
text|Text message you are sending
buttons|Buttons array. See buttons field below for possible fields
org_id|Your organization id
project_id|Your project id

### Buttons fields description
Field|Description
-----|-----------
type|Button field type. Options:postback,web_url
title|Button title
url|Optional valid web url value for type web_url
payload|Optional payload for type postback
