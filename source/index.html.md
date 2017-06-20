---
title: WorkersAI API Documentation

language_tabs:
  - ruby

search: true
---

# Introduction

WorkersAI 는 머신러닝을 위한 트레이닝 데이터를 만들때 사람이 해야만 하는 labeling 을 대신 해주는 서비스 입니다.
유저가 API를 통해서 Labeling Task (e.g. categorization, audio transcription)를 만들기만 하면
WorkersAI 가 빠른 시간 안에 정확한 label 을 해드립니다. Requester 로 가입할때 받으셨던 test API key를 사용해서
다양한 Task 들을 지금 만들어보세요.

# Authentication

API 를 통해서 Task 를 만들기 위해서는 Authentication 이 필요합니다.
먼저 Account Dashboard 에 있는 live api_key 또는 test api_key 를 보신 후
`api_key` param 에 환경에 따라 알맞는 api_key 를 포함시켜주시면 됩니다.

```ruby
include HTTParty

HTTParty.get('http://api.workers.ai.com/v1/tasks?api_key=test_tV4NttbVsN3crLCFonEj')
```

<aside class="notice">
  개발중엔 <code>test API key</code> 를 사용해서 mock requests를 만들 수 있습니다.
</aside>

# Tasks

## List of tasks

```ruby
HTTParty.get('http://api.workers.ai.com/v1/tasks?api_key=test_tV4NttbVsN3crLCFonEj')
```

> 정상적인 response 는 다음과 같은 JSON 구조를 가지고 있습니다:

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

### HTTP Request

`GET http://api.workers.ai/v1/tasks`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
task_type | "categorization" | Task 의 종류 (e.g. "categorization, audio_transcription")
instruction | "" | Worker 에게 보여줄 Task 를 수행하는 방법
params    | {}      | Task type 마다 다른 필요한 params
callback_url    | "http://example.com/callback" | Task 가 완료되었을때 이 response를 보낼 endpoint
metadata    | {}      | Task 에 포함시키고 싶은 추가 정보 (e.g. internal_task_id)

<aside class="success">
  지금 바로 API를 이용해서 Task 를 만들어보세요. 처음 5개의 API requests는 공짜입니다.
</aside>


## Specific task

```ruby
require 'HTTParty'

HTTParty.get('http://api.workers.ai.com/v1/tasks/:id?api_key=test_tV4NttbVsN3crLCFonEj')

```

> 정상적인 response 는 다음과 같은 JSON 구조를 가지고 있습니다:

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

### HTTP Request

`GET http://api.workers.ai/v1/tasks/<id>`

### URL Parameters

Parameter | Description
--------- | -----------
id | 불러오고 싶은 task의 id


## Callback Response 

> Categorization Task 일 경우 Callback (i.e. webhook) response 는 다음과 같은 JSON 구조를 가지고 있습니다:

```json
{
  "task_type": "categorization",
  "external_id": "a6613e60-5bed-4181-9341-2a2f248fa7e8",
  "params": {
    "categories": ["dog", "cat"]
  },
  "response": {
    "category": "dog"
  }
}
```

Task 가 완료되면 Task가 생성될때 지정하신 `callback_url` 로 다음과 같은 `webhook` 을 보냅니다.
이 `webhook` 을 이용해서 polling 을 할 필요 없이 데이터베이스를 실시간으로 업데이트 하실 수 있습니다.
만약 `callback_url` 에서 `2XX response` 를 주지 않는다면 Workers AI 는 몇 번의 재도전 후 더 이상 `webhook` 을 보내지 않습니다.

### Response Parameters

Parameter | Description
--------- | -----------
category | 이미지의 label (e.g. `dog`, `cat`)

<aside class="notice">
  위의 예시는 `categorization` Task 의 <code>response</code> 입니다.
  `response` 는 Task 의 종류에 따라 다른 구조로 되어있을 수 있습니다.
</aside>
