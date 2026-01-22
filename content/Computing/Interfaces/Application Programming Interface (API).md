---
aliases:
  - API
  - api
tags:
  - computing
  - engineering
  - digital-twins
  - data
---
# What are Application Programming Interfaces?
Interacting with a computer application requires some form of interface. When two computer applications need to interact, they do so using an Application Programming Interface (API). 

# API Architectures

More on [API Architectures](https://blog.postman.com/different-types-of-apis/). 

## Representational State Transfer (REST)
A REST API call follows four steps:
1. The client sends a request.
2. The server processes the request.
3. The server sends the response.
4. Data is transferred.
![REST Architecture](https://www.gstatic.com/bricks/image/321862a0-3a14-4abb-a148-36bb8781c0f7.png)
The REST API architecture is by far the most commonly used and powers much of the modern internet. 

#### Request Methods
- `GET` - retrieves a resource
- `POST` - submits new data to the server
- `PUT` - updates existing data
- `DELETE` - removes data

These request methods map to [[Create, Read, Update, Delete (CRUD)|CRUD]] operations. 

### Key Points on REST
- Maps resources to URLs, and uses [[Protocols|HTTP]] to methods to manipulate those resources. 
- For example `/users/{userID}` to get a user or `POST /user/{userID}/booking` to create a booking. 
- If you're returning large results, you'll want to use pagination. 




[More](https://cloud.google.com/discover/what-is-rest-api) on REST APIs. 

## Server Sent Events (SSE)

## Websockets

## gRPC

# API Design
More on [API Design](https://www.hellointerview.com/learn/system-design/core-concepts/api-design). 
## Microservices

### API Gateways

## Pagination

## Rate Limiting

