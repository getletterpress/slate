---
title: API Reference

language_tabs:
  - shell

toc_footers:
  - <a href='https://app.postie.com'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

You've reached the Postie API documentation. The Postie API follows json:api recommendations. For more information, see <a href='http://jsonapi.org'>json:api</a>.

You must have a Postie account to make calls to the API. Your setup and authorization information is in the dashboard <a href='https://app.postie.com/setup'>https://app.postie.com/setup</a>.

Secondly you must upload PDF creatives to mail in the <a href='https://app.postie.com/creatives'>API creative dashboard</a>.

# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl -X GET -H "Accept: application/vnd.api+json" -H "X-User-Token: BokS593809494582wvpp" -H "X-User-Email: test@example.com" https://app.postie.com/api/v1/creatives
```

> You must replace BokS593809494582wvpp with your personal API token and test@example.com with your email.

To authorize, pass in your email and access token as headers:

`X-User-Token: BokS593809494582wvpp`
`X-User-Email: test@example.com`

<aside class="notice">
You must replace <code>BokS593809494582wvpp</code> with your personal API token and <code>test@example.com</code> with your email.
</aside>

# Creatives

## Get All Creatives

```shell
curl -X GET -H "Accept: application/vnd.api+json" -H "X-User-Token: BokS593809494582wvpp" -H "X-User-Email: test@example.com" https://app.postie.com/api/v1/creatives
```

> The above command returns JSON structured like this:

```json
{
  "data": [{
    "id": "KHYKUARR",
    "type": "creatives",
    "links": {
      "self": "https://app.postie.com/api/v1/creatives/KHYKUARR"
    },
    "attributes": {
      "name": "New User First Mailer",
      "creative-id": "KHYKUARR"
    }
  }]
}
```

This endpoint retrieves all creatives for the account that have been loaded into the dashboard at <a href="https://app.postie.com/creatives">https://app.postie.com/creatives</a>.

### HTTP Request

`GET https://app.postie.com/api/v1/creatives`

# Mailers

## Create Mailers

```shell
curl -X POST -H "Accept: application/vnd.api+json" -H "Content-Type: application/vnd.api+json" -H "X-User-Token: BokS593809494582wvpp" -H "X-User-Email: test@example.com" https://app.postie.com/api/v1/mailers -d '{"data": [{"type":"mailers", "attributes":{"name":"SeptemberMailer12016", "external-id": "your-id", "creative-id": "KHYKUARR", "firstname":"John", "lastname":"Doe", "email":"john.doe@test.com"}},{"type":"mailers", "attributes":{"name":"SeptemberMailer12016", "external-id": "your-second-id", "creative-id": "KHYKUARR", "firstname":"Jane", "lastname":"Doe", "address1": "1234 Main St", "address2": "Something secondary", "city": "San Diego","state": "CA","zip": "92102"}}]}'
```

> The above command returns JSON structured like this:

```json
{
  "data": [{
    "id": "21",
    "type": "mailers",
    "links": {
      "self": "https://app.postie.com/api/v1/mailers/21"
    },
    "attributes": {
      "name": "SeptemberMailer12016",
      "creative-id": "KHYKUARR",
      "email": "john.doe@test.com",
      "firstname": "John",
      "lastname": "Doe",
      "address1": null,
      "address2": null,
      "city": null,
      "state": null,
      "zip": null,
      "test-mode": false,
      "external-id": "your-id"
    }
  }, {
    "id": "22",
    "type": "mailers",
    "links": {
      "self": "https://app.postie.com/api/v1/mailers/22"
    },
    "attributes": {
      "name": "SeptemberMailer12016",
      "creative-id": "KHYKUARR",
      "email": null,
      "firstname": "Jane",
      "lastname": "Doe",
      "address1": "1234 Main St",
      "address2": "Something secondary",
      "city": "San Diego",
      "state": "CA",
      "zip": "92102",
      "test-mode": false,
      "external-id": "your-second-id"
    }
  }]
}
```

This endpoint creates a new mailer that will be sent to the addressee. Either an email address or an address is required. If only an email address is provided Postie will run a reverse email append to find a current postal address.

If a USPS certified address is not provided the piece will not be mailed - this will be reflected in the status of the mailer within 24 hours.

Multiple mailers can be created for each API call by nesting multiple requests in the data array.

