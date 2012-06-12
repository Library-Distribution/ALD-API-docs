# Public HTTP API for ALD
This describes the (planned) publicly available HTTP API for ALD. It will provide details about items, users and more.

## Root URI
The root URI for all API requests is `http://maulesel.ahk4.net/api/`.

## `Accept` header
The `Accept` header can be used to determine if the response should be JSON (`application/json`) or XML (`text/xml`).
It can also be used to download an item package (`application/ald-package`).

## Allowed requests
### Retrieve a list of users
#### Request
`GET /users/list.php`

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
<ald:user-list xmlns:ald="ald://api/users/list/schema/2012">
    <ald:user ald:name="user1"/>
    <ald:user ald:name="user2"/>
    <!-- ... -->
</ald:user-list>
```
***

### Retrieve a user description
#### Request
`GET /users/describe.php`

#### GET parameters

parameter | meaning
----------|------------------------------------
`user`    | the nickname of the user to return

#### Response

If [[authentication]] is fulfilled and matches the requested user or a user with higher privileges, the mail address is included in the response. Otherwise, it will be replaced by an md5-hash.

##### For JSON:
```json
{
    "user" : "AnyNickName",
    "mail" : "example@example.com",
    "joined" : "2012-05-04",
    "privileges" : "0"
}
```

##### For XML:
```xml
<ald:user xmlns:ald="ald://api/users/describe/schema/2012"
	ald:name="AnyNickName"
	ald:mail="example@example.com"
	ald:joined="2012-05-04"
	ald:privileges="0"/>
```
***

### List uploaded items
#### Request:

`GET /items/list.php`

#### Optional GET parameters
parameter | allowed values                      | meaning                                             | default
----------|-------------------------------------|-----------------------------------------------------|----------------
`type`    | `app`, `lib`                        | restricts the type of returned items                | (none)
`count`   | any integer >= 1 or `all`           | the number of items to return                       | `all`
`start`   | any integer >= 0                    | the item to start at (returning)                    | `0`
`user`    | a valid user name                   | restricts items to those uploaded by this user      | (none)
`name`    | a valid app or lib name             | restricts output to different versions of this item | (none)
`tags`    | a list of tags, separated by &#124; | restricts output to items who have this / at least one of these tags| (none)
`latest`  | `false` or `0` to disable, anything else to enable this filter | restricts output to the latest ersion of each item found | `0`

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
<ald:item-list xmlns:ald="ald://api/items/list/schema/2012">
    <ald:item ald:name="anyApp" ald:version="0.1" ald:id="e3891565e6de449b8f7058eb49344f3e"/>
    <ald:item ald:name="aLib" ald:version="2.3" ald:id="0bf1abf386b841ffa2f4227e4639f34e"/>
    <!-- ... -->
</ald:item-list>
```
***

### Retrieve an item description
#### Request
`GET /items/describe.php`

#### GET parameters
parameter | meaning
----------|-----------------------------------------
`id`      | the GUID of the item package to retrieve
`name`    | the name of the item
`version` | the exact version number of the item

Either `id` or `name` and `version` must be given.

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
<!-- todo, namespace: "ald://api/items/describe/schema/2012" -->
```

##### For `application/ald-package`:
the package file (i.e. a ZIP-file).
***

### Upload a new item
#### Request
`POST /items/add.php`

#### POST parameters
parameter | meaning
----------|-----------------------------------------------
`package` | the package file to upload *(not the path)*

#### Restrictions
This API requires [[authentication]].

#### Response
##### For JSON:
```json
{ "id" : "354FD39FF09341ABC45E10CCD47692FF" }
```

##### For XML:
```xml
<ald:item-id xmlns:ald='ald://api/items/add/schema/2012' ald:id='354FD39FF09341ABC45E10CCD47692FF'/>
```
***

### Delete an item from the server
#### Request
`DELETE /items/remove.php`

#### GET parameters
parameter | meaning
----------|-----------------------------
`id`      | the ID of the item to delete

#### Restrictions
This API requires [[authentication]]. Possibly email-confirmation will also be needed.

This API is not yet implemented.