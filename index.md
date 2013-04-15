---
title: Public HTTP API for ALD
layout: docs
---
This describes the publicly available HTTP API for ALD. It will provide details about items, users and more.
To get a better understanding of these docs and libba.net's implementation of them, read the [general notes](general.html) on the API.

# Overview
The following API documentation is available:

* [Item API](items.html)
* [User API](users)
    * [Suspension API](users/suspension.html)
    * [Registration](users/registration.html)
* [Stdlib API](stdlib)
    * [Stdlib Release API](stdlib/releases.html)
    * [Stdlib Candidate API](stdlib/candidates.html)
    * [Stdlib Pending Items API](stdlib/pending.html)
    * [Stdlib Items API](stdlib/items.html)

***

# Get the API version
## Status
***developed, but not live***

## Request
```
GET /version
```

## Response
### For XML:
```xml
<ald:version xmlns:ald='ald://api/version/schema/2012'>1.0.0</ald:version>
```
### For JSON:
```json
{ "version" : "1.0.0" }
```