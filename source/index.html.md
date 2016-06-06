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
      url: 'www.johndoh.com',
      type: 'chambers of commerce',
      governingBody: [ 'state', 'federal' ],
      registrationNum: [ 23423423423424, 213131313123321 ]
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

This endpoint creates a Advocate user.

### HTTP Request

`POST http://localhost:8080/advocate/signup`

### Query Parameters

Fields    | Description | Type
--------- | ----------- | --------
name | The users name. | String
email | The users email. | String
password | The users password. | String
city | The users city. | String
street | The users street. | String
zip | The users zip. | Number
username | The users username. | String
Civil Liberties | ( interest ) | Boolean
Crime and Punishment | ( interest ) | Boolean
Education | ( interest ) | Boolean
Energy | ( interest ) | Boolean
Environment | ( interest ) | Boolean
Gun Control | ( interest ) | Boolean
Health and Safety | ( interest ) | Boolean
Immigration | ( interest ) | Boolean
Infrastructure | ( interest ) | Boolean
International Relations | ( interest ) | Boolean
Jobs and the Economy | ( interest ) | Boolean
Quality of Life | ( interest ) | Boolean
Reproduction | ( interest ) | Boolean
Taxes | ( interest ) | Boolean
Social Services | ( interest ) | Boolean
type | Types of Advocate accepted are: "non-profit", "registered lobbyist","chambers of commerce". | String
governingBody | Governing body can be: "state", or "federal". Only required if type is "registered lobbyist" | String
registrationNum | Registration number from governingBodybody. Only required if type is "registered lobbyist" | Number
EIN | EIN is only required is if the Advocate type is "non-profit". ( Employer Identification Number, must be 9 digits ) | Number

<aside class="success">
Remember — set your headers to 'Content-Type': 'application/x-www-form-urlencoded'
</aside>
<aside class="warning">
Need to send at least 2 interests, or will receive validation error.
</aside>

## Create a new Press

```angular

$http({
    method: 'POST',
    url: http://localhost:8080/press/signup,
    data: $.param({
      name: "john Doh",
      password: "password",
      city: "Philadelphia",
      street: "2604 E Somerset St",
      zip: "15004",
      username: "JohnyyDoh",
      email: "Johny@gmail.com",
      mediaOutlet: "TMZ",
      ( area of interest see Advocate )
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

This endpoint creates a new Press user.

### HTTP Request

`POST http://localhost:8080/press/signup`

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
mediaOutlet | The users media outlet ( paper, magazine ect.. )
areaOfInterests | See Advocate

<aside class="success">
Remember — set your headers to 'Content-Type': 'application/x-www-form-urlencoded'
</aside>

## Create a new Politician

```angular

$http({
    method: 'POST',
    url: http://localhost:8080/politician/signup,
    data: $.param({
      name: "john Doh",
      password: "password",
      city: "Philadelphia",
      street: "2604 E Somerset St",
      zip: "15004",
      username: "JohnyyDoh",
      email: "Johny@gmail.com",
      positionWanted: "Senator",
      website: "www.google.com",
      homeZip: "15004",
      positionState: "PA",
      positionCity: "Philadelphia"
      positionCounty: "Beaver County",
      additionalInfo: "Passionate about criminal justice, loves food."


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

This endpoint creates a new Politician user.

### HTTP Request

`POST http://localhost:8080/politician/signup`

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
mediaOutlet | The users media outlet ( paper, magazine ect.. )
positionWanted |
website| Politicians website,
homeZip| Home town zip code,
positionState| State where position is held,
positionCity| City where position is held,
positionCounty | County where position is held,
additionalInfo | Any additional info ( limited to 60 characters )


<aside class="success">
Remember — set your headers to 'Content-Type': 'application/x-www-form-urlencoded'
</aside>

# Admin

## Set your headers before every request

## Get a list of users

```angular

$http({
    method: 'GET',
    url: http://localhost:8080/admin/user/all,
    headers: {'x-access-token': 'Token here'}
})


```

> The above command returns JSON structured like this:

```json
[
  {
    "_id": "574f90f13bda0817269284c4",
    "name": "john doh",
    "password": "$2a$10$mcmj1gklog6fqanrji0z0od/gpdpnoly3mr8bce5acsytsrxfpmq.",
    "admin": false,
    "politician": false,
    "press": false,
    "advocate": false,
    "address": "574f90f03bda0817269284c3",
    "email": "random@gmail.com",
    "geoDiv": "574a82ad74933e9d01423a3b",
    "username": "randoms",
    "__v": 0,
    "created": "2016-06-02T01:50:41.373Z"
  },
  {
    "_id": "574faf103c515dca29790de7",
    "name": "john doh",
    "password": "$2a$10$t3nqdefydsccb//kx6qfwox8hxbycus3o3rvqw7ow73mad5tk2nja",
    "advocate": true,
    "press": false,
    "politician": false,
    "admin": false,
    "address": "574faf103c515dca29790de5",
    "email": "RandomDude@gmail.com",
    "geoDiv": "574a82ad74933e9d01423a3b",
    "username": "djtouchet,jalapano",
    "__v": 0,
    "created": "2016-06-02T03:59:12.661Z"
  },
  {
    "_id": "574fb235ce7939412ae2b2c7",
    "name": "john doh",
    "password": "$2a$10$2jiku3fctiwvtcqtv94mlotecd8vrnd4kutu1egtgzl6rlvsdiono",
    "advocate": true,
    "press": false,
    "politician": false,
    "admin": false,
    "address": "574fb235ce7939412ae2b2c5",
    "email": "random@gmail.com",
    "geoDiv": "574a82ad74933e9d01423a3b",
    "username": "CreativeUsername",
    "__v": 0,
    "created": "2016-06-02T04:12:37.537Z"
  }
]
```

