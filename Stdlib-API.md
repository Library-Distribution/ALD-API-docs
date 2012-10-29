With the stdlib API you can retrieve information about the current and older stdlib releases.

# API requests
## List stdlib releases
### Status
***in development***

### Request

```
GET /stdlib/releases/list
```

### Optional GET parameters
parameter | description
----------|------------------------
`stable`  | whether only stable releases should be returned or prerelease as well
`released`| if the requesting user has the required privileges, show unreleased releases as well


### Response
#### For JSON:
```json
["1.0.0", "1.0.1", "1.1.0", "1.2.0", "2.0.0"]
```

#### For XML:
```xml
<ald:releases xmlns:ald="ald://api/stdlib/releases/list/schema/2012">
    <ald:release ald:version="1.0.0"/>
    <ald:release ald:version="1.0.1"/>
    <ald:release ald:version="1.1.0"/>
    <ald:release ald:version="1.2.0"/>
    <ald:release ald:version="2.0.0"/>
</ald:releases>
```

## Describing a release
### Status
***planned***

### Request
```
GET /stdlib/releases/describe/:version
```

### GET parameters
parameter | description
----------|---------------------------
`version` | The version to describe: either a semver version number or `latest`

### Response
#### For JSON:
```json
{
    "release" : "1.0.1",
    "date" : "2012/05/30",
    "description" : "The release description",
    "changelog" : "release changelog",
    "libraries" :
        [
            {
                "name" : "CGUI",
                "version" : "1.2.0",
                "id" : "00527D3DE2FF494CADD56BF754F3246D"
            }
        ],
    "frameworks" :
        [
            {
                "name" : "Forms",
                "version" : "1.0.0",
                "id" : "00528E3DE2FF494CADD56BF754F3246D"
            }
        ]
}
```

### For XML:
```xml
<!-- todo -->
```
