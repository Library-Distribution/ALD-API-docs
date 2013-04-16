---
title: User API
layout: docs
---

# Notes
## Identifying a user
In general, there are 2 ways to identify a user: his user name and his user ID.

* The user name is defined by the user on registration, but can always be changed later. For display to end-users, the user name is of course the better option.

* The user ID is a GUID which should be unique across all ALD servers. It is assigned on registration and can never be changed. When it comes to saving a user identifier, the ID is the better option. On display, the ALD client can then check the server for the (current) user name.

# Retrieve the list of users
## Status
***live***

## Request
```
GET /users/list
```

## Output restrictions:

Parameter   | Legal Values                | Description                   | Default
------------|-----------------------------|-------------------------------|---------
`count`     | any integer >= 1 or `all`   | how many user names to return | `all`
`start`     | any integer >= 0            | at which user index to start  | `0`

## Response
### For JSON:
```json
[ { "name" : "user nickname", "id" : "user ID (GUID)" } ]
```

### For XML:
```xml
<ald:user-list xmlns:ald="ald://api/users/list/schema/2012">
    <ald:user ald:name="user1" ald:id="48e3a5c62f3f4055a31b34611e78d9f2"/>
    <ald:user ald:name="user2" ald:id="b5fa0b54de54496eac96177bf245569c"/>
    <!-- ... -->
</ald:user-list>
```
XML Schema available at http://api.libba.net/schema/users/list.xsd.

# Retrieve a user description
## Status
***live***

## Request
```
GET /users/describe/:id
GET /users/describe/:name
```

Parameter | Description
----------|------------------------------------
`name`    | the nickname of the user to return
`id`      | the id of the user to return

## Response

If authentication is fulfilled and matches the requested user or a user with higher privileges, the mail address is included in the response. Otherwise, it will be replaced by an md5-hash.

### For JSON:
```json
{
    "name" : "AnyNickName",
    "mail" : "example@example.com",
    "joined" : "2012-05-04",
    "privileges" : "0",
    "id" : "b5fa0b54de54496eac96177bf245569c"
}
```

### For XML:
```xml
<ald:user xmlns:ald="ald://api/users/describe/schema/2012"
	ald:name="AnyNickName"
	ald:mail="example@example.com"
	ald:joined="2012-05-04"
	ald:privileges="0"
        ald:id="b5fa0b54de54496eac96177bf245569c"/>
```
XML Schema available at http://api.libba.net/schema/users/describe.xsd.


# Modify a user
## Status
***live***

## Request
```
POST /users/modify/:id
POST /users/modify/:name
```

Parameter | Description
----------|-------------------------------
`id`      | the user ID of the user to modify
`name`    | the nickname of the user to modify

## Optional POST parameters
Parameter | Description
----------|----------------------
`name`    | a new nickname for the user
`mail`    | a new email address for the user (requires account reactivation)
`password`| a new password for the user

## Restrictions
This API requires authentication. For modification of the mail address, account reactivation via an email sent to the new address is required.

## Response
empty (`204 No content`)