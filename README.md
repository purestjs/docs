
# Purest

[![npm-version]][npm] [![travis-ci]][travis] [![coveralls-status]][coveralls] [![codecov-status]][codecov]

Purest is a generic REST API client library that can be used with various underlying HTTP client libraries:

```js
var request = require('request')
var purest = require('purest')({request})
var google = purest({provider: 'google'})

google
  .query('youtube')
  .select('channels')
  .where({forUsername: 'RayWilliamJohnson'})
  .auth('[ACCESS_TOKEN]')
  .request((err, res, body) => {})
```

Purest implements expressive API and extensive configuration data structure to ensure seamless communication with any REST API provider in a consistent and straightforward way. It also can be used with Promises and regular callbacks.



  [npm-version]: http://img.shields.io/npm/v/purest.svg?style=flat-square (NPM Package Version)
  [travis-ci]: https://img.shields.io/travis/simov/purest/master.svg?style=flat-square (Build Status - Travis CI)
  [coveralls-status]: https://img.shields.io/coveralls/simov/purest.svg?style=flat-square (Test Coverage - Coveralls)
  [codecov-status]: https://img.shields.io/codecov/c/github/simov/purest.svg?style=flat-square (Test Coverage - Codecov)

  [npm]: https://www.npmjs.com/package/purest
  [travis]: https://travis-ci.org/simov/purest
  [coveralls]: https://coveralls.io/r/simov/purest?branch=master
  [codecov]: https://codecov.io/github/simov/purest?branch=master
