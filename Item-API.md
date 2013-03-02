The item API is the central part of the API. It can be used to retrieve information about items and to download them.

# API requests
## List uploaded items
### Status
***live***

### Request
```
GET /items/list
```

### Optional GET parameters
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
***deprecated:*** `stdlib` | <ul><li>`-1` / `no` / `false`</li><li>`0` / `both`</li><li>`+1` / `1` / `yes` / `true`</li></ul> | defines how to include libraries in the stdlib - the first 3 values return only items *not* in the stdlib, the 2nd group returns both and the last 4 values return only items in the stdlib | `both`

### Response
#### For JSON:
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

#### For XML:
```xml
<ald:item-list xmlns:ald="ald://api/items/list/schema/2012">
    <ald:item ald:name="anyApp" ald:version="0.1" ald:id="e3891565e6de449b8f7058eb49344f3e" ald:user="maul.esel" ald:user-id="248b7165a5f44bbdad838388dba6106a"/>
    <ald:item ald:name="aLib" ald:version="2.3" ald:id="0bf1abf386b841ffa2f4227e4639f34e" ald:user="maul.esel" ald:user-id="248b7165a5f44bbdad8-8388dba6106a"/>
    <!-- ... -->
</ald:item-list>
```
XML Schema available at http://maulesel.ahk4.net/schema/2012/api/items/list.xsd.

## Retrieve an item description
### Status
***live***

### Request
```
GET /items/describe/:id
GET /items/describe/:name/:version
```

### GET parameters
parameter | meaning
----------|-----------------------------------------
`id`      | the GUID of the item package to retrieve
`name`    | the name of the item
`version` | the exact version number of the item, `latest` or `first`

### Response
#### For JSON:
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
***Note:*** the `"default"` field is deprecated and will be removed soon.

#### For XML:
```xml
<!-- todo, namespace: "ald://api/items/describe/schema/2012" -->
```

#### For `application/x-ald-package`:
the package file (i.e. a ZIP-file).

## Upload a new item
### Status
***live***

### Request
```
PUT /items/add
```

### POST data
The package file to upload. Note that the `Content-Length` header must be set appropriately.

### Restrictions
This API requires authentication.

### Response
empty (`204 No content`)

XML Schema available at http://maulesel.ahk4.net/schema/2012/api/items/add.xsd.