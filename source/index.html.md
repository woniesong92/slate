---
title: API Reference

language_tabs:
  - shell
  - ruby
  - python
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

WorkersAI 는 API를 통해 수작업으로 해야하는 Task를 만들고, 머신러닝을 위한 label 들을 수집할 수 있게 해줍니다.

# Authentication

> Authentication이 필요한 request에 api_key param을 넣어주세요:

```ruby
include HTTParty

response = HTTParty.get('http://api.workers.ai.com/v1/tasks?api_key=test_tV4NttbVsN3crLCFonEj')
```

```python
import requests

response = requests.get('http://api.workers.ai.com/v1/tasks?api_key=test_tV4NttbVsN3crLCFonEj')
```

```shell
curl "http://api.workers.ai.com/v1/tasks?api_key=test_tV4NttbVsN3crLCFonEj"
```

```javascript

```

<aside class="notice">
개발중엔 <code>test API key</code> 를 사용해서 mock requests를 만들 수 있습니다.
</aside>

# Tasks

## Get All Tasks

```ruby
response = HTTParty.get('http://api.workers.ai.com/v1/tasks?api_key=test_tV4NttbVsN3crLCFonEj')
```

```python
import requests

response = requests.get('http://api.workers.ai.com/v1/tasks?api_key=test_tV4NttbVsN3crLCFonEj')
```

```shell
curl "http://api.workers.ai/v1/tasks"
  -H "Authorization: meowmeowmeow"
```

```javascript
let workers = api.tasks.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "external_id": "623ed523-d7cf-4007-be3e-2732b3b9c526"
    "task_type": "categorization",
    "callback_url": "http://example.com/callback",
    "instruction": "Is this dog big or small?",
    "params": {
      "attachment_type": "image",
      "attachment": "http://www.dogsarebig.com/dog1.jpg",
      "categories": ["big", "small"]
    },
    "response": {}
  }
  {
    "external_id": "8f611c16-590f-4eb3-84d5-28a67417cad1"
    "task_type": "categorization",
    "callback_url": "http://example.com/callback",
    "instruction": "Is this hotdog or not hotdog?",
    "params": {
      "attachment_type": "image",
      "attachment": "http://www.silliconvalley.com/hotdog.jpg",
      "categories": ["hotdog", "not hotdog"]
    },
    "response": {}
  }
]
```

This endpoint retrieves all tasks.

### HTTP Request

`GET http://api.workers.ai/v1/tasks`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
task_type | false | If set to true, the result will also include cats.
instruction | true | If set to false, the result will include tasks that have already been adopted.
params    | {}      | this is a param
callback_url    | "http://example.com/callback"      | this is a param
metadata    | {}      | this is a param

<aside class="success">
Remember — You can use WorkersAI API to start collecting labels regardless of business hours.
</aside>

## Get a Specific Task

```ruby
require 'HTTParty'

response = HTTParty.get('http://api.workers.ai.com/v1/tasks/:id?api_key=test_tV4NttbVsN3crLCFonEj')

```

```python
import requests

response = requests.get('http://api.workers.ai.com/v1/tasks/:id?api_key=test_tV4NttbVsN3crLCFonEj')
```

```shell
curl "http://api.workers.ai/v1/tasks/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const task = require('task');

let api = task.authorize('meowmeowmeow');
let max = api.tasks.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "task_type": "categorization",
  "callback_url": "http://example.com/callback",
  "instruction": "Is this dog big or small?",
  "params": {
    "attachment_type": "image",
    "attachment": "http://www.dogsarebig.com/dog1.jpg",
    "categories": ["big", "small"]
  },
  "response": {}
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://api.workers.ai/v1/tasks/<id>`

### URL Parameters

Parameter | Description
--------- | -----------
id | The id of the kitten to retrieve

