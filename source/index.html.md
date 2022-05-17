---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

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

**Campaign / Project** 

**User** 

**User Campaign** 


# Widget

Information about the widget

# Authentication

> To get an access token, use this code:

```shell
curl "https://auth.innate.app/oauth2/token" \
  --user <CLIENT_ID>:<CLIENT_SECRET>
  -d "grant_type=client_credentials"
```

> Make sure to replace `<CLIENT_ID>` AND `<CLIENT_SECRET>` with your id and secret.

The Innate API uses OAuth2 Client Credentials to allow access to the API. Your OAuth2 Client Id and Client Secret will be provided by Innate.

Then enpoints expect and bearer token to be included in all API requests. The *access_token* should be included in the header as follows:

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
  "id": "",
  "email": "Max",
  "first_name": "unknown",
  "last_name": "unknown",
  "registered": 5
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
curl "https://innate.app/api/registration/<ID>"
  -H "Authorization: Bearer <ACCESS_TOKEN>"
  -H 'Content-Type: application/json'
  --data $'{"email": "url", "first_name": "", "last_name": "", "entry_type": "url"}'
```

> The above command returns JSON structured like this:

```json
{
  "id": "",
  "email": "Max",
  "first_name": "unknown",
  "last_name": "unknown",
  "registered": 5,
  "entry_url": ""
}
```

Use this endpoint to create a registratoin associated with a user.

### HTTP Request

`POST https://innate.app/api/registration/<ID>`

### Registration Request

Parameter | Type | Required | Description
--------- | ----------- | ----------- | -----------
email | String | Yes | The email address of the user
first_name | String | Yes | The first name of the user
last_name | String | Yes | The last name of the user
entry_type | String | Yes | The type of entry to generate. Must be `url`

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
  "entry_url": "https://innate.app/..."
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
