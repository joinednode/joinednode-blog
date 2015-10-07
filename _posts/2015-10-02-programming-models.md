---
layout: post
title:  "The Joined Node Programming Model"
date:   2015-10-02 15:22:22
---

We want Joined Node Tasks to be hyper-flexible, while also following a set programming model. With that in mind, there are three types of tasks you can run.

### 1. Function with callback

In the simplest mechanism, you must provide JavaScript code that returns a function which accepts a single argument: a `callback`. To indicate completion, the function must call the `callback` with two arguments: an `error`, and the `result`.

{% highlight javascript %}
return function(callback) {
    calback(null, { i_am: 'done '});
}        
{% endhighlight %}

When the `callback` is invoked, the `result` value or an `error` will be serialized as JSON and sent back to the caller as `application/json` content type.

### 2. Function with context

A more advanced version of the programming model allows you to return a function that accepts two arguments: a `context` and a `callback`.

{% highlight javascript %}
return function(context, callback) {
    callback(null, { hello: context.data.name || 'Anonymous' });
}
{% endhighlight %}

The `context` parameter is a JavaScript object with data and optionally body properties.

The `context.data` is a JavaScript object that combines parameters passed to the code via `GET`.

The `context.body` is JavaScript representation of a parsed application/json or application/x-www-form-urlencoded request 
body. It is only present for requests with bodies of appropriate content type.

### 3. Full HTTP control

The most flexible programming model allows you to take full control over the HTTP request and response.

{% highlight javascript %}
return function (context, req, res) {
    res.writeHead(200, { 'Content-Type': 'text/html '});
    res.end('<h1>Hello, world!</h1>');
}        
{% endhighlight %}

The context argument behaves the same way as in the two simpler programming models. Note that this programming model does not have a concept of a callback. Ending the HTTP response indicates completion.

### In Conclusion

Our programming model allows for flexibility in building your Joined Node tasks while also sticking to a set model, this helps keep apps streamlined.

```javascript
return function(callback) {
    calback(null, { i_am: 'done '});
} 
```

