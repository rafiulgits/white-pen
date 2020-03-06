# Blogger Backend API

[TOC]

## Content Negotiation

Support `JSON` and `XML` serialization and deserialization.





## DTO Models

### Post DTO

This model is used to transfer a `Post` object from client to server and validate the object on required criteria.

* **Id** : UUID4/GUID
* **Title** : String
* **Body** : String
* **CreatedOn** : Date-Time



### Error DTO

This model is used to transfer an Error from server to client whenever client request invalid formation of data.

* **Field** : String
* **Message** : List of String



### User DTO

This model is used to transfer the information about  a new user on this system.

* **Email** : String
* **FirstName** : String
* **LastName** : String
* **Password** : String
* **BlogName** : String




### AUTH DTO

This model is used transfer user credentials to server

* **Email** : String | *format : example@mail.com
* **Password** : String 



### Token DTO

This model is used to transfer user access token from server to client side

* **Bearer** : String | *JWT Token*






## Response Status

| Status Code               | Reason                                                       |
| ------------------------- | ------------------------------------------------------------ |
| 200 OK                    | Successful `GET` ,`PUT`, `DELETE` request                    |
| 201 Created               | Success `POST` request                                       |
| 400 Bad Request           | Validation requirements or formation error                   |
| 401 Unauthorized          | When anonymous user want to access any authenticated endpoint |
| 403 Forbidden             | User doesn't have the permission to access that endpoint     |
| 404 Not Found             | If requested result not found by the system                  |
| 405 Method Not Allowed    | If requested method doesn't support by the endpoint          |
| 406 Not Acceptable        | If requested form (`Content-Type` and `Accept`) doesn't support by the system. See **Content Negotiation** |
| 500 Internal Server Error | Whenever server is failed to execute or finish a task.       |







## Endpoints

### Blog Posts



#### `POST` `/api/Post`

This endpoint is for create a new blog post.  Request body or data support `JSON` and `XML` formation defined in `Content-Type` header. For invalid body (syntax and format) server will return `400 Bad Request` with error message. Otherwise `201 Created` response will be served in required format.

**Authentication Required**

**Body**

* Title : String  | *max length=250, min length=1*
* Body : String |  *allowed empty string*
* CreatedOn : DateTime |  *format: 2020 02-27T07:15:27.395Z*



**Note**

*`PostDto` object has `Id` parameter which will be generated automatically by the system when requested to create new a post. If client side sent `Id` this will be ignored by the system*



**Request Format**

* JSON Request

```JSON
{
  "title": "string",
  "body": "string",
  "createdOn": "2020-02-26T11:27:37.982Z"
}
```

* XML Request

```xml
<?xml version="1.0" encoding="UTF-8"?>
<PostDto>
	<Title>string</Title>
	<Body>string</Body>
	<CreatedOn>2020-02-26T11:28:37.660Z</CreatedOn>
</PostDto>
```



**Response Format**

* JSON Response

```JSON
{
  "id": "609964d6-3ab3-446f-980e-08d7baaf36fe",
  "title": "string",
  "body": "string",
  "createdOn": "2020-02-26T11:29:54.129Z",
  "lastUpdateOn": "0001-01-01T00:00:00"
}
```

* XML Response

```XML
<Post xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
  <Id>609964d6-3ab3-446f-980e-08d7baaf36fe</Id>
  <Title>string</Title>
  <Body>string</Body>
  <CreatedOn>2020-02-26T11:29:54.129Z</CreatedOn>
  <LastUpdateOn>0001-01-01T00:00:00</LastUpdateOn>
</Post>
```





#### `GET` `/api/Post`

This endpoint is for get all available posts in database. If no post available in database then an empty array will return otherwise an array with available items.  By providing query parameter `filter` client can filter a substring contains by `Title`, `Body` and `Author.BlogName`.



**Query Parameter**

* Filter : string



**Response**

* XML Response

```xml
  <ArrayOfPost xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
  <Post>
      <Id>afb37453-ef7b-49a4-a770-08d7b9fcc1a2</Id>
    <Title>Hello World</Title>
      <Body>First Blog Post</Body>
    <CreatedOn>2020-02-25T14:12:11.506</CreatedOn>
      <LastUpdateOn>0001-01-01T00:00:00</LastUpdateOn>
    </Post>
  </ArrayOfPost>
```
* JSON Response

