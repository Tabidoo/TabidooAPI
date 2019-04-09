# TabidooAPI
REST API Interface for Tabidoo.


FORMAT: 1A
HOST: https://app.tabidoo.cloud/api/v2

# Tabidoo API v.2

The Tabidoo API is a simple way to integrate 
your app with [Tabidoo](https://www.tabidoo.cloud). 
If you're a developer or familiar with APIs, you should be able to build a complete integration in minutes.

[Full Tabidoo Help](https://help.tabidoo.cloud/)

## Using the API

[Tabidoo](https://www.tabidoo.cloud) API is served over HTTPS, on the following endpoint ``https://app.tabidoo.cloud/api/v2/``.

## Authentication

[Tabidoo](https://www.tabidoo.cloud) uses JSON Web Token (JWT) authentication. 

Everything what you need is your own JWT Token. 
You can get one by logging into [Tabidoo app](https://app.tabidoo.cloud/#/login) 
and visiting [User settings page](https://app.tabidoo.cloud/#/user) - section *API* - here you will generate your token.

### Using Token

All API request must contain header named *Authorization* with 
Token as value - ``Authorization: Bearer <jwt-token>`` and made over HTTPS.

**Your JWT Token carries the same privileges as your user account, so be sure to keep it secret!**

## Rate limits

To help prevent strain on Tabidoo’s servers, our API 
checks requests per minute. 
There is a request limit for 
each JWT token. If a request exceeds the limit, 
[Tabidoo](https://www.tabidoo.cloud) will return a *429 Too Many Requests HTTP* status error and you have to wait at least 60 seconds 
before you send another request. 

### Limits
[Tabidoo](https://www.tabidoo.cloud/) offers several pricing plans and each has its own rate limits - check out our [pricing plan](https://www.tabidoo.cloud/pricing/).

If you need to set higher limits, please feel free to contact us at [support@tabidoo.cloud](mailto:support@tabidoo.cloud).

## Request rules

- Character encoding **UTF-8** is used
- Data is in the format **application/json**

### Allowed HTTPs requests
- **GET**    :   Get a resource or list of resources
- **POST**   :   To create resource 
- **PUT**    :   To (full) update resource
- **PATH**   :   To (partial) update resource
- **DELETE** :   To delete resource

## Response rules

- Character encoding **UTF-8** is used
- Data is in the format **application/json**
- Response JSON object contains one of the following top-level members:
    - ``data`` : the document´s "primary data" (object or array of objects)
    - ``errors`` : array of error objects
- The members ``data`` and ``errors`` **do not coexist** in the same response.

## Handle Errors

Error item contains three members. 

- ```type``` - type of error (enum)
    - *info* - validation (info) error
    - *warning* - unexpected work with data
    - *exception* - other errors
- ```id``` - identifier of error
- ```message``` - more info about error

Error body example: 
```json
{
    "errors": [
        {
            "type": "info",
            "id": "specifiedCodeAlreadyExistsException",
            "message": "EndPoint, Name, 853181"
        }
    ]
}
```

### HTTP status codes

#### Success 2xx

- **200** - Everything is working OK
- **201** - New resource has been created
- **204** - The resource was successfully deleted

#### Client errors 4xx

- **400** - Bad Request - The request was invalid or cannot be served. The exact error is explained in the error detail.
- **401** - Unauthorized - The request requires an user authentication
- **403** - Forbidden - The server understood the request, but is refusing it or the access is not allowed.
- **404** - Not found - There is no resource behind the URI.
- **429** - [Too Many Requests](#introduction/rate-limits)

#### Server errors 5xx
There is an error on our side. Please feel free to contact us at [support@tabidoo.cloud](mailto:support@tabidoo.cloud).

## API resources

### Schemas
Each application can contain several schemas. You can imagine the schema as an Excel sheet or a database table. 

The schema consists of items of different data types (such as textbox, date, number, etc.). 

**JSON example - Schema response**
```json
    { 
        "data": [{
            "id": "2252d3ce-0bce-44fa-b8d6-1e0ab1f0c277",
            "shortid": "xbo9aya392",
            "header": "Customers",
            "items": [...]
        }]
    }
```

- ```id``` - schema id
- ```shortid``` - schema short id
- ```header``` - schema header
- ```items``` - array of schema items

#### Schema items ```items```
Each item object contains meta information about the item


**JSON example - Item response**
```json
    {
        "name": "firstname",
        "header": "Firstname",
        "type": "text",
        "metadata": {
            ...
        }
    }
```

- ```name``` - item name
- ```header``` - item header
- ```type``` - data type of item 
    - ```text, radio, chat, long_text, dropdown, multichoice, checklist, date, datetime, checkbox, money, decimal, number, picture, file, system_type```
- ```metadata``` - meta information - by (data) type


### User data - records

User data represents data stored by the user via **[Tabidoo](https://www.tabidoo.cloud/)**. Every time you create a new record, a new "row" of user data is created.
Each record contains system informations and data properties with user values.

**JSON example - Record**
```json
    { 
        "data": [{
            "id": "158cb6ef-02a6-47dc-8134-188c1b6426e6",
            "created": "2019-03-22T14:41:23.5783244+00:00",
            "modified": "2019-03-24T16:45:23.5783244+00:00",
            "ver": 0,
            "fields": {
                "firstname": "John",
                "surname": "Snow",
                "age": 35
            }
        }
    }
```

- ``id`` - ID of the record
- ``created`` - the date the record was created
- ``modified`` - the date of the last change to record
- ``ver`` - current version of the record
- ``fields`` - the array of data properties with values


## Ping test [/ping]
If you want to quickly test whether you have everything well set up, try that simple Ping request.

### Ping [GET]

+ Request 

    + Headers
            
            Authentication: Bearer <jwt-token>

+ Response 200 (application/json)

        {
            "data": {
                "message": "Tabidoo ping OK"
            }
        }
        
## Schemas [/apps/{app_id}/schemas]
Resources for getting schema definitions.

+ Parameters
    + app_id (string, required) - ID of the application

### Get List of schemas [GET]
Get all schemas of the application.

+ Response 200

    + Attributes
        + id (string, required) - schema id
        + shortid (string, required) - schema short id
        + header (string, required) - schema header
        + items (array, optional) - array of schema items
            + name (string, required) - item name
            + header (string, required) - item header
            + type (string, required) - data type of item
            
    + Body
    
            { 
                "data": [
                {
                    "id": "2252d3ce-0bce-44fa-b8d6-1e0ab1f0c277",
                    "shortid": "xbo9aya392",
                    "header": "Customers",
                    "items": [
                        {
                            "name": "firstname",
                            "header": "Firstname",
                            "type": "text",
                            "metadata": {
                                "description": "Firstname of customer",
                                "maxLength": 100,
                                "required": true
                            }
                        },
                        {
                            "name": "surname",
                            "header": "Surname",
                            "type": "text",
                            "metadata": {
                                "description": "Surname of customer",
                                "maxLength": 100,
                                "required": true
                            }
                        },
                        {
                            "name": "age",
                            "header": "Age",
                            "type": "number",
                            "metadata": {
                                "description": "Age of customer",
                                "min": 18,
                                "max": 99,
                                "required": false
                            }
                        }
                    ]
                },
                {
                    "id": "334ce49c-cfdb-4598-ae3f-99c50f1306a4",
                    "shortid": "x8ahlhs1ii",
                    "header": "Cars",
                    "items": [
                        {
                            "name": "identifier",
                            "header": "Identifier",
                            "type": "text",
                            "metadata": {
                                "description": "Identifier of car",
                                "maxLength": 100,
                                "required": true
                            }
                        },
                        {
                            "name": "brand",
                            "header": "Brand",
                            "type": "dropdown",
                            "metadata": {
                                "description": "Car brand",
                                "required": true,
                                "items": [ 
                                    {
                                        "value": "BMW",
                                        "order": 0
                                    },
                                    {
                                        "value": "Audi",
                                        "order": 1
                                    }
                                ]
                            }
                        }
                    ]
                }
                ]
            }

### Get schema [GET /apps/{app_id}/schemas/{schema_id}]

Get one schema of the application.

+ Parameters 
    + schema_id (string) - ID of the schema

+ Response 200

    + Attributes
        + id (string, required) - schema id
        + shortid (string, required) - schema short id
        + header (string, required) - schema header
        + items (array, optional) - array of schema items
            + name (string, required) - item name
            + header (string, required) - item header
            + type (string, required) - data type of item

    + Body

            { 
                "data": {
                    "id": "2252d3ce-0bce-44fa-b8d6-1e0ab1f0c277",
                    "shortid": "xbo9aya392",
                    "header": "Customers",
                    "items": [
                        {
                            "id": "firstname",
                            "header": "Firstname",
                            "type": "text",
                            "metadata": {
                                "description": "Firstname of customer",
                                "maxLength": 100,
                                "required": true
                            }
                        },
                        {
                            "id": "surname",
                            "header": "Surname",
                            "type": "text",
                            "metadata": {
                                "description": "Surname of customer",
                                "maxLength": 100,
                                "required": true
                            }
                        },
                        {
                            "id": "age",
                            "header": "Age",
                            "type": "number",
                            "metadata": {
                                "description": "Age of customer",
                                "min": 18,
                                "max": 99,
                                "required": false
                            }
                        }
                    ]
                }
            }

## User data [/apps/{app_id}/schemas/{schema_id}/data{?limit,skip,sort,filter}]
Resources for CRUD operations with data.

+ Parameters
    + app_id (string, required) - ID of the application
    + schema_id (string) - ID of the schema
    + limit (optional, number) - The number of records returned in each request. Accepts any value between 0 and 1000.
        + Default: `25`
    + skip (optional, number) - Which record in the list to start from. Value should be greater than or equal to 0.
        + Default: `0`
    + sort (optional, string) - A list of sort schema items which are used to sort the retrieved record. 
    Simple sorting - ``orderby=age``, Multi sorting - ``orderby=age,surname(desc)``
    + filter (optional, string) - Filter condition (where condition). 
    Simple filter - ``filter=age(gte)10``, 
    Multi filter - ``filter=age(gte)10,surname(eq)Fox``, 
    Filter with multivalues - ``filter=surname(in)(Fox,Dog)``
        + Members
            + `contains`
            + `notcontains`
            + `startswith`
            + `endswith`
            + `eq`
            + `neq`
            + `empty`
            + `notempty`
            + `in`
            + `notin`
            + `gt`
            + `lt`
            + `gte`
            + `lte`

### Get List of data [GET]
**[GET]** 

To receive the list of data from a schema.

+ Response 200 (application/json)

    + Attributes 
        + data (array) - array of data/records

    + Body
        
            { 
                "data": [
                    {
                        "id": "158cb6ef-02a6-47dc-8134-188c1b6426e6",
                        "created": "2019-03-22T14:41:23.5783244+00:00",
                        "modified": "2019-03-24T16:31:28.5783244+00:00",
                        "ver": 0,
                        "fields": {
                            "firstname": "John",
                            "surname": "Snow",
                            "age": 35
                        }
                    },
                    {
                        "id": "8095b72f-5642-4a75-b4f7-5ba11da65ea2",
                        "created": "2019-03-22T14:41:23.5783244+00:00",
                        "modified": "2019-03-24T16:31:28.5783244+00:00",
                        "ver": 0,
                        "fields": {
                            "firstname": "Peter",
                            "surname": "Parker",
                            "age": 28
                        }
                    },
                    {
                        "id": "8905f3aa-97df-4c0b-b10f-d5fe3bcd6979",
                        "created": "2019-03-22T14:41:23.5783244+00:00",
                        "modified": "2019-03-24T16:31:28.5783244+00:00",
                        "ver": 2,
                        "fields": {
                            "firstname": "Jack",
                            "surname": "Reacher",
                            "age": 42
                        }
                    }
                ]
            }

### Get List of data (filter) [POST /apps/{app_id}/schemas/{schema_id}/data/filter{?limit,skip,sort}]
**[POST]**

To receive the list of data from a schema with filter condition sending in body.

+ Request 

    + Body 
    
            {
                "filter": [
                    { 
                        "field": "age",
                        "operator": "gte",
                        "value": "30"
                    }   
                ]
            }

+ Response 200 (application/json)

    + Body
        
            { 
                "data": [
                    {
                        "id": "158cb6ef-02a6-47dc-8134-188c1b6426e6",
                        "created": "2019-03-22T14:41:23.5783244+00:00",
                        "modified": "2019-03-24T16:31:28.5783244+00:00",
                        "ver": 0,
                        "fields": {
                            "firstname": "John",
                            "surname": "Snow",
                            "age": 35
                        }
                    },
                    {
                        "id": "8905f3aa-97df-4c0b-b10f-d5fe3bcd6979",
                        "created": "2019-03-22T14:41:23.5783244+00:00",
                        "modified": "2019-03-24T16:31:28.5783244+00:00",
                        "ver": 2,
                        "fields": {
                            "firstname": "Jack",
                            "surname": "Reacher",
                            "age": 42
                        }
                    }
                ]
            }

### Get count of data [GET /apps/{app_id}/schemas/{schema_id}/data/count{?limit,skip,sort,filter}]
**[GET]**

Get the number of records in schema.

Example request: 

``https://app.tabidoo.cloud/v2/apps/158cb6ef/schemas/8095b72f/data/count``


+ Response 200

    + Body
        
            { 
                "data": {
                        "count": 3
                }
            }

### Get count of data (filter) [POST /apps/{app_id}/schemas/{schema_id}/data/count/filter{?limit,skip,sort}]
**[POST]**

Get the number of records in schema with filter condition sending in body.

+ Response 200

### Get data record [GET /apps/{app_id}/schemas/{schema_id}/{record_id}]

+ Parameters
    + record_id (string) - ID of data record

+ Response 200

    + Body
    
            { 
                "data": {
                        "id": "158cb6ef-02a6-47dc-8134-188c1b6426e6",
                        "created": "2019-03-22T14:41:23.5783244+00:00",
                        "modified": "2019-03-24T16:31:28.5783244+00:00",
                        "ver": 0,
                        "fields": {
                            "firstname": "John",
                            "surname": "Snow",
                            "age": 35
                        }
                }
            }

### Create data record [POST /apps/{app_id}/schemas/{schema_id}/data]

+ Request

    + Body
    
            { 
                "fields": {
                    "firstname": "John",
                    "surname": "Snow",
                    "age": 35
                }
            }

+ Response 201

    + Body
    
            {
                "data": {
                    "id": "158cb6ef-02a6-47dc-8134-188c1b6426e6",
                    "created": "2019-03-22T14:41:23.5783244+00:00",
                    "modified": "2019-03-22T14:41:23.5783244+00:00",
                    "ver": 0,
                    "fields": {
                        "firstname": "John",
                        "surname": "Snow",
                        "age": 35
                    }
                }
            }

### Update data record [PATCH /apps/{app_id}/schemas/{schema_id}/data/{record_id}]

+ Parameters
    + record_id (string) - ID of data record
    
+ Request

    + Body
    
            { 
                "fields": {
                    "age": 39
                }
            }

+ Response 200

    + Body
    
            {
                "data": {
                    "id": "158cb6ef-02a6-47dc-8134-188c1b6426e6",
                    "created": "2019-03-22T14:41:23.5783244+00:00",
                    "modified": "2019-03-25T17:22:26.5783244+00:00",
                    "ver": 1,
                    "fields": {
                        "firstname": "John",
                        "surname": "Snow",
                        "age": 39
                    }
                }
            }

### Update data record (full) [PUT /apps/{app_id}/schemas/{schema_id}/data/{record_id}]

+ Parameters
    + record_id (string) - ID of data record
    
+ Request

    + Body
    
            { 
                "fields": {
                    "firstname": "Jack",
                    "surname": "Snow",
                    "age": 41
                }
            }

+ Response 200

    + Body
    
            {
                "data": {
                    "id": "158cb6ef-02a6-47dc-8134-188c1b6426e6",
                    "created": "2019-03-22T14:41:23.5783244+00:00",
                    "modified": "2019-03-25T17:22:26.5783244+00:00",
                    "ver": 2,
                    "fields": {
                        "firstname": "Jack",
                        "surname": "Snow",
                        "age": 41
                    }
                }
            }

### Delete data record [DELETE /apps/{app_id}/schemas/{schema_id}/data/{record_id}]

+ Parameters
    + record_id (string) - ID of data record
    
+ Response 204

