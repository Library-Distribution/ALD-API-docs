# Public HTTP API for ALD
This describes the (planned) publicly available HTTP API for ALD. It will provide details about items, users and more.

## Root URI
The HTTP root URI for all API requests is `http://maulesel.ahk4.net/api/`. HTTPS can be used via `https://ahk4.net/user/maulesel/api/`.

## `Accept` header
The `Accept` header can be used to determine if the response should be JSON (`application/json`) or XML (`text/xml`).
It can also be used to download an item package (`application/x-ald-package`).

## Authentication
Some APIs require authentication. Currently, this is done via [[HTTP Basic Auth|http://en.wikipedia.org/wiki/Basic_access_authentication]]. All authenticated requests will only work if accessed via HTTPS.

## Allowed requests
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
`version`   | `latest`, `first`                 | restricts output to the latest / first versions     | (none)
`reviewed` | <ul><li>`-1` / `no` / `false`</li><li>`0` / `both`</li><li>`+1` / `1` / `yes` / `true`</li></ul> | defines whether to return unreviewed items - the first 3 values return only unreviewed ones, the 2nd group returns both and the last 4 values return only reviewed items. | `true`
`stdlib`  | <ul><li>`-1` / `no` / `false`</li><li>`0` / `both`</li><li>`+1` / `1` / `yes` / `true`</li></ul> | defines how to include libraries in the stdlib - the first 3 values return only items *not* in the stdlib, the 2nd group returns both and the last 4 values return only items in the stdlib | `both`

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
XML Schema available at http://maulesel.ahk4.net/schema/2012/api/items/list.xsd.

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
`version` | the exact version number of the item, `latest` or `first`

#### Response
##### For JSON:
```json
{
    "name" : "item name",
    "version" : "item version",
    "reviewed" : true,
    "default" : false,
    "id" : "item GUID",
    "description" : "item description text",
    "type" : "'app' or 'lib'",
    "user" : "user who uploaded this item",
    "userID" : "b5fa0b54de54496eac96-177bf245569c",
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

##### For `application/x-ald-package`:
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
XML Schema available at http://maulesel.ahk4.net/schema/2012/api/items/add.xsd.

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