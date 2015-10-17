---
layout: post
title:  "Killing The Backend"
date:   2015-10-16 15:22:22
published: true
image: /images/killing-backend.png
---

One of the exciting things about [Joined Node](https://joinednode.com), and [Flybase](https://flybase.io) is that we are getting further and further into an era where dedicated backends are less important.

Back in June, I wrote a blog post on my [MVPin30](http://mvpin30.com/2015/06/29/fit-stack/) blog, about a development stack I called the [FIT Stack](https://github.com/flybaseio/fit-stack/).

The FIT Stack stands for:

1. Flybase (for the backend)
2. Interface (usually angular.js)
3. Thin servers (reactors, usually built in node.js, when or if needed)

Wanting a resource that lets you easily build the `Thin servers` portion, since you usually don't actually need a server, as most of the work being done is a singular task, you call a backend script that then does some processing like sending emails, SMS messages, storing incoming SMS messages, etc.

Flybase lets you use incoming webhooks, and we're about to release outgoing webhooks, but we wanted to take webhooks further, not just for Flybase but for anything.

Now, you can host your website in Github Pages, on S3, or anywhere, and use a javascript or HTTP call to trigger a Task on Joined Node, this lets you need less resources to do more, just think of that?

Killing the backend means you can do more, with less requirements...

I'm excited about the possibilities that come from this service, combined with other services and can't wait to see what everyone builds.