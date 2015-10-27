---
layout: post
title:  "Using Joined Node to Send SMS Messages With Twilio"
date:   2015-10-27 15:22:22
published: true
---

One handy Task that we use often on projects is sending SMS messages using Twilio.

To make this quick, we set up a task that we can send the message to.

First, create a [Joined Node account](https://app.joinednode.com/signup), if you haven't already done so.

Then, log in and create a new Task.

In the Task IDE, you will see an option for title, code and environment, add the following parameters to your environment:

```js
{
"TWILIO_AUTH_TOKEN":"YOUR-TWILIO-TOKEN",
"TWILIO_ACCOUNT_SID":"YOUR-TWILIO-ACCOUNT-SID",
"TWILIO_NUMBER":"A-NUMBER-FROM-YOUR-TWILIO-ACCOUNT"
}
```

Replace `YOUR-TWILIO-TOKEN` with your Twilio Account Token, `YOUR-TWILIO-ACCOUNT-SID` with your Twilio Account SID, and `A-NUMBER-FROM-YOUR-TWILIO-ACCOUNT` with a phone number from your Twilio account.

Now, let's create our Task:


```js
var request = require('request');

return function (context, callback) {
	var required_env_params = ['TWILIO_AUTH_TOKEN', 'TWILIO_ACCOUNT_SID', 'TWILIO_NUMBER'];
	for (var p in required_env_params)
		if (!context.env[required_env_params[p]])
			return callback(new Error('The `' + required_env_params[p] + '` parameter must be provided in your env   settngs.'));

	var required_params = ['to', 'message'];
	for (var p in required_params)
		if (!context.data[required_params[p]])
			return callback(new Error('The `' + required_params[p] + '` parameter must be provided.'));

	request({ 
		url: 'https://api.twilio.com/2010-04-01/Accounts/' + context.env.TWILIO_ACCOUNT_SID + '/Messages', 
		method: 'POST',
		auth: {
			user: context.env.TWILIO_ACCOUNT_SID,
			pass: context.env.TWILIO_AUTH_TOKEN
		},
		form: {
			From: context.env.TWILIO_NUMBER,
			To: context.data.to,
			Body: context.data.message
		}
	}, function (error, res, body) {
		callback(error, body);
	});
}
```

This task will use the variables we stored in the environment box, called using `context.env.KEY` and then check our passed data for the message and who to send the text to.

When you hit save, you will get a URL that you can call, the URL is created as `https://api.joinednode.com/YOUR-UNIQUE-CONTAINER/UNIQUE-TASK-URL`, you can then make a cal to your URL from anywhere and it would send an SMS message as long as it passed the `to` and `message` variables.

You would then trigger an SMS message using a POST request:

```js
<form method="POST" action="YOUR-JOINEDNODE-URL">
	<input type="text" name="to">
	<input type="text" name="message">
	<button type="submit">Send</button>
</form>
```

Sending as a form is one example, but you would probably send it using javascript form the browser:

```js
var url = "YOUR-JOINEDNODE-URL";
var type = "POST";
var data = {
	"to": "PHONE-NUMBER-TO-TEXT",
	"message": "Hello from Joined Node"	
};
var req = new XMLHttpRequest();
req.open(type, url, true);
req.onload = function() {
	if (req.status >= 200 && req.status < 400){
		var res = JSON.parse( req.responseText );
		callback( res );
	}
};
req.send( data );
```

You have options that you can use, any method that you can use to POST data, can be used to trigger an SMS message.