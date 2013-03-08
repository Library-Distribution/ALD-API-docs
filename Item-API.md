The item API is the central part of the API. It can be used to retrieve information about items and to download them.

# API requests
## List uploaded items
### Status
***live***

### Release
***[0.1.0](../tree/0.1.0)***

### Request
```
GET /items/list
```

### Filters
parameter | description | values    | default
----------|-------------|-----------|-----------
`type`    | the type the item should have | one of the item types supported by the server | all types
`user`    | the user who uploaded the item | a GUID identifying the user | all users
`name`    | the name of the item (list its versions) | any item name supported by the server | all names
`version` | a special version of the items to be listed | `latest`, `first` | unspecified
`tags`    | the tags one of which the item should have | a list of tags, separated by `|` | no tags
`stdlib`  | ***(deprecated)*** whether the item should be in the stdlib or not | one of the following 3 states: `no`/`false`/`-1`, `both`/`0`, `yes`/`true`/`1`/`+1` | `both` / `0`
`reviewed` | whether reviewed or unreviewed items should be shown | same as above | `true`
`downloads`, `downloads-min`, `downloads-max` | the exact, minimum or maximum number of downloads the item should have (min and max can be combined, exact overrides both) | any positive integer (or `0`) | unspecified
`rating`, `rating-min`, `rating-max` | the exact, minimum or maximum average rating the item should have (as above, min and max can be combined but are overriden by exact) | any positive integer (or `0`) | unspecified

### Output restrictions
parameter | description
----------|-----------------------
`count`   | the number of items to output, or `all`
`start`   | start output at the given (zero-based) item index (sorting is not guaranteed, in work for 0.2.0)

### Response
#### For JSON:
```json
[
    {
      "name" : "the item name",
      "type" : "the item type",
      "id" : "the id of the item",
      "version" : "the version of the item",
      "user" : {
          "name" : "nickname of the user who uploaded this",
          "id" : "GUID of the user who uploaded this"
      }
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
XML Schema available at http://api.libba.net/schema/items/list.xsd.

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