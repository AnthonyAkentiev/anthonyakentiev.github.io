---
layout: post
title: "What technologies and services i'm currently using"
date: 2014-04-17 01:14
comments: true
categories: [web,nodejs,frameworks]
---

First thing people ask when you tell them about your project is "what technology STACK do you use?".
Let me begin:

# Stack
Stack that i am currently using is called ["MEAN"](http://www.mean.io)
(MongoDB + Express + AngularJS + NodeJS) for short. 
Actually, I use different DataBases for different things:

For example - i use **Redis** as a quick key-value store and **Memcache** as a cache.

## Backend
As i sayed before - i use NodeJS with **npm** as a default package manager.
I use **Express** for routing and processing, **Cluster** for load-balancing, **Winston** for logging, **Mocha** for tests, **Nconf** for configuration.

## Frontend
I use Yo for templates, Bower/NPM as package managers, Grunt as a "task runner".
To build my web-site for debugging i just type:

```bash
     grunt server
```
This starts web server on localhost to connect to.

To build non-optimized version i just type:

```bash
     grunt build 
```

To build optimized version i type:

```bash
     grunt release 
```

This command does:
less, cssmin, concat, ngmin, uglify, htmlmin, imagemin...

# Online Services
Services i use are:
Amazon: EC2, CloudFront, S3, Route53.

Very cool service i use for online monitoring and performance tuning is [NewRelic](http://newrelic.com/). 
They e-mail me each time my server is "too busy" or down ))
I will soon write an article about that. It is worth that. 

{% img http://anthonyakentiev.github.io/images/availability.png %}

{% img http://anthonyakentiev.github.io/images/perf.png %}

I highly recommend [browserstack](browserstack.com) service to get screenshots of your site from different machines/browsers automatically.