```json
[ 
    {
        "id": "609964d6-3ab3-446f-980e-08d7baaf36fe",
        "title": "string",
        "body": "string",
        "createdOn": "2020-02-26T11:29:54.129Z",
        "lastUpdateOn": "0001-01-01T00:00:00"
    }
]
```





#### `PUT` `/api/Post`

To update an existing post.

**Authentication Required**

**Required Body**

* Id : UUID/GUID | *format : "609964d6-3ab3-446f-980e-08d7baaf36fe"*
* Title : String  | *max length=250, min length=1*
* Body : String |  *allowed empty string*
* CreatedOn : DateTime |  *format: 2020 02-27T07:15:27.395Z*



**Request**

* JSON

```json
{
	"id": "851abf5a-0dfb-4879-afeb-08d7bb54d4da",
	"title": "updated Title",
	"body": "updated body",
	"createdOn": "2020-02-27T07:15:27.395Z"
}
```

* XML

```xml
<?xml version="1.0" encoding="UTF-8"?>
<PostDto>
   <Id>3fa85f64-5717-4562-b3fc-2c963f66afa6</Id>
   <Title>updated title</Title>
   <Body>updated body</Body>
   <CreatedOn>2020-02-27T08:23:55.004Z</CreatedOn>
</PostDto>
```



**Response**

* JSON

```json
{
    "id": "851abf5a-0dfb-4879-afeb-08d7bb54d4da",
    "title": "updated Title",
    "body": "updated body",
    "createdOn": "2020-02-27T07:15:27.395Z",
    "lastUpdateOn": "2020-02-27T07:15:28.395Z"
}
```

  

* XML

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Post>
   <Id>3fa85f64-5717-4562-b3fc-2c963f66afa6</Id>
   <Title>updated title</Title>
   <Body>updated body</Body>
   <CreatedOn>2020-02-27T08:23:55.004Z</CreatedOn>
   <LastUpdateOn>2020-02-27T08:23:56.004Z</LastUpdateOn>
</Post>
```






#### `GET` `/api/Post/{id}`

To fetch a particular post object. This endpoint take an `Id` as parameter and return the associated `Post` object.



**Required Parameter**

* Id : GUID/UUID4



**Response**

* JSON Response

```json
{
  "id": "609964d6-3ab3-446f-980e-08d7baaf36fe",
  "title": "string",
  "body": "string",
  "createdOn": "2020-02-26T11:29:54.129Z",
  "lastUpdateOn": "0001-01-01T00:00:00"
}
```

* XML Response

```xml
<Post xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
  <Id>609964d6-3ab3-446f-980e-08d7baaf36fe</Id>
  <Title>string</Title>
  <Body>string</Body>
  <CreatedOn>2020-02-26T11:29:54.129Z</CreatedOn>
  <LastUpdateOn>0001-01-01T00:00:00</LastUpdateOn>
</Post>
```





#### `DELETE` `/api/Post/{id}`

To delete a particular post object. This endpoint take an `Id` as parameter and return the associated `Post` object after deletion.

**Authentication Required**

**Required Parameter**

* Id : GUID/UUID4



**Response**

* JSON Response

```json
{
  "id": "609964d6-3ab3-446f-980e-08d7baaf36fe",
  "title": "string",
  "body": "string",
  "createdOn": "2020-02-26T11:29:54.129Z",
  "lastUpdateOn": "0001-01-01T00:00:00"
}
```

* XML Response

```xml
<Post xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
  <Id>609964d6-3ab3-446f-980e-08d7baaf36fe</Id>
  <Title>string</Title>
  <Body>string</Body>
  <CreatedOn>2020-02-26T11:29:54.129Z</CreatedOn>
  <LastUpdateOn>0001-01-01T00:00:00</LastUpdateOn>