A sample json request to create mailers looks like this:
{
  "data": [{
    "type": "mailers",
    "attributes": {
      "name": "SeptemberMailer12016",
      "external-id": "your-id",
      "creative-id": "KHYKUARR",
      "firstname": "John",
      "lastname": "Doe",
      "email": "john.doe@test.com"
    }
  }, {
    "type": "mailers",
    "attributes": {
      "name": "SeptemberMailer12016",
      "external-id": "your-second-id",
      "creative-id": "KHYKUARR",
      "firstname": "Jane",
      "lastname": "Doe",
      "address1": "1234 Main St",
      "address2": "Something secondary",
      "city": "San Diego",
      "state": "CA",
      "zip": "92102"
    }
  }]
}

### HTTP Request

`POST https://app.postie.com/api/v1/mailers`

### Post Parameters

Parameter | Required | Description
--------- | ------- | -----------
name | true | Name of the mailer. Same-named mailers will be grouped in the dashboard for performance analysis.
external-id | false | Your internal id that you want this mailer tagged with.
creative-id | true | The creative id that you got from the creative endpoint.
firstname | true | Recipient first name.
lastname | true | Recipient last name.
email | false | If no address is given, Postie will attempt a reverse email append on this email.
address1 | false | First line of address.
address2 | false | Second line of address.
city | false | City of address.
state | false | State of address.
zip | false | Zip of address.
test-mode | false | If set to true this mailer will not be sent. Defaults to false.


## Show Mailer

```shell
curl -X GET -H "Accept: application/vnd.api+json" -H "Content-Type: application/vnd.api+json" -H "X-User-Token: BokS593809494582wvpp" -H "X-User-Email: test@example.com" https://app.postie.com/api/v1/mailers/1234
```

> The above command returns JSON structured like this:

```json
{
  "data": {
    "id": "1234",
    "type": "mailers",
    "links": {
      "self": "https://app.postie.com/api/v1/mailers/1234"
    },
    "attributes": {
      "name": "SeptemberMailer12016",
      "creative-id": "KHYKUARR",
      "mailed": false,
      "status": "Processing",
      "email": "john.doe@test.test",
      "firstname": "John",
      "lastname": "Doe",
      "test-mode": false,
      "external-id": "some-customer-id"
    }
  }
}
```

This endpoint returns the status of a previously created mailer. Use the id or uri returned from the original create post API call to retrieve the status.

The status has three possible values:
"Processing" if the email append or address verification is still in progress.
"Address failed certification" if the address passed in failed CASS certification.
"No adddress found for email" if no address was passed in and there was no match on the email to postal lookup.
"Mailed" if the piece was successfully mailed.

### HTTP Request

`GET https://app.postie.com/api/v1/mailers/12345`

### Response Attributes

Parameter | Description
--------- | -----------
name | Name of the mailer.
external-id | Your internal id that was passed in during creation.
creative-id | The creative id that correlates to the creative endpoint.
mailed | True/False depending on mailed status.
status | Mail piece status.
firstname | Recipient first name.
lastname | Recipient last name.
email | If no address is given, Postie will attempt a reverse email append on this email.
test-mode | Denotes whether this was a test mailer.

# Transactions

## Get All Transactions

```shell
curl -X GET https://app.postie.com/api/v1/transactions \
-H "Accept: application/vnd.api+json" \
-H "Content-Type: application/vnd.api+json" \
-H "X-User-Token: BokS593809494582wvpp" \
-H "X-User-Email: test@example.com"
```

> The above command returns JSON structured like this:

```json
{
    "data": [
        {
            "attributes": {
                "campaign-id": 3,
                "campaign-name": "Your Campaign",
                "completed-at": null,
                "number": "123"
            },
            "id": "1",
            "links": {
                "self": "https://app.postie.com/api/v1/transactions/1"
            },
            "type": "transactions"
        }
    ]
}
```

This endpoint retrieves all transactions along with the related campaign id and name.

### HTTP Request

`GET https://app.postie.com/api/v1/transactions`

## Get a Specific Transaction

```shell
curl -X GET https://app.postie.com/api/v1/transactions/1 \
-H "Accept: application/vnd.api+json" \
-H "Content-Type: application/vnd.api+json" \
-H "X-User-Token: BokS593809494582wvpp" \
-H "X-User-Email: test@example.com"
```

> The above command returns JSON structured like this:

```json
{
    "data": {
        "attributes": {
            "campaign-id": 3,
            "campaign-name": "Your Campaign",
            "completed-at": null,
            "order-number": "123"
        },
        "id": "1",
        "links": {
            "self": "https://app.postie.com/api/v1/transactions/1"
        },
        "type": "transactions"
    }
}
```

This endpoint retrieves a specific transaction.

### HTTP Request

`GET https://app.postie.com/api/v1/transactions/1`

### Response Attributes

Parameter | Description
--------- | -----------
campaign-id | The id of the campaign
campaign-name | The name of the campaign
completed-at | The date the transaction completed
order-number | The order number associated with the transaction