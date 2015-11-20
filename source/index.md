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

# Oauth Overview

All developers need to register their application before getting started. A registered OAuth application is assigned a unique Client ID and Client Secret. The Client Secret should not be shared. You may create a personal access token for your own use or implement the web flow below to allow other users to authorize your application.

## Definitions


| Phrase | Definition | example  |
| --------- | -------- | :------- |
| **Resource Owner** | the user who wants to share a resource | john.smith@xyz.com |
| **Client** | the application that wants to leverage a resource hosted by scalus.com | your social network |
| **Authorization Server** | the entity that decides to grant access to the client | scalus.com's authorization server |
| **Resource Server** | the place where the third party resource is hosted | scalus.com's server where the task's & tasklists exist |


## Web Application Flow

### 1) Redirect users to request access to Scalus

     GET https://organization_slug.scalus.com/oauth/authorize

Parameters

| Name | Type | Description  |
| --------- | -------- | :------- |
| **client_id** | string | The client ID on http://organization_slug.scalus.com/settings |
| **redirect_uri** | string | The URL in your app where users will be sent after authorization.  |

### 2) Scalus redirects back to your site

If the user accepts your request, Scalus redirects back to your site with a temporary code in a `code` parameter.

Parameters

| Name | Type | Description  |
| --------- | -------- | :------- |
| **client_id** | string | The client ID on http://organization_slug.scalus.com/settings |
| **client_secret** | string | The client secret you received on http://organization_slug.scalus.com/settings |
| **code** | string | The code you received as a response to Step 1 |
| **redirect_uri** | string | The URL in your app where users will be sent after authorization.  |


Exchange this for an access token:

     POST https://organization_slug.scalus.com/oauth/token
     Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW
     Content-Type: application/x-www-form-urlencoded;charset=UTF-8
     grant_type=client_credentials

After that you'll have the access token in the response:

    token = JSON.parse(response)["access_token"]

### 3)  Use the access token to access the API

The access token allows you to make requests to the API on a behalf of a user. 

    GET https://organization_slug.scalus.com/api/tasks?access_token=...

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

<aside class="warning">If you request a task that is not in your organization or the logged in user does not have permissions to see the task, the response will return 422 Unprocessable Entity.</aside>

### HTTP Request

`GET http://organization_slug.scalus.com/api/tasks/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the task to retrieve


# Users

## Get a Specific Task

```shell
curl
-F access_token="YOUR_ACCESS_TOKEN"
-X GET http://localhost:3000/api/users/32
```

```ruby
require 'rest-client'
require 'json'

client_id     = '4ea1b...'
client_secret = 'a2982...'

response = RestClient.get 'http://organization_slug.scalus.com/api/users/32', {
  access_token: 'YOUR_ACCESS_TOKEN'
}

```

> The above command returns JSON structured like this:

```json
{
  "data": {
    "type": “users”,
    "id": “32”,
    "attributes": {
      “first_name”: “Johnny”,
      “last_name”: “Dee”,
      “email”: “johnny.dee@scalusapi.com”,
      "status": “active”,
      “organization_id": 19,
      “team_id": 34,
      "kind": “organization”,
      "send_task_notifications": false,
      "has_daily_digest": true,
      "created_at": "2015-09-19 20:59:55"
    },
    "links": {
      "self": "http://example.com/api/users/32”
    },
    "relationships": {
    }
  },
  "included": []
}
```

Parameter | Description
--------- | -----------
first_name | First Name of the user
last_name | Last Name of the user
email | email of the user also used for logging in 
status | status of the user, Can be active or inactive 
organization_id | ID of the user’s organization
team_id | ID of the user’s team in the organization
kind | kind of user, Can be “Team” or “Organization”
send_task_notifications | Set to true if the user receives emails when events occur around tasks
has_daily_digest | Set to true if the user receives a daily email digest
created_at | time user was created in their organization’s time zone

