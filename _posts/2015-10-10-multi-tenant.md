---
layout: post
title:  "Extending Joined Node for a multi-tenant platform"
date:   2015-10-10 15:22:22
published: false
---

_Run untrusted customer code in a multi-tenant trading platform_

Let's consider a trading platform that allows customers to define algorithmic trading strategies by writing custom code. This is an example of a class of multi-tenant systems that allow extensibility through custom code, and require a secure way of running that code in an isolated way. Flybase itself is another good example of such system: we run custom authorization rules and DB scripts on behalf of our subscribers.

Tasks are a great way to run such untrusted code because they provide strong isolation guarantees.

To provide a simple example, let's assume each of the customers of the trading platform defined a JavaScript function that returns an trade order disposition when called with the latest stock price information:

{% highlight javascript %}
	var algorithms_per_customer = {
	'john': 'return function (ctx, callback) {' +
		'  callback(null, ctx.data.price > 10.0 ? "SELL" : "HOLD");' +
	'}',
	'fred': 'return function (ctx, callback) {' +
		'  callback(null, ctx.data.price < 5.0 ? "BUY" : "HOLD");' +
	'}'
};     
{% endhighlight %}

Your application has no control over the code your customers write. Some problems that you must account for when running untrusted code on behalf of your customers is data isolation between customers and fair consumption of computing resources. These are the exact problems that Tasks address. By executing your customer code using Tasks you benefit from the isolation guarantees they provide.

This is how you could execute your customer code using Tasks:

{% highlight javascript %}
	var request = require('request');
	var price = Math.random() * 20;
	
	for (var customer in algorithms_per_customer) {
		var algorithm = algorithms_per_customer[customer];
		request({
			method: 'POST',
			url: 'https://api.joinednode.com/run/' + customer + '?price=' + price,
			headers: {
				Authorization: 'Bearer eyJhbGciOiJIUzI1NiIsImtpZCI6IjIifQ.eyJqdGkiOiI4ZDQ3YmJjMDA3MTQ0ZTI3OTBjYjlmNTZlNTNhNThhNCIsImlhdCI6MTQ0Mjc3MzA0MCwiY2EiOltdLCJkZCI6MSwidGVuIjoiL153dC1mcmVla3JhaS1tZV9jb20tWzAtMV0kLyJ9.cZbPQzO08-8fMUiYtDIHQl-tdvZVwjpo5HoRWawxvW0'
			},
			data: algorithm
		}, function (error, res, body) {
			// process error or customer's trade disposition in body
		});
	} 
{% endhighlight %}

Take note of the following in the sample above:

1. Each customer's strategy is executed through an HTTPS request to Tasks.

2. Each request to Tasks specifies the customer name as the Task container to use. This means that code of every customer will execute within its own isolation boundary and cannot interact with or affect code or data of other customers.

3. The price information is passed as URL query parameter and propagated to the JavaScript function as part of context.

4. The code of the JavaScript function to run itself is passed in the payload of the HTTPS POST request.

5. The call to Tasks is authenticated using your own webtask token.

6. Using Tasks to execute untrusted code in a secure and isolated way was the primary motivation for this technology, and this is how we are using it internally at Flybase. Tasks solve the difficult problem of running untrusted code in a multi-tenant system and let you spend more time on the logic of your application.