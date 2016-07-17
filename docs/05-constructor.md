
# Constructor

The Purest's constructor accepts a few options, where only the `config` and the `provider` key being required.

## config

Object containing [provider configuration][provider-configuration].

> **required** `{facebook: {}, twitter: {}, ...}`

## provider

Provider name matching one of the provider keys found in the configuration object.

> **required** `'facebook'` | `'twitter'` ...

## api

Specify which API to use with this instance. To use the *[Basic API][basic-api]* specify `api: 'basic'` otherwise the *[Chain API][chain-api]* will be used.

> optional `'basic'` | `'chain'`

## alias

Default [path alias][path-alias] to use with this instance.

> optional `'some'` | `'alias'` ...

## defaults

Default request options to pass to each request.

> optional `{headers: {}, oauth: {}, ...}`

## methods

Specify your own method aliases and/or define your own custom methods to use with the *Chain API*.

> optional, see [Custom Chain API Methods][custom-methods] documentation.

## subdomain, subpath, version, type

Default [path modifiers][path-modifiers] to use with this instance.

> optional `'string'`

## key, secret

OAuth 1.0 application `consumer_key` and `consumer_secret` to use for all requests made through that instance.

> optional `'string'`


  [custom-methods]: https://simov.gitbooks.io/purest/content/docs/08-custom-api-methods.html
  [provider-configuration]: https://simov.gitbooks.io/purest/content/docs/06-rest-api-config.html
  [basic-api]: https://simov.gitbooks.io/purest/content/docs/03-provider-api.html#basic-api
  [chain-api]: https://simov.gitbooks.io/purest/content/docs/03-provider-api.html#chain-api
  [path-alias]: https://simov.gitbooks.io/purest/content/docs/06-rest-api-config.html#alias
  [path-modifiers]: https://simov.gitbooks.io/purest/content/docs/07-url-modifiers.html
