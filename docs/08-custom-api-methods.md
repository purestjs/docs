
# Custom Chain API Methods

Purest comes with just a few of pre-configured method aliases to use with its [Chain API][chain-api]:

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

> The actual methods are on the left, and their aliases are to the right.

Using the above configuration the following *Chain API* calls are identical:

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

> Notice that we called the `select()` method instead of `get()`.

However you may want to define your own method aliases:

```js
var facebook = purest({provider:'facebook', config,
  methods: {
    method: {get: ['loot']},
    custom: {request: ['submit']}
  }
})
```

Then the following code can be used:

```js
facebook
  .loot('me')
  .submit((err, res, body) => {})
```

> **Note:** Keep in mind that you should not use the actual method names as alias names.

> **Note:** Your alias methods override any previously defined aliases with the same name.

TODO: Add example about defining custom methods


  [chain-api]: ...
