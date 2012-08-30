# Workflow
Registration is now part of the API. The workflow is as follows:

* initiate registration session via API
* receive chaptcha email for verification
* complete registration by sending decoded chaptcha

If a registration session is not completed, it expires after a certain (configurable) amount of time.

# Permissions
Permissions can be configured so that registration is blocked to the public. Only registered members with a special privilege may add other users. In this case, step 1 is done by the registered member, step 2 and 3 are done by the new user.

# API requests
## initiate a registration session

### Status
***developed, but not live***

### Request

```
POST /users/register
```

### POST parameters
name           | description
---------------|-------------------
`name`         | the new user name
`mail`         | the mail address of the new user. A verification mail will be sent to this address.
`password`     | the users desired password
`password-alt` | the users desired password again. Both password parameters must be obtained independently, but match exactly. Otherwise, registration will fail.
`template`     | the template text for the verification mail. See [[mail template]] for details.

### Response
empty (`204 No content`)

### mail template
The text for the verification mail. This can contain several variables which are replaced by the respective values in the mail:

variable       | description
---------------|--------------------------
`{%NAME%}`     | The user name
`{%MAIL%}`     | The user's email address
`{%PASSWORD%}` | The user's password in cleartext. Be careful and only include this if necessary.
`{%ID%}`       | The registration session ID which can be used to retrieve the chaptcha.

## Obtain