This endpoint gets all users ( Voter, Politician, Press and Advocate ).

### HTTP Request

`GET http://localhost:8080/admin/user/all`


## Get a user by ID

```angular

$http({
    method: 'GET',
    url: http://localhost:8080/admin/user/574f90f13bda0817269284c4,
    headers: {'x-access-token': 'Token here'}
})


```

> The above command returns JSON structured like this:

```json

  {
    "_id": "574f90f13bda0817269284c4",
    "name": "john doh",
    "password": "$2a$10$mcmj1gklog6fqanrji0z0od/gpdpnoly3mr8bce5acsytsrxfpmq.",
    "admin": false,
    "politician": false,
    "press": false,
    "advocate": false,
    "address": "574f90f03bda0817269284c3",
    "email": "random@gmail.com",
    "geoDiv": "574a82ad74933e9d01423a3b",
    "username": "randoms",
    "__v": 0,
    "created": "2016-06-02T01:50:41.373Z"
  }

```

This endpoint get a specific user by there id. The ":id" should be replaced with the users id.

### HTTP Request

`GET http://localhost:8080/admin/user/:id`

## Get a Advocate, Press, Politician by ID

```angular

$http({
    method: 'GET',
    url: http://localhost:8080/admin/advocate/574690d09a40f2aa5b987cc8,
    headers: {'x-access-token': 'Token here'}
})


```

> The above command returns JSON structured like this:

```json

{
  "_id": "574690d09a40f2aa5b987cc8",
  "userId": "574690d09a40f2aa5b987cc7",
  "url": "www.google.com",
  "areaOfInterest": "574690d09a40f2aa5b987cc6",
  "confirmed": false,
  "__v": 0
}

```

This endpoint get a specific advocate, press or politician by there id or all by using `/all`

You can use the `userId` to get more info about the user by doing a `GET http://localhost:8080/admin/user/:userId `

### HTTP Requests

`GET http://localhost:8080/admin/advocate/:id`

`GET http://localhost:8080/admin/advocate/all`

`GET http://localhost:8080/admin/press/:id`

`GET http://localhost:8080/admin/advocate/all`

`GET http://localhost:8080/admin/politician/:id`

`GET http://localhost:8080/admin/advocate/all`


## Random Password Generator

```angular

$http({
    method: 'GET',
    url: http://localhost:8080/admin/randompass/8,
    headers: {'x-access-token': 'Token here'}
})


```

> The above command returns JSON structured like this:

```json

{
"password": "RTx3SQsKP"
}

```

This endpoint generates a random password, with a length of what you pass in.

### HTTP Request

`GET http://localhost:8080/admin/user/:length`


# GeoDiv Lookup ( only PA )

## Get by ID, fedRep, stateSen, stateRep, zip or by fips

```angular

$http({
    method: 'GET',
    url: 'http://localhost:8080/geodivpa/574a82ad74933e9d01423a11'
})


```

> The above command returns JSON structured like this:

```json

{
   "_id": "574a82ad74933e9d01423a11",
   "ZIPCensusTabulationArea": 15001,
   "FIPSstate": 42,
   "vtd": 5,
   "fedRep": 12,
   "stateSen": 47,
   "stateRep": 16,
   "sduni": 2130,
   "sdelm": 99999,
   "sdsec": 99999,
   "statePostalCode": "PA",
   "votingDistrictName": "ALIQUIPPA VTD 01",
   "ElementarySchoolDistrict": 4299999,
   "SecondarySchoolDistrict": 4299999,
   "__v": 0
 }
```

This endpoint get a specific geoDiv by id.

### HTTP Requests

`GET http://localhost:8080/geodivpa/:id`

`GET http://localhost:8080/geodivpa/fedRep/:fedRep`

`GET http://localhost:8080/geodivpa/stateSen/:stateSen`

`GET http://localhost:8080/geodivpa/stateRep/:stateRep`

`GET http://localhost:8080/geodivpa/zip/:zip`

`GET http://localhost:8080/geodivpa/fips/:fips`

# FIPS lookup

## Get by state or fips code

```angular

$http({
    method: 'GET',
    url: 'http://localhost:8080/fips/PA'
})


```

> The above command returns JSON structured like this:

```json

{
  "fips": {
    "_id": "574a7c3acd6ea40c85e57a92",
    "code": 42,
    "stateAbr": "PA",
    "state": "PENNSYLVANIA",
    "__v": 0
  }
}
```

This endpoint get a specific geoDiv by id.

### HTTP Requests

`GET http://localhost:8080/fips/:state`

`GET http://localhost:8080/fips/:code`

# ZIP lookup

## Get by id, state or city

```angular

$http({
    method: 'GET',
    url: 'http://localhost:8080/ziplookup/state/PA'
})


```

> The above command returns JSON structured like this:

```json

{
  "zip": [
    {
      "_id": "574a9c32cd24d8720cfdc7be",
      "zip": 15003,
      "state": "PA",
      "county": "Beaver County",
      "__v": 0,
      "city": [
        "Beaver County"
      ]
    },
    {
      "_id": "574a9c32cd24d8720cfdc7bd",
      "zip": 15001,
      "state": "PA",
      "county": "Beaver County",
      "__v": 0,
      "city": [
        "Beaver County",
        "Macarthur",
        "W Aliquippa"
      ]
    } ]
  }
  ```

This endpoint allows you to loookup zip codes by state and city

### HTTP Requests

`GET http://localhost:8080/ziplookup/state/:state`  :state = ( PA, MI .... ) )

`GET http://localhost:8080/ziplookup/city/:city`

`GET http://localhost:8080/ziplookup/zip/:zip`
