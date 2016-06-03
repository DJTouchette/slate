---
title: VoteWise API

language_tabs:
  - angular

toc_footers:
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>
  - <a href='https://github.com/spencersnygg/VoteWise2'>VoteWise Repo</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the VoteWise API! You can use our API to access VoteWise API endpoints, which can get information on various users, states, districts and more in our database.

Our docs all refrence Angular 1.x. You can view code examples in the dark area to the right.

# Authentication

This API uses JSON web tokens, at the moment the only protected routes are prefixed with /admin ( for development purposes ).

For the front end something like this could be use to simplify the process of injecting JWT's into headers <a href='https://github.com/auth0/angular-jwt'>auth0 Angular</a>

# Users

## Create a user

```angular

$http({
    method: 'POST',
    url: http://localhost:8080/user/signup,
    data: $.param({
      name: "john Doh",
      password: "password",
      city: "Philadelphia",
      street: "2604 E Somerset St",
      zip: "15004",
      username: "JohnyyDoh",
      email: "Johny@gmail.com"
      }),
    headers: {'Content-Type': 'application/x-www-form-urlencoded'}
})


```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "error": false
}

```
```json
On error will return something like this:

{
  "success": false,
  "error": {
    "message": "User validation failed",
    "name": "ValidationError",
    "errors": {
      "email": {
        "message": "This email address is already registered",
        "name": "ValidatorError",
        "properties": {
          "type": "user defined",
          "message": "This email address is already registered",
          "path": "email",
          "value": "example@gmail.com"
        },
        "kind": "user defined",
        "path": "email",
        "value": "example@gmail.com"
      },
      "username": {
        "message": "This useranme is already registered",
        "name": "ValidatorError",
        "properties": {
          "type": "user defined",
          "message": "This useranme is already registered",
          "path": "username",
          "value": "johndoh"
        },
        "kind": "user defined",
        "path": "username",
        "value": "johndoh"
      }
    }
  }
}
```

This endpoint creates a user ( voter ).

### HTTP Request

`POST http://localhost:8080/user/signup`

### Query Parameters

Fields    | Description
--------- | -----------
name | The users name.
email | The users email.
password | The users password.
city | The users city.
street | The users street.
zip | The users zip.
username | The users username

<aside class="success">
Remember — set your headers to 'Content-Type': 'application/x-www-form-urlencoded'
</aside>

## Get a token! ( sign a user in )

```angular
$http({
    method: 'POST',
    url: 'http://localhost:8080/authenticate',
    data: $.param({
      password: "password",
      email: "Johny@gmail.com"
      }),
    headers: {'Content-Type': 'application/x-www-form-urlencoded'}
})

```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "message": "Enjoy your token!",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyIkX18iOnsic3RyaWN0TW9kZSI6dHJ1ZSwiZ2V0dGVycyI6e30sIndhc1BvcHVsYXRlZCI6ZmFsc2UsImFjdGl2ZVBhdGhzIjp7InBhdGhzIjp7ImFkZHJlc3MiOiJpbml0IiwicHJlc3MiOiJpbml0IiwiYWR2b2NhdGUiOiJpbml0IiwicG9saXRpY2lhbiI6ImluaXQiLCJhZG1pbiI6ImluaXQiLCJnZW9EaXYiOiJpbml0IiwicGFzc3dvcmQiOiJpbml0IiwiZW1haWwiOiJpbml0IiwidXNlcm5hbWUiOiJpbml0IiwibmFtZSI6ImluaXQiLCJjcmVhdGVkIjoiaW5pdCIsIl9fdiI6ImluaXQiLCJfaWQiOiJpbml0In0sInN0YXRlcyI6eyJpZ25vcmUiOnt9LCJkZWZhdWx0Ijp7fSwiaW5pdCI6eyJfX3YiOnRydWUsImNyZWF0ZWQiOnRydWUsInVzZXJuYW1lIjp0cnVlLCJnZW9EaXYiOnRydWUsImVtYWlsIjp0cnVlLCJhZGRyZXNzIjp0cnVlLCJhZHZvY2F0ZSI6dHJ1ZSwicHJlc3MiOnRydWUsInBvbGl0aWNpYW4iOnRydWUsImFkbWluIjp0cnVlLCJwYXNzd29yZCI6dHJ1ZSwibmFtZSI6dHJ1ZSwiX2lkIjp0cnVlfSwibW9kaWZ5Ijp7fSwicmVxdWlyZSI6e319LCJzdGF0ZU5hbWVzIjpbInJlcXVpcmUiLCJtb2RpZnkiLCJpbml0IiwiZGVmYXVsdCIsImlnbm9yZSJdfSwiZW1pdHRlciI6eyJkb21haW4iOm51bGwsIl9ldmVudHMiOnt9LCJfZXZlbnRzQ291bnQiOjAsIl9tYXhMaXN0ZW5lcnMiOjB9fSwi"
}

