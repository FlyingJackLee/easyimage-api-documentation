---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - typescript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the EasyImage API
---

# Introduction

Welcome to the EasyImage API! You can use the API to access user, image library and label images.

The API is completly written in RESTful format, so it is easy to request by any language.


# Authentication

> Authorization header

```json
{
  "Authorization":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VybmFtZSI6InRlc3QifQ.dZ9M-4m9F-IUykYYlU0pfMY1SE1dEoLQ5sysYia4Csw"
}
```


Easyimage realizes JSON Web Tokens
(JWT) to authenticate the validity of quests. To get token, you need to send a POST request with usename and password.Please check the user-login for Authentication for detail.


# Restful data format
> The above is restful data format:

```json
{
  "code": 200~999,
  "message": "message",
  "data":{
    "key1":"value",
    "key2":"value"
  }
}
```


Most APIs are requested/responsed with the following restful format.
Check in the right console.



# User
## Login

```typescript
this.http.post<RestBody>("http://127.0.0.1:8080/api/user/login",requestBody);
```

> Request:

```json
{
  "code":999,
  "message":"login",
  "data":{
    "username":username,
    "password":password
}
```

> Response:

```json
{
  "code":200,
  "message":"",
  "data":{
    "token":username,
}
```


Login in with username and password,the response contains a token for authentication.

### Method
POST

### HTTP Request

`POST http://127.0.0.1:8080/api/user/login`

### Data Parameters

Key | required | Default | Description
--------- | -------| ------- | -----------
username | true | false | unique username(principal).
password | true | false | hash password(credential).


## Sign up

```typescript
this.http.post("http://127.0.0.1:8080/api/user/signup",signUpRequestBody)
```

> Request:

```json
{
  "code":999,
  "message":"check username",
  "data":{
    "username":username,
    "password":password,
    "email":email
  }
}
```

> Response:

```json
{
  "code":200,
  "message":"Sign Up complete",
  "data":{
}
```


Sign up with username, password and email.

### Method
POST

### HTTP Request

`POST http://127.0.0.1:8080/api/user/signup`

### Data Parameters

Key | required | Default | Description
--------- | -------| ------- | -----------
username | true | false | unique username(principal).
password | true | false | hash password(credential).
email | true | false | email address.


## Logout

```typescript
this.http.post("http://127.0.0.1:8080/api/user/logout",getTokenHeader())
```

> Request:

```json
{
  "code":999,
  "message":"log out",
  "data":{
    "username":username,
    "token":token,
  }
}
```

> Response:

```json
{
  "code":200,
  "message":"you have logged out",
  "data":{
}
```


Exsit current user.

### Method
POST

### HTTP Request

`POST http://127.0.0.1:8080/api/user/logout`

### Data Parameters

Key | required | Default | Description
--------- | -------| ------- | -----------
username | true | false | unique username(principal).
token | true | false | authentication token.

<aside class="success">
  JWT Token header required.
</aside>


## Find by username

```typescript
this.http.post("http://127.0.0.1:8080/api/user/find_by_username",)
```

> Request:

```json
{
  "code":900,
  "message":"user has been exist",
  "data":{
    "username":username,
}
```

> Response:

```json
{
  "code":200,
  "message":"Username is available",
  "data":{
}
```


Find a username in database to check if it is available in signup.

### Method
POST

### HTTP Request

`POST http://127.0.0.1:8080/api/user/find_by_username`

### Data Parameters

Key | required | Default | Description
--------- | -------| ------- | -----------
username | true | false | unique username(principal).


## Check token

```typescript
this.http.post("http://127.0.0.1:8080/api/user/check",getTokenHeader())
```

> Request:

```json
{
  "code":999,
  "message":"",
  "data":{
  }
}
```

> Response:

```json
{
  "code":200,
  "message":"Right token",
  "data":{
}
```


Exsit current user.

### Method
GET

### HTTP Request

`GET http://127.0.0.1:8080/api/user/}`

### Data Parameters

Key | required | Default | Description
--------- | -------| ------- | -----------
token | true | false | authentication token.

<aside class="success">
  JWT Token header required.
</aside>


# Library
## Library list

```typescript
this.http.post("http://127.0.0.1:8080/api/library/all",getTokenHeader())
```

> Response:

```json
{
  "code":200,
  "message":"fetch success",
  "data":{
    "library_name":library_first_or_default_image_path
  }
}
```

Get all library of current user.

### Method
GET

### HTTP Request

`GET http://127.0.0.1:8080/api/library/all`

### Data Parameters

Key | required | Default | Description
--------- | -------| ------- | -----------
token | true | false | authentication token.

<aside class="success">
  JWT Token header required.
</aside>

## Library list

```typescript
this.http.post("http://127.0.0.1:8080/api/library/create",getTokenHeader())
```

> Request:

```json
{
      "code":999,
      "message":"create library",
      "data":{
        "library_name":libraryName,
    }
}
```

> Response:

```json
{
  "code":200,
  "message":"success",
  "data":{
    "library_id":library_name
  }
}
```

create a new image library for user.

### Method
GET

### HTTP Request

`GET http://127.0.0.1:8080/api/library/create`

### Data Parameters

Key | required | Default | Description
--------- | -------| ------- | -----------
token | true | false | authentication token.
library_name | true | false | library name.

<aside class="success">
  JWT Token header required.
</aside>


## Upload image
```typescript
const req = new HttpRequest(
  const formData:FormData = new FormData();
  'POST',"http://127.0.0.1:8080/api/library/upload",formData,{
    reportProgress:true,
    responseType:'json',
    headers:new HttpHeaders(
      {
        'Authorization': this.cookieService.get("token") as string,
        'Access-Control-Allow-Origin': '*',
        'Access-Control-Allow-Headers': 'Content-Type',
        'Access-Control-Allow-Methods': 'GET,POST,OPTIONS,DELETE,PUT',
      }),
  }
);
return this.http.request(req);
```


> Response:

```json
{
  "code":200,
  "message":"Save Success",
  "data":{
  }
}
```

Upload images to serve, please user formdata to transfer image.

### Method
POST

### HTTP Request

`POST http://127.0.0.1:8080/api/library/upload`

### Data Parameters

Key | required | Default | Description
--------- | -------| ------- | -----------
token | true | false | authentication token.
formdata | true | false |  images' formdata.

<aside class="success">
  JWT Token header required.
</aside>


## Fetch images
```typescript
this.http.get<RestBody>(LIBRARY_BASE_URL+"images",{
      headers:this.getTokenHeader().headers,
      params:{
        library_name:library_name
      }
    })
  };
```


> Response:

```json
{
  "code":200,
  "message":"Save Success",
  "data":{
    "image_id":image(Object)
  }
}
```

Fetch images of specific library with current user.

### Method
GET

### HTTP Request

`GET http://127.0.0.1:8080/api/library/images`

### Data Parameters

Key | required | Default | Description
--------- | -------| ------- | -----------
token | true | false | authentication token.
library_name | true | false |  library name.

<aside class="success">
  JWT Token header required.
</aside>

## Tag a image

> Request:

```json
{
  "code":999,
  "message":"",
  "data":{
    "image_id": tag
  }
}
```


> Response:

```json
{
  "code":200,
  "message":"add successfully",
  "data":{
  }
}
```

Add a tag to a image.

### Method
POST

### HTTP Request

`POST http://127.0.0.1:8080/api/library/tag`

### Data Parameters

Key | required | Default | Description
--------- | -------| ------- | -----------
token | true | false | authentication token.
image_id | true | false |  image id.
tag | true | false |  new tag.


<aside class="success">
  JWT Token header required.
</aside>