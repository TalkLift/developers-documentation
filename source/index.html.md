---
title: TalkLift Developers API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - json

toc_footers:
  - <a href='https://app.talklift.com/accounts/signup/' target="_blank">Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate' target="_blank">Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

TalkLift uses Webhooks and REST API to communicate between systems. From your [TalkLift Account](https://app.talklift.com) you can configure your application backend to receive various EVENTS sent when users:

  - Reaches a [SLOT](/#webhook-on-slot-filling)
  - Completes a [MODULE](/#webhook-when-user-completes-module)


# How to Setup Webhook

Login to your [account](https://app.talklift.com) and select the project you would like to work with.

Navigate to the **integrations** setting. Under the Webhook section, set your endpoint url and basic auth details if available.

![Webhook Setup Form](images/webhook-setting-form.png)

# Authentication

> TalkLift API uses token authentication. To authenticate your API, send your request with the Authorization header.

```shell
curl "https://app.talklift.com/api/v1/api-resource/" 
  -H 'Authorization: Token 9944b09199c62bcf9418ad846dd0e4bbdfc6ee4b'
```

> Make sure to replace `9944b09199c62bcf9418ad846dd0e4bbdfc6ee4b` with your API Token.

You can access your API Token from your account under your [profile's](https://app.talklift.com/profile/#api-token]) API section.

`Authorization: Token 9944b09199c62bcf9418ad846dd0e4bbdfc6ee4b`

<aside class="notice">
You must replace <code>9944b09199c62bcf9418ad846dd0e4bbdfc6ee4b</code> with your personal API token.
</aside>

# Webhooks Workflow

## How it works

![Webhooks Summary Diagram](images/talklift-webhook-flow.png)

## Webhook On Slot Filling

Slots are used to collect users information e.g KYC details. Think of slots just like forms that 
the users need to fill as part of an application to accomplish the set goals. During the slot filling 
process, you might want to validate users information through an external endpoint and revert to the users based on your findings.

### Slot filling use cases examples

  - Validating user membership number through a third party API
  - Check if user provided input is valid e.g supported delivery location for e-commerce delivery
  - Save provided information before the user completes the rest of the questions
  - Authenticate the user through a third party OTP

### How to set a Slot Filling Webhook

From your module, click xxx

## Webhook When User Completes Module

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## Delete a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete

