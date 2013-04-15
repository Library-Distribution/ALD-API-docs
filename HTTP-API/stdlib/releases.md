---
title: Stdlib Releases
layout: docs
---
With the stdlib release API you can retrieve information about the current and older stdlib releases.

# List stdlib releases
## Status
***developed, but not live***

## Request

```
GET /stdlib/releases/list
```

## Optional GET parameters
parameter  | description
-----------|------------------------
`published`| `0`, `both`: if the requesting user has the required privileges, show unpublished releases as well; `-1`, `false`, `no`: show *only* unpublished releases; `1`, `+1`, `true`, `yes` (default) : show only published releases


## Response
### For JSON:
```json
["1.0.0", "1.0.1", "1.1.0", "1.2.0", "2.0.0"]
```

### For XML:
```xml
<ald:releases xmlns:ald="ald://api/stdlib/releases/list/schema/2012">
    <ald:release ald:version="1.0.0"/>
    <ald:release ald:version="1.0.1"/>
    <ald:release ald:version="1.1.0"/>
    <ald:release ald:version="1.2.0"/>
    <ald:release ald:version="2.0.0"/>
</ald:releases>
```

# Describing a release
## Status
***in development***

## Request
```
GET /stdlib/releases/describe/:version
```

## GET parameters
parameter | description
----------|---------------------------
`version` | The version to describe: either a semver version number or `latest`

## Response
### For JSON:
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
        ]
}
```

### For XML:
```xml
<!-- todo -->
```

# Creating a new release
### Status
***developed, but not live***

## Request
```
POST /stdlib/releases/create/:type
```

## GET parameters
parameter | description
----------|-----------------------
`type`    | the update type for the new release: `patch`, `minor` or `major`

## Optional GET parameters
parameter | description
----------|-----------------------
`base`    | the release to base the new release on (in terms of version number). Can either be `all` (default) or `published`.

## Optional POST parameters
parameter | description
----------|-----------------------
`version` | By default, the version is updated accoring to the `type` parameter. This parameter can be used to increase the new version.
`date`    | The date the release will be automatically published
`description` | A short description for the release

All these parameters can also later be set or changed through the `modify` API method.

## Restrictions
This API requires authentication. The user must be part of the stdlib team.

## Response
### For JSON:
```json
{ "release" : "1.0.0" }
```

### For XML:
```xml
<ald:version xmlns:ald="ald://api/stdlib/releases/create/schema/2012">1.0.0</ald:version>
```

# Delete an unpublished release
## Status
***developed, but not live***

## Request
```
DELETE /stdlib/releases/delete/:release
```

## GET parameters
parameter | description
----------|--------------------
`release` | The semver version number of the release to delete

## Restrictions
This API requires authentication. The user must be part of the stdlib team.

## Response
empty (`204 No content`)

# Modify an unpublished release's metadata
## Status
***planned***

## Request
```
POST /stdlib/releases/modify/:release
```

## GET parameters
parameter | description
----------|--------------------
`release` | The semver version number of the release to modify

## Optional POST parameters
parameter | description
----------|--------------------
`version` | bump the version number
`description` | change the release's short description
`date`    | The date the release will be automatically published

## Restrictions
This API requires authentication. The user must be part of the stdlib team.

### Response
empty (`204 No content`)

# Publish a release
## Status
***planned***

## Request
```
POST /stdlib/releases/publish/:release
```

## GET parameters
parameter | description
----------|------------------
`release` | The version of the release to publish now.

## Restrictions
This API requires authentication. The user must be the admin of the stdlib team.

## Response
empty (`204 No content`)