On error will return something like this:

{
  "success": false,
  "error": "User not found! "
}
```

This endpoint verifies a user and returns a token.

<aside class="warning">These tokens expire in 24 hours and should be sent in headers for every authorized request </aside>
<aside class="success">
Since Advocate, Press and Politician all extend user, you can get a token for all of them at this endpoint.
</aside>

### HTTP Request

`POST http://localhost:8080/authenticate

### URL Parameters

Fields    | Description
--------- | -----------
email | The users email.
password | The users password.

## Create a new Advocate

```angular

$http({
    method: 'POST',
    url: http://localhost:8080/advocate/signup,
    data: $.param({
      name: "john Doh",
      password: "password",
      city: "Philadelphia",
      street: "2604 E Somerset St",
      zip: "15004",
      username: "JohnyyDoh",
      email: "Johny@gmail.com",
      jobs: true,
      taxes: true,
      url: 'www.johndoh.com'
      }),
    headers: {'Content-Type': 'application/x-www-form-urlencoded'}
})


```

> The above command returns JSON structured like this:

```json
{
  "success": true,
  "error": false
}

```
```json
On error will return something like this:

{
  "success": false,
  "error": {
    "message": "User validation failed",
    "name": "ValidationError",
    "errors": {
      "email": {
        "message": "This email address is already registered",
        "name": "ValidatorError",
        "properties": {
          "type": "user defined",
          "message": "This email address is already registered",
          "path": "email",
          "value": "example@gmail.com"
        },
        "kind": "user defined",
        "path": "email",
        "value": "example@gmail.com"
      },
      "username": {
        "message": "This useranme is already registered",
        "name": "ValidatorError",
        "properties": {
          "type": "user defined",
          "message": "This useranme is already registered",
          "path": "username",
          "value": "johndoh"
        },
        "kind": "user defined",
        "path": "username",
        "value": "johndoh"
      }
    }
  }
}
```

This endpoint creates a Advocate.

### HTTP Request

`POST http://localhost:8080/advocate/signup`

### Query Parameters

Fields    | Description
--------- | -----------
name | The users name.
email | The users email.
password | The users password.
city | The users city.
street | The users street.
zip | The users zip.
username | The users username.
Civil Liberties | ( interest ) Boolean.
Crime and Punishment | ( interest ) Boolean.
Education | ( interest ) Boolean.
Energy | ( interest ) Boolean.
Environment | ( interest ) Boolean.
Gun Control | ( interest ) Boolean.
Health and Safety | ( interest ) Boolean.
Immigration | ( interest ) Boolean.
Infrastructure | ( interest ) Boolean.
International Relations | ( interest ) Boolean.
Jobs and the Economy | ( interest ) Boolean.
Quality of Life | ( interest ) Boolean.
Reproduction | ( interest ) Boolean.
Taxes | ( interest ) Boolean.
Social Services | ( interest ) Boolean.

<aside class="success">
Remember — set your headers to 'Content-Type': 'application/x-www-form-urlencoded'
</aside>
<aside class="warning">
Need to send at least 2 interests, or will receive validation error.
</aside>
