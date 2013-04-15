---
title: Public HTTP API for ALD
layout: docs
---
This describes the publicly available HTTP API for ALD. It will provide details about items, users and more.
To get a better understanding of these docs and libba.net's implementation of them, read the [general notes](general.html) on the API.

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