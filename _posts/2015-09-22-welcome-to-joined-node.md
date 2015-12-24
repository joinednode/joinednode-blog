---
layout: post
title:  "Welcome to Joined Node!"
date:   2015-09-22 23:33:42
---

## What Exactly Is Joined Node?

**Joined Node** is a simple, lightweight, and secure way of running isolated backend code that removes or reduces the need for a backend.

It is a secure and cost efficient solution to the challange of supporting **extensibility through custom code in multi-tenant systems**.

It is an ideal companion for **mobile or single page applications that need just a little bit of a backend**.

Finally, Joined Node recipes are a flexible and lightweight mechanism that supports a variety of **integration scenarios**.

The Joined Node service was created and is supported by [Flybase](http://flybase.io). Joined Node Node.js recipes work well with your Flybase-powered apps. In fact, we are using Joined Node internally as a key element of our real-time app backend platform.

With Joined Node, all you need to run code is an HTTP call, that's it, no provisioning. no deployment.

{% highlight javascript %}
return function (cb) {
  cb(null, 'hello world!');
}
{% endhighlight %}


{% highlight javascript %}
your-machine:~ curl https://api.joinednode.com/run/joined-container-52/hello

HTTP status: 200
execution time: 82ms
"hello world!"
{% endhighlight %}

With Joined Node as your microservices provider and Flybase as your real-time backend, you have a winning combination that can't be beat

Please contact us at [support@flybase.io](mailto:support@flybase.io) with any questions or suggestions.
