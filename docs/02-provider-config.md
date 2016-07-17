
# Provider Configuration

Once we have Purest initialized we can create a provider instance. A provider instance needs provider name and provider configuration:

```js
// usually stored in a JSON configuration file
var config = {
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
}
//  both the `provider` and the `config` keys are required
var google = purest({provider: 'google', config})
```

Alternatively we can use the common configuration for all of the pre-configured [providers][purest-providers] in Purest:

```js
// the @purest/providers module contains basic configuration for about 150 providers
var config = require('@purest/providers')
// initialize the provider
var google = purest({provider: 'google', config})
var facebook = purest({provider: 'facebook', config})
var twitter = purest({provider: 'twitter', config})
```


  [purest-providers]: https://github.com/purestjs/providers
