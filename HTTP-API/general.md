---
title: General notes
layout: docs
---

This document informs about specifics of the HTTP protocol and other general information useful when dealing with an ALD server.
It also includes notes specific to the libba.net implementation.

# Root URL
The root URL for all API requests must be the same. It must support HTTPS (see [below](#toc_3)).

<div class="libba-specific">
The root URL for the libba.net implementation is <code>http://api.libba.net/</code>. HTTPS can be used via <code>https://api.libba.net/</code>.
</div>

# Output content types
Unless otherwise noted, ALD servers must support the following output content types for all usual output methods:

* JSON (`application/json`)
* XML (`application/xml`)

The output schematics must match those described in these docs.

Furthermore, servers may support any other output content type or alias them to other mime-types.

## Content Negotiation
ALD servers must obey the `Accept` header sent by the client. If they do not support the specified types, they must return the respective HTTP error code 406
with a list of supported content types.

In case the requested type is supported, the server output must be explicitly labeled with the content type it contains via the `Content-Type` header.

<div class="libba-specific">
At the moment, api.libba.net only supports the required content types. However, <code>application/xml</code> is aliased to <code>text/xml</code>.
</div>

# Authentication
Several methods in this API require authentication. They must of course not be accessed without valid credentials.
The authentication method being used can either be [HTTP Basic Auth](http://en.wikipedia.org/wiki/Basic_access_authentication) or
[Digest Access Authentication](http://en.wikipedia.org/wiki/Digest_access_authentication). . If, however, HTTP Basic authentication is supported,
it must only be allowed via HTTPS.

<div class="libba-specific">
libba.net only supports HTTP Basic Auth to avoid the storing of user passwords in cleartext.
</div>

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
