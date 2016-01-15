---
layout: post 
published: false
draft: true
title: "Build a Plug and Play Live Blogging System with Twilio, Flybase and Joined Node" 
date: 2015-12-20T07:19:34.192Z 
link: http://blog.flybase.io/2015/12/15/live-blogging-twilio-flybase-joined-node/ 
tags:
  - code
  - links
---

We built a [live blogging app](http://blog.flybase.io/2015/03/23/live-blogging-twilio-data-mcfly/) a few months ago, and it's been used by several projects, but with the launch of our sister service [Joined Node](http://joinednode.com) today, we wanted to rewrite our live blogging app to be even easier to integrate into existing sites.
 
To do this, we are going to build a _Plug and Play_ live blogging widget that can be added to any website, and then use Joined Node to receive incoming SMS messages from Twilio and save them in our Flybase app.
