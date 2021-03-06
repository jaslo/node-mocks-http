[![node-mocks-http logo](https://raw.githubusercontent.com/wiki/howardabrams/node-mocks-http/images/nmh-logo-200x132.png)](https://github.com/howardabrams/node-mocks-http)
---
[![NPM version](https://badge.fury.io/js/node-mocks-http.png)](https://www.npmjs.com/package/node-mocks-http)
[![Build Status](https://travis-ci.org/howardabrams/node-mocks-http.svg?branch=master)](https://travis-ci.org/howardabrams/node-mocks-http)
[![Gitter chat](https://badges.gitter.im/howardabrams/node-mocks-http.png)](https://gitter.im/howardabrams/node-mocks-http)


Mock 'http' objects for testing [Express](http://expressjs.com/)
routing functions, but could be used for testing any
[Node.js](http://www.nodejs.org) web server applications that have
code that requires mockups of the `request` and `response` objects.

## Installation

This project is available as a
[NPM package](https://www.npmjs.org/package/node-mocks-http).

```bash
$ npm install --save-dev node-mocks-http
```

> Our example includes `--save-dev` based on the assumption that **node-mocks-http** will be used as a development dependency..

After installing the package include the following in your test files:

```js
var httpMocks = require('node-mocks-http');
```

## Usage

Suppose you have the following Express route:

```js
app.get('/user/:id', routeHandler);
```

And you have created a function to handle that route's call:

```js
var routeHandler = function( request, response ) { ... };
```

You can easily test the `routeHandler` function with some code like
this using the testing framework of your choice:

```js
exports['routeHandler - Simple testing'] = function(test) {

    var request  = httpMocks.createRequest({
        method: 'GET',
        url: '/user/42',
        params: {
          id: 42
        }
    });

    var response = httpMocks.createResponse();

    routeHandler(request, response);

    var data = JSON.parse( response._getData() );
    test.equal("Bob Dog", data.name);
    test.equal(42, data.age);
    test.equal("bob@dog.com", data.email);

    test.equal(200, response.statusCode );
    test.ok( response._isEndCalled());
    test.ok( response._isJSON());
    test.ok( response._isUTF8());

    test.done();

};
```

## Design Decisions

We wanted some simple mocks without a large framework.

We also wanted the mocks to act like the original framework being
mocked, but allow for setting of values before calling and inspecting
of values after calling.

## For Developers

We are looking for more volunteers to bring value to this project,
including the creation of more objects from the
[HTTP module](http://nodejs.org/docs/latest/api/http.html).

This project doesn't address all features that must be
mocked, but it is a good start. Feel free to send pull requests,
and a member of the team will be timely in merging them.

If you wish to contribute please read our [Contributing Guidelines](CONTRIBUTING.md).


## Release Notes

Most releases fix bugs with our mocks or add features similar to the
actual `Request` and `Response` objects offered by Node.js and extended
by Express.

[Most Recent Release Notes](https://github.com/howardabrams/node-mocks-http/releases)

* [v1.4.1](https://github.com/howardabrams/node-mocks-http/releases/tag/v1.4.1) - April 14, 2015
* [v1.4.0](https://github.com/howardabrams/node-mocks-http/releases/tag/v1.4.0) - April 12, 2015
* [v1.3.0](https://github.com/howardabrams/node-mocks-http/releases/tag/v1.3.0) - April 5, 2015
* [v1.2.7](https://github.com/howardabrams/node-mocks-http/releases/tag/v1.2.7) - March 24, 2015
* [v1.2.6](https://github.com/howardabrams/node-mocks-http/releases/tag/v1.2.6) - March 19, 2015
* [v1.2.5](https://github.com/howardabrams/node-mocks-http/releases/tag/v1.2.5) - March 5, 2015


License
---

Licensed under [MIT](https://github.com/howardabrams/node-mocks-http/blob/master/LICENSE).
