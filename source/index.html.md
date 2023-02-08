---
title: Innate SDK Reference

language_tabs: # must be one of https://git.io/vQNgJ

toc_footers:
  - <a href='mailto:support@innate.com'>Contact Innate</a>

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the Innate SDK
---

# Introduction

Welcome to the Innate SDK! Here you'll find information and documentation regarding integration with Innate Assessments and the Innate App.

# Terminology

To get started, lets define a few terms to make these docs a bit easier to understand.

**Partners** That is most likely _you_. If you are looking to host an Innate Assessment on your site and/or intergrate with the Innate Api to incorporate Innate functionality into another app, then you are a partner.

**Campaign** Each project a partner establishes with Innate is called a Campaign. The campaign will be based on a specific assessment and be configured to the a parnters specifications.

**User** The individuals taking the assessments and reviewing the results.

**User Campaign** The instance of a User taking part in a Campaign. The User Campaign begins the moment a User starts an assessment and produces an unique identifier that is key to tying the Innate experience together.

# Assessment Widget

> The snippet will look like:

```html
<innate-app project-id="XXXXXXX"></innate-app>

<!-- place at the end of the <body> -->
<script async src="https://innate.app/loaders/widget.js?id=XXXXXXX"></script>
```

Hosting the Innate Assesment Widget within your site is done by placing the code snippet we provide within an html page. The <code>&lt;innate-app&gt;</code> web component is responsive and can be treated as a <code>&lt;div&gt;</code>.

## CSS Styling

```html
<template id="overrides">
  <style>
    .someclass {
      background-color: yellow;
    }
  </style>
</template>
<innate-app project-id="XXXXXXX" style-template="overrides"></innate-app>
```

CSS Styling within the widget can be overriden by providing a style template to the <code>&lt;innate-app&gt;</code> web component.

Examine the elements of the widget within your browser developer tools to identify the CSS selectors.

## Override Registration

```html
<innate-app project-id="XXXXXXX" reg-url="https://yoursite.com/regpage"></innate-app>
```

By adding a <code>reg-url</code> attribute to the <code>&lt;innate-app&gt;</code> web component the Registration step will be redirected to a url of your choosing. The User Campaign ID will appended to the query string with the name <code>ucid</code>

# API Authentication

> To get an access token, use this code:

```shell
curl "https://auth.innate.app/oauth2/token" \
  --user <CLIENT_ID>:<CLIENT_SECRET>
  -d "grant_type=client_credentials"
```

> Make sure to replace `<CLIENT_ID>` AND `<CLIENT_SECRET>` with your id and secret.

> The above command returns JSON structured like this:

```json
{
  "access_token": "...",
  "expires_in": 3600,
  "token_type": "Bearer"
}
```


The Innate API uses OAuth2 Client Credentials to allow access to the API. Your OAuth2 Client Id and Client Secret will be provided by Innate.

The API endpoints expect a Bearer token to be included in all API requests. The *access_token* response MUST be included in the header as follows:

`Authorization: Bearer <ACCESS_TOKEN>`

<aside class="notice">
You must replace <code>&lt;ACCESS_TOKEN&gt;</code> with token provided by /oauth2/token endpoint.
</aside>

# Registrations

## Get a Registration

```shell
curl "https://innate.app/api/registration/<ID>" \
  -H "Authorization: Bearer <ACCESS_TOKEN>"
```

> The above command returns JSON structured like this:

```json
{
  "id": "....",
  "email": "registration@test.com",
  "first_name": "Test",
  "last_name": "User",
  "registered": 1652886639
}
```

This endpoint retrieves a registration.

<aside class="warning">A <code>404 - Not Found</code> is returned if no registration exists</aside>

### HTTP Request

