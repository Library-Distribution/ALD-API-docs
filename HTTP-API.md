# Public HTTP API for ALD
This describes the publicly available HTTP API for ALD. It will provide details about items, users and more.

# Root URI
The HTTP root URI for all API requests is `http://api.libba.net/`. HTTPS can be used via `https://api.libba.net/`.

# `Accept` header
The `Accept` header can be used to determine if the response should be JSON (`application/json`) or XML (`text/xml`).
It can also be used to download an item package (`application/x-ald-package`).

# Authentication
Some APIs require authentication. Currently, this is done via [[HTTP Basic Auth|http://en.wikipedia.org/wiki/Basic_access_authentication]]. All authenticated requests will only work if accessed via HTTPS.

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