---
layout: default
title: Scaling .onion sites
---

A while ago a question was asked on the
[tor-talk](https://lists.torproject.org/cgi-bin/mailman/listinfo/tor-talk)
mailing list about "scaling a .onion site."

First off, it's pretty neat that enough people are using Tor [Hidden
Services](https://www.torproject.org/docs/hidden-services.html.en) such that
*scaling* a site running a Hidden Service is required. 

I figured I would write up a little guide on how to set up a "scalable hidden
service" to help those looking to build genuinely interesting services out
there.

## Step 1

Actually setting up a Hidden Service for the first time is likely the biggest
hurdle most people will face. Creating a useful Hidden Service is a much bigger
challenge than creating a scalable one. To accomplish this I highly recommend
**[this guide](https://www.torproject.org/docs/tor-hidden-service.html.en)**
from the Tor project.


## Step 2

In my opinion, the easiest way to scale a hidden web server is to add *more*
hidden web servers. This can be accomplished with a [load
balancer](https://en.wikipedia.org/wiki/Load_balancing_(computing)) sitting in
front of the web servers. In the case of a Hidden Service, you would simply
point the Hidden Service at the load balancer instead of the web server itself.
This can offer additional benefits in the way of checking the availability of
the backend web server or even showing a helpful "sorry the site is down" web
page.

I've gone ahead and created an example in a repository called
[scalable-onions](https://github.com/warms0x/scalable-onions) of this
technique. In the repository I've not included any of the details of setting up
a Hidden Service, I'll leave that up to the ready him/herself.



Packages to install:

* haproxy
* thttpd

