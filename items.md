---
title: Item API
layout: docs
---
The item API is the central part of the API. It can be used to retrieve information about items and to download them.

# List uploaded items
## Status
***live***

## Release
***[0.2.0](https://github.com/Library-Distribution/ALD-API/tree/0.2.0)***

## Request
```
GET /items/list
```

## Filters
Parameter       | Description                                 | Legal Values                                 | Default
----------------|---------------------------------------------|----------------------------------------------|----------
`type`          | the type the item should have               | one of the item types supported by the server| unspecified
`user`          | the user who uploaded the item              | a GUID identifying the user                  | unspecified
`name`          | the name of the item (list its versions)    | any item name supported by the server        | unspecified
`version`       | a special version of the items to be listed | `latest`, `first`                            | unspecified
`version-min`   | the minimum semver version                  | any valid semver version                     | unspecified
`version-max`   | the maximum semver version                  | any valid semver version                     | unspecified
`stable`        | list only stable or unstable semver versions| see [switches](./general.html#toc_6)         | `both`
`tags`          | the tags one of which the item should have  | a list of tags, separated by vertical bars   | unspecified
`reviewed`      | the review status of the items to be listed | see [switches](./general.html#toc_6)         | `true`
`downloads`     | the exact number of item downloads          | any positive integer or `0`                  | unspecified
`downloads-min` | the minimum number of item downloads        | any positive integer or `0`                  | unspecified
`downloads-max` | the maximum number of item downloads        | any positive integer or `0`                  | unspecified
`rating`        | the exact average item rating               | any positive integer or `0`                  | unspecified
`rating-min`    | the minimum average item rating             | any positive integer or `0`                  | unspecified
`rating-max`    | the maximum average item rating             | any positive integer or `0`                  | unspecified

## Sort criteria
You can sort by the following criteria:

Criteria   | Description                                  | Note
-----------|----------------------------------------------|------------------
`name`     | the item name                                |
`version`  | the item version, sorting obeys semver rules | only use if required as might have performance impact
`uploaded` | the date the item was uploaded to the server |
`downloads`| the number of downloads                      |
`rating`   | the average item rating                      |

## Output restrictions
Parameter | Description                                       | Note
----------|---------------------------------------------------|------------------------------------
`count`   | the number of items to output, or `all`           |
`start`   | start output at the given (zero-based) item index | consider specifying sort criteria

## Response
### For JSON:
```json
[
    {
      "name" : "ItemName",
      "id" : "e3891565e6de449b8f7058eb49344f3e",
      "version" : "0.1.0"
    }
]
```

### For XML:
```xml
<ald:item-list xmlns:ald="ald://api/items/list/schema/2012">
    <ald:item ald:name="anyApp" ald:version="0.1.0" ald:id="e3891565e6de449b8f7058eb49344f3e"/>
    <ald:item ald:name="aLib" ald:version="2.3.1" ald:id="0bf1abf386b841ffa2f4227e4639f34e"/>
    <!-- ... -->
</ald:item-list>
```
XML Schema available at http://api.libba.net/schema/items/list.xsd.

# Retrieve an item description
## Status
***live***

## Release
***[0.2.0](https://github.com/Library-Distribution/ALD-API/tree/0.2.0)***

## Request
```
GET /items/describe/:id
GET /items/describe/:name/:version
```

Parameter | Description             | Legal Values
----------|-------------------------|---------------------------------------------------------------
`id`      | the GUID of the item    | a GUID in form of a 128-bit HEX number (without leading `0x`)
`name`    | the name of the item    | the item's name (a simple string)
`version` | the version of the item | an exact semver version number, `latest` or `first`

## Response
### For JSON:
```json
{
    "name" : "ItemName",
    "version" : "2.0.0+build2",
    "reviewed" : true,
    "id" : "e3891565e6de449b8f7058eb49344f3f",
    "description" : "The item's description text",
    "type" : "app",
    "user" : {
        "name" : "UserA",
        "id" : "b5fa0b54de54496eac96-177bf245569c"
    },
    "uploaded" : "2012-07-06 19:13:15",
    "rating" : 4.5,
    "downloads" : 2439,
    "authors" :
        [
            {
                "name" : "SomeAuthor",
                "user-name" : "AHK forum user name",
                "homepage" : "http://author-site.com",
                "mail" : "author@example.com"
            },
            {
                "name" : "AnotherAuthor"
            }
        ],
    "tags" : ["tag1", "tag2"],
    "links" : ["... todo ..."],
    "dependencies" : ["... todo ..."],
    "requirements" : ["... todo ..."]
}
```

### For XML:
```xml
<!-- todo, namespace: "ald://api/items/describe/schema/2012" -->
```

### For `application/x-ald-package`:
the package file (i.e. a ZIP-file).

# Upload a new item
## Status
***live***

## Request
```
PUT /items/add
```

## POST data
The package file to upload. Note that the `Content-Length` header must be set appropriately.

## Restrictions
This API requires authentication.

## Response
[empty](./general.html#toc_3)