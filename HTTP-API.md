# Public HTTP API for ALD
This describes the (planned) publicly available HTTP API for ALD. It will provide details about items, users and more.

## Root URI
The HTTP root URI for all API requests is `http://maulesel.ahk4.net/api/`. HTTPS can be used via `https://ahk4.net/user/maulesel/api/`.

## `Accept` header
The `Accept` header can be used to determine if the response should be JSON (`application/json`) or XML (`text/xml`).
It can also be used to download an item package (`application/ald-package`).

## Authentication
Some APIs require authentication. Currently, this is done via [[HTTP Basic Auth|http://en.wikipedia.org/wiki/Basic_access_authentication]]. All authenticated requests will only work if accessed via HTTPS.

## Allowed requests
### Retrieve a list of users
#### Request
```
GET /users/list
```

#### Optional GET parameters:

parameter   | allowed values              | meaning                       | default
------------|-----------------------------|-------------------------------|---------
`count`     | any integer >= 1 or `all`   | how many user names to return | `all`
`start`     | any integer >= 0            | at which user index to start  | `0`

#### Response
##### For JSON:
```json
[ { "name" : "user nickname", "id" : "user ID (GUID)" } ]
```

##### For XML:
```xml
<ald:user-list xmlns:ald="ald://api/users/list/schema/2012">
    <ald:user ald:name="user1" ald:id="48e3a5c62f3f4055a31b34611e78d9f2"/>
    <ald:user ald:name="user2" ald:id="b5fa0b54de54496eac96177bf245569c"/>
    <!-- ... -->
</ald:user-list>
```
***

### Retrieve a user description
#### Request
```
GET /users/describe/:id
GET /users/describe/:name
```

parameter | meaning
----------|------------------------------------
`name`    | the nickname of the user to return
`id`      |  the id of the user to return

#### Response

If authentication is fulfilled and matches the requested user or a user with higher privileges, the mail address is included in the response. Otherwise, it will be replaced by an md5-hash.

##### For JSON:
```json
{
    "name" : "AnyNickName",
    "mail" : "example@example.com",
    "joined" : "2012-05-04",
    "privileges" : "0",
    "id" : "b5fa0b54de54496eac96177bf245569c"
}
```

##### For XML:
```xml
<ald:user xmlns:ald="ald://api/users/describe/schema/2012"
	ald:name="AnyNickName"
	ald:mail="example@example.com"
	ald:joined="2012-05-04"
	ald:privileges="0"
        ald:id="b5fa0b54de54496eac96177bf245569c"/>
```
***

### List uploaded items
#### Request:
```
GET /items/list
```

#### Optional GET parameters
parameter | allowed values                      | meaning                                             | default
----------|-------------------------------------|-----------------------------------------------------|----------------
`type`    | `app`, `lib`                        | restricts the type of returned items                | (none)
`count`   | any integer >= 1 or `all`           | the number of items to return                       | `all`
`start`   | any integer >= 0                    | the item to start at (returning)                    | `0`
`user`    | a valid user name                   | restricts items to those uploaded by this user      | (none)
`name`    | a valid app or lib name             | restricts output to different versions of this item | (none)
`tags`    | a list of tags, separated by &#124; | restricts output to items who have this / at least one of these tags| (none)
`latest`  | `false` or `0` to disable, anything else to enable this filter | restricts output to the latest version of each item found **(not fully implemented)**| `0`

#### Response
##### For JSON:
```json
[
    {
      "name" : "the item name",
      "id" : "the id of the item",
      "version" : "the version of the item",
      "user" : "nickname of the user who uploaded this",
      "userID" : "GUID of the user who uploaded this"
    }
]
```

##### For XML:
```xml
<ald:item-list xmlns:ald="ald://api/items/list/schema/2012">
    <ald:item ald:name="anyApp" ald:version="0.1" ald:id="e3891565e6de449b8f7058eb49344f3e" ald:user="maul.esel" ald:user-id="248b7165a5f44bbdad838388dba6106a"/>
    <ald:item ald:name="aLib" ald:version="2.3" ald:id="0bf1abf386b841ffa2f4227e4639f34e" ald:user="maul.esel" ald:user-id="248b7165a5f44bbdad8-8388dba6106a"/>
    <!-- ... -->
</ald:item-list>
```
***

### Retrieve an item description
#### Request
```
GET /items/describe/:id
GET /items/describe/:name/:version
```

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
    "userID" : "b5fa0b54de54496eac96-177bf245569c"
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
```
POST /items/add
```

#### POST parameters
parameter | meaning
----------|-----------------------------------------------
`package` | the package file to upload *(not the path)*

#### Restrictions
This API requires authentication.

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
```
DELETE /items/remove/:id
DELETE /items/remove/:name/:version
```

parameter | meaning
----------|-----------------------------
`id`      | the ID of the item to delete
`name`    | the name of the item
`version` | the exact version number of the item

#### Restrictions
This API requires authentication. Possibly email-confirmation will also be needed.

This API is not yet implemented.