`GET https://innate.app/api/registration/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The User Campaign ID for the registration

## Create a Registration

```shell
curl "https://innate.app/api/registration"
  -H "Authorization: Bearer <ACCESS_TOKEN>"
  -H 'Content-Type: application/json'
  --data $'{"id": "...."
            "email": "registration@test.com", 
            "first_name": "Test", 
            "last_name": "User", 
            "entry_type": "url"}'
```

> The above command returns JSON structured like this:

```json
{
  "id": "24Fl9U....kY",
  "email": "registration@test.com",
  "first_name": "Test",
  "last_name": "User",
  "registered": 1652886639,
  "entry_url": "https://innate.app/access/entry/eyJhb....t1DmAA"
}
```

Use this endpoint to create a registration associated with a user.

### HTTP Request

`POST https://innate.app/api/registration`

### Registration Request

Parameter | Type | Required | Description
--------- | ----------- | ----------- | -----------
id | String | Yes | The User Campaign Id
email | String | Yes | The email address of the user
first_name | String | Yes | The first name of the user
last_name | String | Yes | The last name of the user
entry_type | String | Yes | The type of entry to generate. Must be `url`



# Fields

## Get Fields

```shell
curl "https://innate.app/api/fields/<ID>" \
  -H "Authorization: Bearer <ACCESS_TOKEN>"
```

> The above command returns JSON structured like this:

```json
{
  "id": "....",
  "gender": "female",
  "year_of_birth": 1972,
  "current_career": "19-1022.00",
  "education_level": "masters_degree"
}
```

This endpoint retrieves the fields assocaited with a user campaign.

<aside class="warning">A <code>404 - Not Found</code> is returned if no fields have been set</aside>

### HTTP Request

`GET https://innate.app/api/fields/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The User Campaign ID 

## Put Fields

```shell
curl "https://innate.app/api/fields/<ID>"
  -X PUT
  -H "Authorization: Bearer <ACCESS_TOKEN>"
  -H 'Content-Type: application/json'
  --data $'{"gender": "other", 
            "year_of_birth": 1982, 
            "current_career": "11-9199.01", 
            "education_level": "high_school"}'
```

> The above command returns JSON structured like this:

```json
{
  "id": "24Fl9U....kY",
  "gender": "other",
  "year_of_birth": 1982,
  "current_career": "11-9199.01",
  "education_level": "high_school"
}
```

Use this endpoint to update the fields associated with a user campaign.

### HTTP Request

`PUT https://innate.app/api/fields/<ID>`

### Fields Request

<aside class="info">All fields are optional, but at least one must be provided</aside>

Parameter | Type | Required | Description
--------- | ----------- | ----------- | -----------
gender | String | No | Valid values are `male`, `female`, or `other`
year_of_birth | String | No | Valid values are between (Current Year - 13) and (Current Year - 110). For 2023, the range would be 2010 - 1913
current_career | String | No | SOC Code of the user's current career. Only values from this [list](./images/all_careers.csv) are supported.
education_level | String | No | Valid values are `no_formal_education`, `high_school`, `trade_school`, `associates_degree`, `bachelors_degree`, `masters_degree`, or `doctoral_degree`


# Entry

## Generate an Entry Url

```shell
curl "https://innate.app/api/entry/<ID>"
  -H "Authorization: Bearer <ACCESS_TOKEN>"
  -H 'Content-Type: application/json'
  --data $'{"type": "url"}'
```

> The above command returns JSON structured like this:

```json
{
  "entry_url": "https://innate.app/access/entry/eyJhb....t1DmAA"
}
```

Use this endpoint to generate entry urls with which a user can be redirected to the their Innate assessment results.

<aside class="warning">Only user campaigns that have been registered will be able to generate entry urls</aside>

### HTTP Request

`POST https://innate.app/api/entry/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The User Campaign Id

### Entry Request

Parameter | Type | Required | Description
--------- | ----------- | ----------- | -----------
type | String | Yes | The type of entry to generate. Must be `url`

### Entry Response

Parameter | Description
--------- | -----------
entry_url | A tokenized url capable of sending fit for redirecting a user
