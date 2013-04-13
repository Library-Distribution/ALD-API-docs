---
title: HTTP API
permalink: index.html
---
# Public HTTP API for ALD
This describes the publicly available HTTP API for ALD. It will provide details about items, users and more.

# Root URI
The HTTP root URI for all API requests is `http://api.libba.net/`. HTTPS can be used via `https://api.libba.net/`.

# `Accept` header
The `Accept` header can be used to determine if the response should be JSON (`application/json`) or XML (`text/xml`).
It can also be used to download an item package (`application/x-ald-package`).

# Authentication
Some APIs require authentication. Currently, this is done via [[HTTP Basic Auth|http://en.wikipedia.org/wiki/Basic_access_authentication]]. All authenticated requests will only work if accessed via HTTPS.

# Filters
In the documentation, you will find mentions of so-called *"filters"* for most of the API methods.
They are used to filter the output you get from a request, mostly for listing methods but in some
cases also for others. To enable a filter, just include it as a `GET` parameter in the request and
set it to the desired value. The output should then only include elements that match the filter condition.

## switches
Some of the filter parameters are so called *"switches"* which can either be *on*, *off* or *undetermined*.
These can always take the following values:

* `yes`, `true`, `+1` or `1`: the filter is *on*
* `both` or `0`: the filter is *undetermined*
* `no`, `false` or `-1`: the filter is *off*

**Note** that the default value of the switch if you do not include the filter in your request does not have
to be *undetermined*, it can also be *off* or *on*. See the filter's documentation for the default value.

# Output sorting
For the listing APIs, you also have the ability to sort the output. All sorting directives are specified
using the `GET` parameter `sort`. It consists of a list of sorting criteria ordered by precedence and
separated with `+`. By default, all criteria are sorted ascending, to reverse this, prefix the criteria
with an exclamation mark. The sorting criteria you can use are documented with the method they apply to.

An example of a sorting parameter could be:

```
...?sort=name+!version
```

This sorts the output first by name (ascending), and if there are two output elements with the same name,
they are sorted by version (descending).

# API Requests
## Get the API version
### Status
***developed, but not live***

### Request
```
GET /version
```

### Response
#### For XML:
```xml
<ald:version xmlns:ald='ald://api/version/schema/2012'>1.0.0</ald:version>
```
#### For JSON:
```json
{ "version" : "1.0.0" }
```