# Public HTTP API for ALD
This describes the (planned) publicly available HTTP API for ALD. It will provide details about items, users and more.

## `Accept` header
The `Accept` header can be used to determine if the response should be JSON (`application/json`) or XML (`text/xml`).
It can also be used to download an item package (`application/ald-package`).

## Allowed requests
### Retrieve a list of users
#### Request
`GET /users.php`

#### Optional GET parameters:

parameter   | allowed values              | meaning                       | default
------------|-----------------------------|-------------------------------|---------
`count`     | any integer >= 1 or `all`   | how many user names to return | `all`
`start`     | any integer >= 0            | at which user index to start  | `0`

#### Response
##### For JSON:
an array of user names

##### For XML:
```xml
<ald:user-list xmlns:ald="ald://package/schema/2012">
    <ald:user name="user1"/>
    <ald:user name="user2"/>
    <!-- ... -->
</ald:user-list>
```
***

### Retrieve a user description
#### Request
`GET /users.php?name=*`

#### GET parameters

parameter | meaning
----------|------------------------------------
`name`    | the nickname of the user to return

#### Optional POST parameters

parameter | meaning
----------|--------------------------------------------------------------
`name`    | a valid user name
`mail`    | a valid user mail address. Either this or `name` must be given, not both.
`password`| the password of the user specified in one of the above parameters

If these credentials are given and match  the requested user, more data is revealed.

#### Response
##### For JSON:
```json
{
    "name" : "the user's nickname",
    "mail" : "the user's mail address, but only if credentials match",
    "joined" : "joined date, such as 2012-05-04",
    "privileges" : "integer, by default 0"
}
```

##### For XML:
```xml
<!-- todo -->
```
***

### List uploaded items
#### Request:

`GET /items.php`

#### Optional GET parameters
parameter | allowed values           | meaning                                        | default
----------|--------------------------|------------------------------------------------|----------------
`type`    | `app`, `lib`             | restricts the type of returned items           | (none)
`items`   | any integer >= 1 or `all`| the number of items to return                  | `all`
`start`   | any integer >= 0         | the item to start at (returning)               | `0`
`user`    | a valid user name        | restricts items to those uploaded by this user | (none)

#### Response
##### For JSON:
```json
[
    {
      "name" : "the item name",
      "id" : "the id of the item",
      "version" : "the version of the item"
    }
]
```

##### For XML:
```xml
<ald:item-list xmlns:ald="ald://package/schema/2012">
    <ald:item name="anyApp" version="0.1" id="1"/>
    <ald:item name="aLib" version="2.3" id="2"/>
    <!-- ... -->
</ald:item-list>
```
***

### List versions for an item
#### Request
`GET /items.php?name=*`

#### GET parameters
parameter | meaning
----------|------------------------
`name`    | the name of the item

#### Response
##### For JSON:
an array of available version numbers

##### For XML:
```xml
<ald:version-list xmlns:ald="ald://package/schema/2012">
    <ald:version value="1.0 beta 2"/>
    <ald:version value="1.2 RC"/>
    <!-- ... -->
</ald:version-list>
```
***

### Retrieve an item description
#### Request
`GET /items.php?id=*`

`GET /items.php?name=*&version=*`

#### GET parameters
parameter | meaning
----------|-----------------------------------------
`id`      | the GUID of the item package to retrieve
`name`    | the name of the item
`version` | the exact version number of the item

#### Response
##### For JSON:
```json
{
    "name" : "item name",
    "version" : "item version",
    "id" : "item GUID",
    "description" : "item description text",
    "type" : "'app' or 'lib'",
    "user" : "user who uploaded this item",
    "uploaded" : "upload date",
    "authors" :
        [
            {
                "name" : "author name",
                "user-name" : "AHK forum user name",
                "homepage" : "http://author-site.com",
                "mail" : "author@example.com"
            }
        ],
    "tags" : ["tag1", "tag2"],
    "links" :
        [
            {
              "name" : "name of the link",
              "description" : "short description of link",
              "href" : "http://... - address of link"
            }
        ],
    "dependencies" : "... todo ...",
    "requirements" : "... todo ..."
}
```

##### For XML:
```xml
<!-- todo -->
```

##### For `application/ald-package`:
the package file (i.e. a ZIP-file).
***