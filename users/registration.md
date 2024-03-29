---
title: Registration API
layout: docs
---
# Notes
## Workflow
Registration is now part of the API. The workflow is as follows:

* initiate registration session via API
* receive chaptcha email for verification
* complete registration by sending decoded chaptcha to the API

If a registration session is not completed, it expires after a certain (configurable) amount of time.

## Permissions
Permissions can be configured so that registration is blocked to the public. Only registered members with a special privilege may add other users. In this case, step 1 is done by the registered member, step 2 and 3 are done by the new user.

Also, it is in this case required to access the API via HTTPS as the requests have to be authenticated.

# Initiate a registration session

## Status
***developed, but not live***

## Request
```
POST /users/register
```

## POST parameters
Parameter      | Description
---------------|-------------------
`name`         | the new user name
`mail`         | the mail address of the new user. A verification mail will be sent to this address.
`password`     | the users desired password
`password-alt` | the users desired password again. Both password parameters must be obtained independently, but match exactly. Otherwise, registration will fail.
`template`     | the template text for the verification mail. See below for details.

## Response
[empty](../general.html#toc_3)

## mail template
The text for the verification mail. This can contain several variables which are replaced by the respective values in the mail:

Variable                        | Description
--------------------------------|--------------------------
`{%raw%}{%NAME%}{%endraw%}`     | The user name
`{%raw%}{%MAIL%}{%endraw%}`     | The user's email address
`{%raw%}{%PASSWORD%}{%endraw%}` | The user's password in cleartext. Be careful and only include this if necessary.
`{%raw%}{%ID%}{%endraw%}`       | The registration session ID which can be used to retrieve the chaptcha.

# Obtain the chaptcha token
## Status
***developed, but not live***

## Request
```
GET /users/register/token/:id
```

Parameter | Description
----------|------------------
`id`      | The registration session ID. This must be obtained via the template for registration initiation.

## Reponse
`Content-type: image/png`

the token as PNG image

# Complete the registration
## Status
***developed, but not live***

## Request
```
POST /users/register/verify/:id
```

Parameter | Description
----------|------------------
`id`      | The registration session ID. This must be obtained via the template for registration initiation.

## POST parameters
Parameter | Description
----------|------------------
`token`   | The decoded chaptcha token as string

## Response
[empty](../general.html#toc_3)



