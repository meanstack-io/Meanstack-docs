# Request Validation
*Request Validation for middleware*

---

- [**Introduction**](#introduction)
- [**Create the request validation**](#createtherequestvalidation)
- [**Rules**](#rules)
  - [Required](#required)
  - [Min](#min)
  - [Max](#max)
  - [Equal](#equal)
  - [Boolean](#boolean)
  - [Email](#email)
- [**Labels**](#labels)
- [**Message**](#message)
- [**Error message for the field**](#errormessageforthefield)
- [**Status Code returned if error**](#statusCodereturnediferror)
- [**Using in controller**](#usingincontroller)

---

## Introduction
Validate request in the middleware passing only rules for validation, thus avoiding confusing validations and dirty code.

## Create the request validation
You can create a request validation like this.
```
$ meanstack make:request <requestName>
```

> **Note:** Organize your validations as needed, but it is recommended that you associate the requests with the crud or with the crud method. Example documentation or createDocumentation.

## Rules

* [Required](#required)
* [Min](#min)
* [Max](#max)
* [Equal](#equal)
* [Boolean](#boolean)
* [Email](#email)

#### required
The field under validation must be present in the input data and not empty. A field is considered "empty" if one of the following conditions are true:

* The field is undefined.
* The value is null.
* The value is an empty string.

#### min:value
The field under validation must have a minimum value. Valid Strings or numerics.

#### max:value
The field under validation must be less than or equal to a maximum value. Valid Strings or numerics.

#### equal:field
The given field must match the field under validation.

#### boolean
The field under validation must be able to be cast as a boolean. Accepted input are true, false, "true", "false",  1, 0, "1", and "0".

#### email
The field under validation must be formatted as an e-mail address.

---

#### Example usage
```javascript
'use strict';

var validator = require('meanstack/lib/validation');

var signUp = function () {

    /**
     * Rules. [Required]
     *
     * @type {{username: string, email: string, password: string, repassword: string, terms: string}}
     */
    this.rules = {
        username: 'required|min:3|max:25',
        email: 'required|email',
        password: 'required|min:6',
        repassword: 'required|min:6|equal:password',
        terms: 'boolean|required',
    };
};

module.exports = validator(signUp);
```

## Labels
Customizing the field name for the error message. When some validity is invalid, an error message is returned. Example "Field username is required !", "Field password accept max 6 characters !". If you need to change the field name in the message to "Field user is required !"

```javascript
    /**
     * Labels of attributes. [Optional]
     *
     *
     * @type {{username: string, repassword: string}}
     */
    this.labels = {
        username: 'user',
        repassword: 'confirm password'
    };
```

## Message
Customizing error message. In the error message the index ":attribute" is replaced by the field name and the ":param" by the validation parameter.

```javascript
    /**
     * Messages. [Optional]
     *
     * @type {{min: string}}
     */
    this.message.min = ':attribute required min :param characters ! Thks';
```

## Error message for the field
Customizing only the field error message.

```javascript
    /**
     * Custom message for field. [Optional]
     *
     * @type {{{email: string}}}
     */
    this.fieldMessage.email =  {
        email: 'Email :value invalid !!!'
    };
```

## Status Code returned if error
Assigning the status of the error returned if the validation is invalid, by default the status is 403.

```javascript
    /**
     * Status Code return. [Optional]
     *   Default: 403
     *
     * @type {number}
     */
    this.code = 401;
```

## Using in controller
The request will only enter the controller if all fields are valid.

```javascript
'use strict';

var meanstack = require('meanstack'),
    router = meanstack.Router(),
    requestSignUp = require('../requests/auth/sign-up'),
/**
 * Strategy local sign up.
 */
router.post('/signup', requestSignUp, function (req, res) {
    // All validated
    // req.body
})
```
