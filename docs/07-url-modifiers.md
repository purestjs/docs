
# URL Modifiers

Purest supports a few tokens that we can embed into our provider's domain and path configurations.


## Domain Modifiers

Currently there is only one domain modifier that you can use called `[subdomain]`:

```js
"mailchimp": {
  "https://[subdomain].api.mailchimp.com": {...}
},
"salesforce": {
  "https://[subdomain].salesforce.com": {...}
}
```

The `subdomain` value is usually a user specific data that needs to be added to the domain dynamically.

We can set that value in various ways:

```js
// set it directly in the config
"salesforce": {
  "https://[subdomain].salesforce.com": {
    "__domain": {
      "subdomain": "us2"
    }
  }
}
```

```js
// Set it in the constructor
var salesforce = purest({provider: 'salesforce', config, subdomain: 'us2'})
```

```js
// set it on each request
salesforce
  .get('me')
  .options({subdomain: 'us2'})
  .request((err, res, body) => {})
// or
salesforce.get('me', {subdomain: 'us2'}, (err, res, body) => {})
```

> The `subdomain` key specified directly for the request overrides the one specified in the constructor, and the one specified in the configuration.


## Path Modifiers

The path modifiers are tokens that we can embed into our path definitions:

```js
"basecamp": {
  "https://basecamp.com": {
    "[subpath]/api/[version]/{endpoint}.[type]": {
      "__path": {
        "alias": "__default",
        "version": "v1"
      }
    }
  }
}
```

Having the above configuration we can request the `https://basecamp.com/123/api/v1/people/me.json` endpoint like this:

```js
var basecamp = purest({provider: 'basecamp', config})
basecamp
  .get('people/me')
  .options({subpath: '[USER_ID]'})
  .request((err, res, body) => {})
```

Supported path modifiers are:

- `[subpath]` arbitrary string placed in the path
- `[version]` used for specifying the API version
- `[type]` data type of the response, defaults to `json`
- `{endpoint}` the endpoint to request, *notice the `{}`*

We can set either one of this path modifiers in various places *(except the `{endpoint}` one)*:

```js
// set it directly in the config
"basecamp": {
  "https://basecamp.com": {
    "[subpath]/api/[version]/{endpoint}.[type]": {
      "__path": {
        "alias": "__default",
        "subpath": "some default value",
        "version": "v1",
        "type": "xml"
      }
    }
  }
}
```

```js
// set it in the constructor
var basecamp = purest({provider: 'basecamp', config,
  subpath: '123', version: 'v1.1', type: 'xml'})
```

```js
// set it on each request
basecamp
  .get('me')
  .options({subpath: '456', version: 'v1.2', type: 'xml'})
  .request((err, res, body) => {})
// or
basecamp.get('me', {
  subpath: '123', version: 'v1.1', type: 'xml'
}, (err, res, body) => {})
```

> Path modifiers specified directly for the request override the ones specified in the constructor, and the ones specified in the configuration.
