
# Custom Chain API Methods

Purest comes with just a few pre-configured method aliases to use with the *[Chain API][chain-api]*:

```js
{
  "method": {
    "get"      : ["select"],
    "post"     : [],
    "put"      : [],
    ...
  },
  "option": {
    "qs"       : ["where"],
    "form"     : [],
    "multipart": [],
    ...
    "options"  : []
  },
  "custom": {
    "query"    : [],
    "auth"     : [],
    "request"  : []
  }
}
```

> The actual methods are on the left side and their corresponding aliases are to the right.

Using the above configuration the following *[Chain API][chain-api]* calls are identical:

```js
var facebook = purest({provider: 'facebook', config})
facebook
  .get('me')
  .request((err, res, body) => {})
// same as
facebook
  .select('me')
  .request((err, res, body) => {})
```

> Notice that we can call the `select()` method instead of `get()`.

However you may want to define your own method aliases:

```js
var facebook = purest({provider:'facebook', config,
  methods: {
    method: {get: ['loot']},
    custom: {request: ['submit']}
  }
})
```

Then the following expression is valid:

```js
facebook
  .loot('me')
  .submit((err, res, body) => {})
```

> **Note:** Keep in mind that the actual method names should not be used as aliases.

> **Note:** Your alias methods override any previously defined aliases with the same name.

TODO: Add example about defining custom methods


  [chain-api]: https://simov.gitbooks.io/purest/content/docs/03-provider-api.html#chain-api
