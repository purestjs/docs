
# Constructor

The Purest's constructor accepts a few options, where only the `config` and the `provider` key being required.

## config

Object containing provider configuration.

> **required** `{facebook: {}, twitter: {}, ...}`

## provider

Provider name matching one of the provider keys found in the configuration object.

> **required** `'facebook'` | `'twitter'` ...

## api

Specify which API to use with this instance. To use the *Basic API* specify `api: 'basic'` otherwise the *Chain API* will be used.

> optional `'basic'` | `'chain'`

## alias

Default path alias to use with this instance.

> optional `'some'` | `'alias'` ...

## defaults

Default request options to pass to each request.

> optional `{headers: {}, oauth: {}, ...}`

## methods

Specify your own method aliases and/or define your own custom methods to use with the *Chain API*.

> optional, see [Custom Chain API Methods][custom-methods] documentation.

## subdomain, subpath, version, type

Default path modifiers to use with this instance.

> optioanal `'string'`

## key, secret

OAuth 1.0 application `consumer_key` and `consumer_secret` to use for all requests made through that instance.

> optional `'string'`


  [custom-methods]: ...
