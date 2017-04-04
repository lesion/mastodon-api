# Mastodon API

[Mastodon](https://github.com/tootsuite/mastodon/) API Client for node

# Installing

TODO

# TODO

* Implement Streaming API

# Usage:

## Authorization

For getting an access token, please take a look into [examples/authorization.js](https://github.com/vanita5/mastodon-api/blob/master/examples/authorization.js).

For more information, please take a look on the wiki [here](https://github.com/jessicahayley/node-mastodon/wiki/Getting-an-access_token-with-the-oauth-package)
and [here](https://github.com/tootsuite/mastodon/blob/master/docs/Using-the-API/API.md#apps).

The authorization process works as follows:

1. Hit the `/apps` endpoint to create an OAuth application
2. With the received `client_id` and `client_secret` get an authorization URL
3. Get an access token by hitting the `/oauth/token` endpoint with the authorization code you got from the authorization page

## `Mastodon.createOAuthApp(url, clientName, scopes)`
Makes a call to the `/app` endpoint to create an OAuth app.
Returns the apps `id`, `client_id` and `client_secret`.

These values should be stored and used from now on. Ideally only call this once!

**url**

Optional. The base url of the Mastodon instance. Defaults to `https://mastodon.social/api/v1/apps`

**clientName**

Optional. Defaults to `mastodon-node`

**scopes**

Optional. Defines the scopes of your OAuth app whitespace seperated. Defaults to `read write follow`

## `Mastodon.getAuthorizationUrl(clientId, clientSecret, baseUrl)`
Returns an authorization url for users to authorize your application.
`clientId` and `clientSecret` can be obtained by calling `Mastodon.createOAuthApp(...)` before.

**clientId**

Your `client_id`.

**clientSecret**

Your `client_secret`.

**baseUrl**

Optional. Defaults to `https://mastodon.social`.

## `Mastodon.getAccessToken(clientId, clientSecret, authorizationCode, baseUrl)`
After authorizing your OAuth application via the authorization URL from `Mastodon.getAuthorizationUrl(...)`
you'll get the authorization code on the website, which lets us obtain the access token we actually need.

**clientId**

Your `client_id`.

 **clientSecret**

 Your `client_secret`.

 **authorizationCode**

 The authorization code you should have got from the authorization page.

 **baseUrl**

 Optional. Defaults to `https://mastodon.social`.


```javascript
var Mastodon = require('mastodon')

var M = new Masto({
  access_token: '...',
  timeout_ms: 60*1000,  // optional HTTP request timeout to apply to all requests.
  api_url: 'https://gay.crime.team/api/v1/', // optional, defaults to https://mastodon.social/api/v1/
})
```

# node-mastodon API:

## `var M = new Mastodon(config)`

Create a `Mastodon` instance that can be used to make requests to Mastodon's APIs.

If authenticating with user context, `config` should be an object of the form:
```
{
  access_token: '...'
}
```

## `M.get(path, [params], callback)`
GET any of the REST API endpoints.

**path**

The endpoint to hit.

**params**

(Optional) parameters for the request.

**callback**

`function (err, data, response)`

- `data` is the parsed data received from Mastodon.
- `response` is the [http.IncomingMessage](http://nodejs.org/api/http.html#http_http_incomingmessage) received from Mastodon.

## `M.post(path, [params], callback)`

POST any of the REST API endpoints. Same usage as `T.get()`.

## `M.getAuth()`
Get the client's authentication tokens.

## `M.setAuth(tokens)`
Update the client's authentication tokens.

-------

# Examples

### Reading the home timeline
```javascript
M.get('timelines/home', {}).then(resp => console.log(resp.data))
```

### Upload an image and attach it to a tweet
```javascript
var id;
M.post('media', { file: fs.createReadStream('path/to/image.png') }).then(resp => id = resp.data.id)
M.post('statuses', { status: '#selfie', media_ids: [id] });
```

-------

# Advanced

You may specify an array of trusted certificate fingerprints if you want to only trust a specific set of certificates.
When an HTTP response is received, it is verified that the certificate was signed, and the peer certificate's fingerprint must be one of the values you specified. By default, the node.js trusted "root" CAs will be used.

eg.
```js
var M = new Mastodon({
  access_token:         '...',
  trusted_cert_fingerprints: [
    '66:EA:47:62:D9:B1:4F:1A:AE:89:5F:68:BA:6B:8E:BB:F8:1D:BF:8E',
  ]
})
```

# License

This software is a fork of [twit](https://github.com/ttezel/twit) and [node-mastodon](https://github.com/jessicahayley/node-mastodon).

Thanks for your awesome work <3

```
(The MIT License)

Copyright (c) 2017 vanita5 <mail@vanit.as>

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
```
