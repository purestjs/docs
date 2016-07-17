
# REST API Configuration

This is how `Facebook` is configured in [@purest/providers][purest-providers]:

```js
var config = {
  "facebook": {
    "https://graph.facebook.com": {
      "__domain": {
        "auth": {
          "auth": {"bearer": "[0]"}
        }
      },
      "{endpoint}": {
        "__path": {
          "alias": "__default"
        }
      }
    }
  }
}
```

That's the bare minimum configuration that we want to have for a provider.

With this configuration we can request the user's profile like this:

```js
var facebook = purest({provider: 'facebook', config})
facebook
  .get('me')
  .auth('[ACCESS_TOKEN]')
  .request((err, res, body) => {})
```

This will result in requesting the `https://graph.facebook.com/me` endpoint of the Facebook API. Purest will also send the `Authorization: Bearer [ACCESS_TOKEN]` header.


## Domain

Each provider configuration should contain at least one domain in it, though it can also contain multiple domains:

```js
"google": {
  "https://www.googleapis.com": {...},
  "https://maps.googleapis.com": {...},
  "https://www.google.com": {...},
  "https://accounts.google.com": {...}
}
```

Each domain can contain one optional `__domain` meta key in it for specifying options related to that domain:

```js
"https://graph.facebook.com": {
  "__domain": {
    "auth": {
      "auth": {"bearer": "[0]"}
    }
  }
}
```

In this case we're specifying authentication scheme to use with the `.auth()` method of the *Chain API*.


## Auth

The `auth` key can be used inside `__domain`, `__path` and `__endpoint` meta key. The innermost `auth` configuration key overrides the outer ones.

The `auth` key is designed to be used only with the `.auth()` method of the *[Chain API][chain-api]*. It can contain any valid request options:

```js
// one of the following
"auth": {
  // OAuth1.0
  "oauth": {"token": "[0]", "secret": "[1]"}
  // OAuth2 header
  "auth": {"bearer": "[0]"}
  // basic auth
  "auth": {"user": "[0]", "pass": "[1]"}
  // custom header
  "headers": {"Authorize": "X-Shopify-Access-Token [0]"}
  // querystring
  "qs": {"access_token": "[0]"}
  // querystring
  "qs": {"api_key": "[0]", "api_secret": "[1]"}
  // combination of querystring + header
  "qs": {"client_id": "[0]"}, "headers": {"Authorization": "OAuth [1]"}
}
```

Having such configuration we can pass just the string values to the `.auth()` method:

```js
var facebook = purest({provider: 'facebook', config})
facebook
  .get('me')
  .auth('[ACCESS_TOKEN]')
  .request((err, res, body) => {})
```

Alternatively the `auth` key can be array of authentication schemes:

```js
"auth": [
  {"auth": {"bearer": "[0]"}},
  {"auth": {"user": "[0]", "pass": "[1]"}}
]
```

In this case Purest will pick authentication scheme based on the arguments count:

```js
// will use OAuth2 Bearer header
facebook.auth('[ACCESS_TOKEN]')
// will use Basic authentication
facebook.auth('[USER]', '[PASS]')
```


## Path

Each domain can have multiple paths in it:

```js
{
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
      "plus/[version]/{endpoint}": {
        "__path": {
          "alias": "plus",
          "version": "v1"
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
}
```

With the above configuration we can use the path aliases defined for the Google+ and the YouTube API to remove the clutter:

```js
var google = purest({provider: 'google', config})
google
  .query('youtube')
  .get('channels')
  .qs({mine: true})
  .auth('[ACCESS_TOKEN]')
  .request((err, res, body) => {})
```

This will result in requesting the `https://www.googleapis.com/youtube/v3/channels` endpoint of the YouTube API.

In the exact same way we can request the `https://www.googleapis.com/plus/v1/people/me` endpoint of the Google+ API:

```js
google
  .query('plus')
  .select('people/me')
  .auth('[ACCESS_TOKEN]')
  .request((err, res, body) => {})
```

## Alias

Each path should have a `__path` meta key in it specifying `alias` name to use to access that path *(otherwise it won't be accessible)*:

```js
"google": {
  "https://www.googleapis.com": {
    "plus/[version]/{endpoint}": {
      "__path": {
        "alias": "plus",
        "version": "v1"
      }
    }
  }
}
```

> All alias names should be unique for that provider!

Having the above configuration we can access the Google+ API in various ways:

```js
// one of the following:
// default alias to use for this instance
var google = purest({provider: 'google', config, alias: 'plus'})
// Basic API
google.get('people/me', {alias: 'plus'}, (err, res, body) => {})
// Chain API
google.query('plus').get('people/me').request((err, res, body) => {})
// OR
google.get('people/me').options({alias: 'plus'}).request((err, res, body) => {})
```

Alternatively the `alias` key can contain a list of names:

```js
"google": {
  "https://www.googleapis.com": {
    "drive/[version]/{endpoint}": {
      "__path": {
        "alias": ["drive", "storage"],
        "version": "v2"
      }
    }
  }
}
```

With the above configuration we can use either one of the specified aliases:

```js
google
  .query('drive')
  .get('about')
  .auth('[ACCESS_TOKEN]')
  .request((err, res, body) => {})
```

```js
// identical to
google
  .query('storage')
  .get('about')
  .auth('[ACCESS_TOKEN]')
  .request((err, res, body) => {})
```


## Endpoint

Each path can contain multiple endpoints in it:

```js
"live": {
  "https://apis.live.net": {
    "__domain": {
      "auth": {
        "auth": {"bearer": "[0]"}
      }
    },
    "[version]/{endpoint}": {
      "__path": {
        "alias": "__default",
        "version": "v5.0"
      },
      "me/picture": {
        "get": {
          "encoding": null
        }
      },
      ".*\\/skydrive\\/files\\/.*": {
        "__endpoint": {
          "regex": true
        },
        "put": {
          "headers": {
            "Content-Type": "application/json"
          }
        }
      }
    }
  }
}
```

With the above configuration every `GET` request to the `me/picture` endpoint will have the `encoding: null` [request option][request-options] set:

```js
var live = purest({provider: 'live', config})
live
  .get('me/picture')
  // no longer required to set this option explicitly
  // .options({encoding: null})
  .request((err, res, body) => {})
```

> The `encoding: null` option is required when using the [request][request] module, and when we expect binary response body, such as image.

Additionally, with the above configuration, each `PUT` request made to the `[USER_ID]/skydrive/files/[FILE_NAME]` endpoint will have the `Content-Type: application/json` header set.

The only difference here is that this a `regex` endpoint definition:

```js
".*\\/skydrive\\/files\\/.*": {
  "__endpoint": {
    "regex": true
  }
}
```

Omitting the `regex: true` key will result in a regular string endpoint.

> Notice the double escape used in the regex string: `.*\\/skydrive\\/files\\/.*`


## Match All

There is one special endpoint that can be used to match all endpoints:

```js
"*": {
  "get": {
    "headers": {"x-li-format": "json"}
  }
}
```

This will result in all `GET` requests made to any of the endpoints in that path having the `x-li-format: json` header set.

Finally one special HTTP method can be used to match all request types:

```js
"*": {
  "all": {
    "headers": {"x-li-format": "json"}
  }
}
```

This will result in having the `x-li-format: json` header being set **always** for that path.


  [purest-providers]: https://github.com/purestjs/providers
  [chain-api]: ...
  [request-options]: https://github.com/request/request#requestoptions-callback
  [request]: https://github.com/request/request
