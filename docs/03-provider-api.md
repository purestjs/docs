
# Provider API

## Chain API

Once you have the provider initialized you can start using the *Chain API*:

```js
google
  .query('youtube')
  .select('channels')
  .where({forUsername: 'CaseyNeistat'})
  .auth('[ACCESS_TOKEN]')
  .request((err, res, body) => ())
```

```js
// or if Purest was initialized with Promises
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

## Basic API

A *Basic API* is also available in the form of:

```
provider_instance[HTTP_VERB]([endpoint], [options], [callback])
```

One additional `api` key can be used in the Purest's constructor to specify which API to use with this instance:

```js
var google = purest({provider: 'google', config, api: 'basic'})
```

Then the *Basic API* can be used like this:

```js
google.get('channels', {
  alias: 'youtube',
  auth: {bearer: '[ACCESS_TOKEN]'},
  qs: {forUsername: 'CaseyNeistat'}
}, (err, res, body) => {})
```

```js
// or if Purest was initialized with Promises
var req = google.get('channels', {
  alias: 'youtube',
  auth: {bearer: '[ACCESS_TOKEN]'},
  qs: {forUsername: 'CaseyNeistat'}
})

req
  .catch((err) => {})
  .then((result) => {})
```
