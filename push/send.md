---
layout: default
---

<img src="/img/push-docs/pushlogo.png" style="width: 300px;">
Ionic Push, Sending Messages
-------

Assuming you have registered some devices and uploaded your developer SSL certificates through the CLI, you're ready to 
start sending some push notifications!  There are two main ways of accomplishing this; through the REST API and using 
the <a href="apps.ionic.io">ionic.io</a> dashboard.

## Using the REST API

Ionic.io has an API endpoint which you can use to send push notifications from your backend server (or with a 
<code>curl</code> for testing).

You can send a push notification by making a POST to <strong>https://push.ionic.io/api/v1/push</strong> with the 
following headers.

```javascript
Content-Type: application/json
X-Ionic-Application-Id: YOUR_APP_ID
```

In addition, your POST should authenticate using 
<a href="http://en.wikipedia.org/wiki/Basic_access_authentication">Basic Access Authentication</a>, with no password and 
your <strong>private API key</strong> for a username.

The POST data should be a JSON object of the following format as well (`"ios"`, and `"android"` sections of 
`"notification"` are optional customizations):

```javascript
{
  "platform":"ios,android",
  "tokens":[
    "b284a6f7545368d2d3f753263e3e2f2b7795be5263ed7c95017f628730edeaad",
    "d609f7cba82fdd0a568d5ada649cddc5ebb65f08e7fc72599d8d47390bfc0f20"
  ],
  "notification":{
    "alert":"Hello World!",
    "ios":{
      "badge":1,
      "sound":"ping.aiff",
      "expiry": 1423238641,
      "priority": 10,
      "contentAvailable": true,
      "payload":{
        "key1":"value",
        "key2":"value"
      }
    },
    "android":{
      "collapseKey":"foo",
      "delayWhileIdle":true,
      "timeToLive":300,
      "payload":{
        "key1":"value",
        "key2":"value"
      }
    }
  }
}
```

<strong>Please note: </strong>`"platform"` can be `"ios"`, `"android"`, or `"ios,android"` depending on device targets.

## Using the ionic.io dash

In addition to the API, ionic.io provides a powerful tool for scheduling and targeting push notifications.  This feature
leverages <a href="/identify">Ionic User</a> to let you pare down your users however you see fit.

To get started with it, simply go to https://apps.ionic.io/app/YOUR\_APP\_ID and click <strong>Push</strong>.