# Installation
*Installation of the MEANStack.io, with 3 steps you install and start your application.*

## Prerequisites
Node.js, MongoDB, Gulp, Bower, Nodemon and MEANStack-Client.

## Install, it's easy...
* Update Npm and **Install** Gulp, Bower, Nodemon and MEANStack CLI.
```
$ npm update -g npm && npm install -g gulp bower nodemon meanstack-cli
```

* **Create** your application.
```
$ meanstack new <path_app>
```

* **Start** your application.
```
$ cd <path_app> && meanstack serve
```

## Database
By default the connection provider with MongoDB is disabled by not being configured the connection.

### Configure
Edit the file "env.js" stating the details of your connection.

### Enable provider
Edit the file "config/app.js" => providers, uncomment the line 'meanstack/lib/database/DatabaseServiceProvider'.

Result
```js
'providers': [
    ...
    'meanstack/lib/bodyParser/BodyParserServiceProvider',
    'meanstack/lib/cookieParser/CookieParserServiceProvider',
    'meanstack/lib/database/DatabaseServiceProvider', // <= Uncomment.
    'meanstack/lib/response/ResponseServiceProvider',
    ...
]
```

## Common installation errors
Some common errors already solved. An error has occurred in the installation? Please let us know.
[https://github.com/meanstack-io/meanstack.io/wiki/Common-installation-errors](https://github.com/meanstack-io/meanstack.io/wiki/Common-installation-errors)