---
layout: post
title: "How you'd design this system (pt.1)?"
date: 2013-11-23 00:40
comments: true
categories: [nodejs, web, https]
---
I met one of my friends today. We've been discussing (for about 2 hours) web processing system he has designed. I am going to show you (and him) my vision of that product and a little bit of real working code. Of course, i am not aware of all requirements and constraints (he-he), so that is just a sketch.

##Reqs:
1. System MUST provide API that can be accessed remotely.
2. System MUST control millions of computers in the CLOUD.
3. System MUST control servers using IPMI interface. 
4. System MUST be "secure".
5. System MUST monitor servers and collect statistics (kpi, load avg. etc)
6. System MAY be scalable.
7. System MUST have sophisticated tools for data monitoring:
     * It should allow us to "monitor node15 and collect load avg if temperature exceeds 15 degrees".
     * It should allow us to "switch node92 off when load avg. exceeds 80% over 1 hour".
     * It should allow us to "alert if node41 is not responding".

###Simplifying: we are developing some kind of 'watching' application that collects data and applies actions if needed. 

How i see that system:

1. Watcher daemon: must collect data into Core, apply actions (IPMI) and pull tasks from Core.  
2. Core: must collect data and provide Watcher with tasks. 
3. Frontend Servers (let me call them like that): must process request from Clients and put them into Core.
4. Clients: have to be able to send commands and receive response to Frontend Servers.
5. High-privileged clients: admins. 

## Core
Contains 2 key components: DB and QUEUE.
DB can be NoSQL if we can lessen some ACID requirements and want speed (and ease of use) over functionality. Read about 'relaxed consistency'.  
I will write a post about that later.

Some folks simply do "always say NoSQL if have no opinion" and don't understand key NoSQL features. Pure RDBMS (SQL, PostgreSQL …) is needed when you want transactions + complicated queries and so on. NoSQL is always simpler under the hood because it is more like traditional FileSystem (documents) and are easy to use.   

The best article i've ever seen on that topic is [here](http://blog.nahurst.com/visual-guide-to-nosql-systems):

> One of the primary goals of NoSQL systems is to bolster horizontal scalability. To scale horizontally, you need strong network partition tolerance which requires giving up either consistency or availability. NoSQL systems typically accomplish this by relaxing relational abilities and/or loosening transactional semantic.

QUEUE (RabbitMQ, ZeroMQ …) provides us with fast "collect tasks" and "get tasks" features. Usually queues are implemented in C++ and allocates a lot of memory. So if your Core will be powered down or will crash -> all information will be lost. 
If we need 100% guarantee that no information will be lost - we should use SQL DB (see above) and keep all task in single DB instead of queue. 

###FRONTEND is a stateless RESTful HTTPS server. 
* Stateless means it keeps no data. Stateless is crucial for us. No one knows which server client will use next second. It is possible to 
add balancer in front of multiple HTTP(S) servers or do Round-Robin DNS: 
{% img http://support.novell.com/techcenter/articles/img/ana2000050206.gif %}

* REST-ful means that all requests can be sent using standard GET/POST request. All URLS are nouns, not verbs:

Example:
     https://my.com/servers/134/temperature_detectors/65  

It is not a good idea to use such URL style (with verbs): 
     https://my.com/set-server-temp?id=134&temperature=65

See great article on URL naming [here](http://apigee.com/about/content/web-api-design)

* HTTPS means we can authenticate client (using client-side certificate). And this is done automatically.
The certificate can be self-signed or issued by CerticiationAuthority (Verisign, Thawte...).

**To be continued…**



