---
layout: default
---

<img src="/img/push-docs/pushlogo.png" style="width: 300px;">
Ionic Push Backend (webhook) Setup
-------

Ionic Push lets you register a webhook url with the service so that you can receive device tokens and registration data 
at an API endpoint of your choosing.

## Register your webhook URL
The push service is able to send a webhook to your server with information, including the device token, when a device 
registers, when metadata is updated (such as the user_id) or if a token becomes invalid. Registering your webhook url is 
done via the CLI with the following command:

```bash
$ ionic push webhook_url http://example-server.com/example-api-endpoint
```

When a device registers it will receive a HTTP POST with JSON body. The post will contain an tokens corresponding to 
the device upon registration. It will also contain the `$ionicUser.identify()` keys you attached when registering from 
your client code.  Below is an example POST body:

```javascript
{ 
  received: '2015-03-18T17:21:42.571286',
  user_id: 1337,
  name: 'Test User',
  app_id: 'YOUR_APP_ID',
  _push: {
    android_tokens: [
      'LIST', 'OF', 'ANDROID', 'TOKENS'
    ]
  },
  message: 'I come from planet Ion' 
}
```

<strong>Note: </strong>`"android_tokens"` will be replaced with `"ios_tokens"` when an iOS device is registered.

Our webhook system expects to receive a `2xx` response code from your server and if it doesn't it will retry 3 times at 
30 minute intervals to deliver the webhook before it gives up.  The received key above is the UTC time that we received 
the token registration and can be used to ensure that you are saving the most up-to-date information.

If a device token becomes invalid, the webhook will receive a HTTP POST with JSON body like the following:

```javascript
{
  "ios_token":"b284a6f7545368d2d3f753263e3e2f2b7795be5263ed7c95017f628730edeaad",
  "token_invalid": true
}
```

If a device becomes unregistered, your webhook will receive a POST with the following data.

```javascript
{
  unregister: true,
  received: '2015-03-25T16: 15: 00.773575',
  _push: {
    android_tokens: [
      'LIST', 'OF', 'ANDROID', 'TOKENS'
    ]
  },
  app_id: 'YOUR_APP_ID'
}
```

## All Done!

That's all there is to it!