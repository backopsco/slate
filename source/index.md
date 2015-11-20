---
title: API Reference

language_tabs:
  - shell
  - ruby

toc_footers:
  - <a href='http://go.scalus.com/register'>Sign Up for a new account</a>

includes:
  - errors

search: true
---

# Scalus's API

HOST: http://organization_slug.scalus.com/api/

Scalus's API allows developers to create tasks within the scalus ecosystem.

# Tasks

## Creating a Task 

```shell
curl
-F task[title]="Thank you for looking at our API" \
-F task[description]="The best way to get work done and keep people in sync." \
-F task[requester_id]=1 \
-F task[assignee_id]=1 \
-F task[due_date]="10-25-2021" \
-F access_token="YOUR_ACCESS_TOKEN"
-X POST http://localhost:3000/api/tasks
```

```ruby
require 'rest-client'
require 'json'

client_id     = '4ea1b...'
client_secret = 'a2982...'

response = RestClient.post 'http://organization_slug.scalus.com/api/tasks', {
  access_token: 'YOUR_ACCESS_TOKEN',
  "task": {
    "title": "Thank you for looking at our API",
    "description": "The best way to get work done and keep people in sync.",
    "requester_id": 1,
    "assignee_id": 1,
    "due_date": "10-25-2021"
  }
}

```
> The above command returns JSON structured like this:

```json
{
  "data": {
    "type": "tasks",
    "id": "19",
    "attributes": {
      "title": "Thank you for looking at our API",
      "description": "The best way to get work done and keep people in sync.",
      "requester_id": 1,
      "assignee_id": 1,
      "created_by": 2,
      "due_date": "10-25-2021",
      "status": "not_started",
      "active_messages_count": 0,
      "source": "api",
      "created_at": "2015-09-19 20:59:55",
      "closed_at": nil
    },
    "links": {
      "self": "http://example.com/api/tasks/19"
    },
    "relationships": {
      "messages": {
        "links": {
          "self": "http://organization_slug.scalus.com/api/tasks/19/messages"
        },
        "data": [
          { "type": "messages", "id": "5" },
          { "type": "messages", "id": "12" }
        ]
      },
      "labels": {
        "links": {
          "self": "http://organization_slug.scalus.com/api/tasks/19/labels"
        },
        "data": [
          { "type": "labels", "id": "5" }
        ]
      }
    }
  },
  "included": [{
    "type": "labels",
    "id": "5",
    "attributes": {
      "name": "Milestone 1"
    },
    "links": {
      "self": "http://example.com/api/labels/5"
    }
  }]
}
```

This endpoint creates a task.

### HTTP Request

`POST http://organization_slug.scalus.com/api/tasks`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
title | true | The title of the task
description | false | A short description of the task between 0 & 250 characters
requester_id | false | ID of the `User` that requested the task 
assignee_id | false | ID of the `User` that is assigned the task 
due_date | false | Date that the task is due

<aside class="notice">
You must replace <code>organization\_slug</code> with your personal slug. (your subdomain in scalus is your organization\_slug)
</aside>

## Get a Specific Task


```shell
curl
-F access_token="YOUR_ACCESS_TOKEN"
-X GET http://localhost:3000/api/tasks/19
```

```ruby
require 'rest-client'
require 'json'

client_id     = '4ea1b...'
client_secret = 'a2982...'

response = RestClient.post 'http://organization_slug.scalus.com/api/tasks/19', {
  access_token: 'YOUR_ACCESS_TOKEN'
}

```

> The above command returns JSON structured like this:

```json
{
  "data": {
    "type": "tasks",
    "id": "19",
    "attributes": {
      "title": "Thank you for looking at our API",
      "description": "The best way to get work done and keep people in sync.",
      "requester_id": 1,
      "assignee_id": 1,
      "created_by": 2,
      "due_date": "10-25-2021",
      "status": "not_started",
      "active_messages_count": 0,
      "source": "api",
      "created_at": "2015-09-19 20:59:55",
      "closed_at": nil
    },
    "links": {
      "self": "http://example.com/api/tasks/19"
    },
    "relationships": {
      "messages": {
        "links": {
          "self": "http://organization_slug.scalus.com/api/tasks/19/messages"
        },
        "data": [
          { "type": "messages", "id": "5" },
          { "type": "messages", "id": "12" }
        ]
      },
      "labels": {
        "links": {
          "self": "http://organization_slug.scalus.com/api/tasks/19/labels"
        },
        "data": [
          { "type": "labels", "id": "5" }
        ]
      }
    }
  },
  "included": [{
    "type": "labels",
    "id": "5",
    "attributes": {
      "name": "Milestone 1"
    },
    "links": {
      "self": "http://example.com/api/labels/5"
    }
  }]
}
```

This endpoint retrieves a specific task if the user is authorized.

<aside class="warning">If you request a task that is not in your firm or the logged in user does not have permissions to see the task, the response will return 422 Unprocessable Entity.</aside>

### HTTP Request

`GET http://organization_slug.scalus.com/api/tasks/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the task to retrieve


# Authentication

> To authorize, use this code:
