# REST Documentation

## Bookstore Example
Based on Keith Casey's LinkedIn Learning tutorial
### Important disclaimers to make in this kind of document:
 - For internal use only
 - Work in progress

### Technologies used:
 - Hapi.js (hapi) framework for building the API
 - HAL for JSON resources

<br/>

## Endpoints:

### **GET /**
API Directory

Responses:
 - 200 OK

Example:

``GET /``
```
{
    "item_search_url": "https://bookstore.com/Search?query={query}",
    "item_url": "https://bookstore.com/Item/{item_id}",
    "cart_url": "https://bookstore.com/Cart/{cart_id}",
}
```

<br/>

### **GET /Search?query={query}&page={page}**
Search for items

Parameters:
 - query: The expression used to search for items
 - page: A positive non-zero integer selecting the page of results to display. '1' is equivalent to omitting the page parameter.

Responses:
 - 200 OK: 'query' was valid and search was completed
 - 400 Bad Request: 'query' was invalid
 - 404 Not Found: 'page' out of range
 - 5xx: Something went wrong with the search

Example:

``GET https://bookstore.com/Search?query=galaxy&page=2``
```
{
    "_links": {
        "self": { "href": "/Search?query=galaxy&page=2" },
        "prev": { "href": "/Search?query=galaxy" },
        "next": { "href": "/Search?query=galaxy&page=3" }
    },
    "count": 2,
    "responseTimeMs": 124,
    "_embedded": {
        "item": [
            {
                "_links": { "self": { "href": "/ViewItem/1222" } },
                "item_id": 1222,
                "title", "Hitchhiker's Guide to the Galaxy",
                "author", "Adams, Douglas"
            },
            {
                "_links": { "self": {"href": "/ViewItem/1223" } },
                "item_id": 1223,
                "title": "Guardians of the Galaxy",
                "author": "Drake, Arnold"
            }
        ]
    }
}
```

<br/>

### **GET /Item/{item_id}**
Get the information on one item

Parameters:
 - item_id: The unique identifier for the item

Responses:
 - 200 OK: Found the item
 - 301 Moved Permanently: Item was given a new identifier
 - 404 Not Found: No item with that identifier

Example:

``GET https://bookstore.com/Item/1222``
```
{
    "_links": {
        "self": { "href": "/Item/1222" }
    },
    "item_id": 1222,
    "title": "Hitchhiker's Guide to the Galaxy",
    "author": "Adams, Douglas",
    "isbn": "9781529046137",
    "publishDate": "13/07/2021",
    "stock": 4
}
```

<br/>

### **POST /Cart**

Request Body:
```
{
    "first_item": {item_id}
}
```
- item_id: the identifier of the first item in the cart

Responses:
 - 201 Created
 - 400 Bad Request: Didn't provide valid 'first_item'

Example:

``POST https://bookstore.com/Cart``

Request Body:
```
{
    "first_item": 1222
}
```
Response Body:
```
{
    "_links": {
        "self": { "href": "/Cart/4432" }
    },
    "cart_id": 4332,
    "count": 1,
    "_embedded": {
        "item": [
            {
                "_links": { "self": { "href": "/ViewItem/1222" } },
                "item_id": 1222,
                "title", "Hitchhiker's Guide to the Galaxy",
                "author", "Adams, Douglas"
            }
        ]
    }
}
```

<br/>

### **GET /Cart/{cart_id}**
View what is in cart.

Parameters:
 - cart_id: the unique identifier for a cart

Responses:
 - 200 OK
 - 301 Moved Permanently
 - 404 Not Found


Example:

``GET https://bookstore.com/Cart/4432``

```
{
    "_links": {
        "self": { "href": "/Cart/4432" }
    },
    "cart_id": 4332,
    "count": 1,
    "_embedded": {
        "item": [
            {
                "_links": { "self": { "href": "/ViewItem/1222" } },
                "item_id": 1222,
                "title", "Hitchhiker's Guide to the Galaxy",
                "author", "Adams, Douglas"
            }
        ]
    }
}
```

### **PUT /Cart/{cart_id}**
Add and remove items from the cart.

Parameters:
 - cart_id: the unique identifier for a cart

Responses:
 - 200 OK
 - 301 Moved Permanently
 - 400 Bad Request
 - 404 Not Found

Example:

``PUT /Cart/4432``

Request Body:
```
{
    "add": [1223],
    "remove": [1222]
}
```

Response Body:
```
{
    "_links": {
        "self": { "href": "/Cart/4432" }
    },
    "cart_id": 4332,
    "count": 1,
    "_embedded": {
        "item": [
            {
                "_links": { "self": {"href": "/ViewItem/1223" } },
                "item_id": 1223,
                "title": "Guardians of the Galaxy",
                "author": "Drake, Arnold"
            }
        ]
    }
}
```

### **DELETE /Cart/{cart_id}**
Clear cart

Parameters:
 - cart_id: The unique identifier of a cart

Responses:
 - 204 OK
 - 301 Moved Permanently
 - 404 Not Found

Example:

``DELETE /Cart/4432``

Response Code: 204 No Content