</Post>
```





#### `GET` `/api/Post/page`

#### `GET` `/api/Post/page/{skip}`

#### `GET` `/api/Post/page/{skip}/{top}`

`/api/Post/page` endpoint has three options. By default `skip=0` and `top=20`. `skip` means how many post have to skip and `top` means how many post have to take. Client can hit any of this three option to paginate.



**Required Parameter**

* skip: Integer | *default : 0*
* top : integer | *default: 20*



**Response**

* XML Response

```xml
  <ArrayOfPost xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
  <Post>
      <Id>afb37453-ef7b-49a4-a770-08d7b9fcc1a2</Id>
    <Title>Hello World</Title>
      <Body>First Blog Post</Body>
    <CreatedOn>2020-02-25T14:12:11.506</CreatedOn>
      <LastUpdateOn>0001-01-01T00:00:00</LastUpdateOn>
    </Post>
  </ArrayOfPost>
```
* JSON Response

```json
  [ 
    {
      "id": "609964d6-3ab3-446f-980e-08d7baaf36fe",
      "title": "string",
      "body": "string",
      "createdOn": "2020-02-26T11:29:54.129Z",
      "lastUpdateOn": "0001-01-01T00:00:00"
    }
  ]
```





#### `GET /api/Post/blog/{name}`

To provide all post by an author blog name. If any post exists then return an array of posts with 200 status code otherwise 404.



**Required Parameter**

* Name : string


**Response**

* XML Response

```xml
  <ArrayOfPost xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
  <Post>
      <Id>afb37453-ef7b-49a4-a770-08d7b9fcc1a2</Id>
    <Title>Hello World</Title>
      <Body>First Blog Post</Body>
    <CreatedOn>2020-02-25T14:12:11.506</CreatedOn>
      <LastUpdateOn>0001-01-01T00:00:00</LastUpdateOn>
    </Post>
  </ArrayOfPost>
```
* JSON Response

```json
  [ 
    {
      "id": "609964d6-3ab3-446f-980e-08d7baaf36fe",
      "title": "string",
      "body": "string",
      "createdOn": "2020-02-26T11:29:54.129Z",
      "lastUpdateOn": "0001-01-01T00:00:00"
    }
  ]
```






### User Management



#### `POST /api/User`

To create or register a new user on that system



**Request**

* JSON

```json
{
  "firstName": "Mr",
  "lastName": "Wolf",
  "email": "wolf@mail.com",
  "password": "12345678",
  "blogName": "wolfBlog"
}
```



* XML

```xml
<?xml version="1.0" encoding="UTF-8"?>
<UserDto>
	<FirstName>string</FirstName>
	<LastName>string</LastName>
	<Email>string</Email>
	<Password>string</Password>
	<BlogName>string</BlogName>
</UserDto>
```





**Response**

* JSON

```json
{
  "bearer": "token"
}
```



* XML

```xml
<?xml version="1.0" encoding="UTF-8"?>
<TokenDto>
	<Bearer>token</Bearer>
</TokenDto>
```







#### `GET /api/User`

To fetch user profile

**Authentication Required**

**Response**

* JSON

```json
{
  "id": 13,
  "firstName": "Rafiul",
  "lastName": "Islam",
  "email": "rafi@mail.com",
  "blogName": "rafiulblog"
}
```



* XML

```xml
<?xml version="1.0" encoding="UTF-8"?>
<User xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
   <Id>13</Id>
   <FirstName>Rafiul</FirstName>
   <LastName>Islam</LastName>
   <Email>rafi@mail.com</Email>
   <BlogName>rafiulblog</BlogName>
</User>
```





### Authentication Management

#### `POST /api/Auth`

To generate a new access token with existing user credentials. User have to provide a valid `Email` and `Password` pair.



**Request**

* JSON

```json
{
  "email": "user@mail.com",
  "password": "pass1234"
}
```

* XML

```xml
<?xml version="1.0" encoding="UTF-8"?>
<AuthDto>
	<Email>string</Email>
	<Password>string</Password>
</AuthDto>
```



**Response**
* JSON

```json
{
  "bearer": "token"
}
```



* XML

```xml
<?xml version="1.0" encoding="UTF-8"?>
<TokenDto>
	<Bearer>token</Bearer>
</TokenDto>
```