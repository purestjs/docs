
# Complete Example

For this example we are going to use the [request][request] module as underlying HTTP client library. We will also include the basic configuration from [@purest/providers][purest-providers]. Lastly we are going to create a provider instance for Google:

```js
var request = require('request')
var purest = require('purest')({request})
var config = require('@purest/providers')
var google = purest({provider: 'google', config})
```

In this example we are going to request the [channels][youtube-channels] endpoint of the YouTube API:

```js
google
  .query('youtube')
  .get('channels')
  .qs({forUsername: 'CaseyNeistat'})
  .auth('[ACCESS_TOKEN]')
  .request((err, res, body) => ())
```

Here is how the related portion of the Google's configuration looks like in [@purest/providers][purest-providers]:

```js
"google": {
  "https://www.googleapis.com": {
    "__domain": {
      "auth": {
        "auth": {"bearer": "[0]"}
      }
    },
    "{endpoint}": {
      "__path": {
        "alias": "__default"
      }
    },
    "youtube/[version]/{endpoint}": {
      "__path": {
        "alias": "youtube",
        "version": "v3"
      }
    }
  }
}
```

> Using the above configuration Purest knows how to construct the absolute URL `https://www.googleapis.com/youtube/v3/channels` for the [channels][youtube-channels] endpoint.

Given the above configuration we can also use the so called `__default` path:

```js
google
  .get('youtube/v3/channels')
  .qs({forUsername: 'CaseyNeistat'})
  .auth('[ACCESS_TOKEN]')
  .request((err, res, body) => ())
```

> Notice that the `__default` path can be used without specifying path alias through the `query()` method.

Alternatively we can provide the absolute URL:

```js
google
  .get('https://www.googleapis.com/youtube/v3/channels')
  .qs({forUsername: 'CaseyNeistat'})
  .auth('[ACCESS_TOKEN]')
  .request((err, res, body) => ())
```

Lastly we can create a separate instance specifically for making requests to the YouTube API, using the `alias` option of the Purest's constructor:

```js
var youtube = purest({provider: 'google', config, alias: 'youtube'})

youtube
  .get('channels')
  .qs({forUsername: 'CaseyNeistat'})
  .auth('[ACCESS_TOKEN]')
  .request((err, res, body) => ())
```

> Notice that using the `.query('youtube')` method for each request is no longer required.

## Basic API

Alternatively we can use the so called *Basic API*:

```js
var request = require('request')
var purest = require('purest')({request})
var config = require('@purest/providers')
var google = purest({provider: 'google', config, api: 'basic'})
```

> Notice the `api: 'basic'` key used in the Purest's constructor to specify which API to use for this instance.

Then we can use it like this:

```js
google.get('channels', {
  alias: 'youtube',
  auth: {bearer: '[ACCESS_TOKEN]'},
  qs: {forUsername: 'CaseyNeistat'}
}, (err, res, body) => {})
```

> Notice the `alias` key in the options object used for specifying which path alias to use.

## Promise API

Using the Promise API with either the *Chain* or the *Basic* API is easy:

```js
var request = require('request')
var promise = require('bluebird')
var purest = require('purest')({request, promise})
var config = require('@purest/providers')
var google = purest({provider: 'google', config})
```

> Notice the `promise` key passed to the Purest module.

Then we can start making requests:

```js
var req = google
  .query('youtube')
  .select('channels')
  .where({forUsername: 'CaseyNeistat'})
  .auth('[ACCESS_TOKEN]')
  .request()

req
  .catch((err) => {})
  .then((result) => {})
```


  [youtube-channels]: https://developers.google.com/youtube/v3/docs/channels/list
  [purest-providers]: https://github.com/purestjs/providers
  [request]: https://github.com/request/request
