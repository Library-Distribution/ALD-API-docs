---
title: Stdlib Releases API
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

## Filters
Parameter  | Description                            | Legal Values                           | Default
-----------|----------------------------------------|----------------------------------------|-----------
`published`| the publication status of the releases | see [switches](./../general.html#toc_6)| `true`


## Restrictions
Unpublished releases are only shown to authenticated users who are members of the stdlib team.

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

# Describe a release
## Status
***in development***

## Request
```
GET /stdlib/releases/describe/:version
```

Parameter | Description
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

# Create a new release
### Status
***developed, but not live***

## Request
```
POST /stdlib/releases/create/:type
```

Parameter | Description
----------|-----------------------
`type`    | the update type for the new release: `patch`, `minor` or `major`

## Optional GET parameters
Parameter | Description
----------|-----------------------
`base`    | the release to base the new release on (in terms of version number). Can either be `all` (default) or `published`.

## Optional POST parameters
Parameter     | Description
--------------|-----------------------
`version`     | By default, the version is updated accoring to the `type` parameter. This parameter can be used to increase the new version.
`date`        | The date the release will be automatically published
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

# Delete a release
## Status
***developed, but not live***

## Request
```
DELETE /stdlib/releases/delete/:release
```

Parameter | Description
----------|--------------------
`release` | The semver version number of the release to delete

## Restrictions
This API requires authentication. The user must be part of the stdlib team.

Only unpublished releases can be deleted.

## Response
[empty](../general.html#toc_3)

# Modify a release's metadata
## Status
***planned***

## Request
```
POST /stdlib/releases/modify/:release
```

Parameter | Description
----------|--------------------
`release` | The semver version number of the release to modify

## Optional POST parameters
Parameter     | Description
--------------|--------------------
`version`     | bump the version number
`description` | change the release's short description
`date`        | The date the release will be automatically published

## Restrictions
This API requires authentication. The user must be part of the stdlib team.

Only unpublished releases can be modified.

### Response
[empty](../general.html#toc_3)

# Publish a release
## Status
***planned***

## Request
```
POST /stdlib/releases/publish/:release
```

Parameter | Description
----------|------------------
`release` | The version of the release to publish now.

## Restrictions
This API requires authentication. The user must be the admin of the stdlib team.

## Response
[empty](../general.html#toc_3)
