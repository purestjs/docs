
# Provider API

## Chain API

Once we have initialized a provider we can start using it through the *[Chain API][chain-api]*:

```js
google
  .query('youtube')
  .get('channels')
  .qs({forUsername: 'CaseyNeistat'})
  .auth('[ACCESS_TOKEN]')
  .request((err, res, body) => ())
```

```js
// or if Purest was initialized with Promises
var req = google
  .query('youtube')
  .get('channels')
  .qs({forUsername: 'CaseyNeistat'})
  .auth('[ACCESS_TOKEN]')
  .request()

req
  .catch((err) => {})
  .then((result) => {})
```

## Basic API

A *[Basic API][basic-api]* is available as well:

```
provider_instance[HTTP_VERB]([endpoint], [options], [callback])
```

One additional `api` key should be passed to the Purest's constructor to specify which API to use with this instance:

```js
var google = purest({provider: 'google', config, api: 'basic'})
```

Then the *[Basic API][basic-api]* can be used like this:

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


  [chain-api]: https://simov.gitbooks.io/purest/content/docs/03-provider-api.html#chain-api
  [basic-api]: https://simov.gitbooks.io/purest/content/docs/03-provider-api.html#basic-api
