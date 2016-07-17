
# Purest Configuration

## HTTP Client Library

Purest requires HTTP client library to work with:

```js
// use the request module as underlying HTTP client library
var request = require('request')
// initialize Purest (defaults to callbacks)
var purest = require('purest')({request})
```

The `request` key is used to specify the underlying HTTP client library.

## Promises

Additionally we can specify a Promise implementation to use:

```js
// use bluebird
var promise = require('bluebird')
// or use the built-in Promise implementation
var promise = Promise
// initialize Purest (use promises instead of callbacks)
var purest = require('purest')({request, promise})
```

The `promise` key is used to specify the Promise implementation to use.
