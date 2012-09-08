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
challenge than creating a scalable one. In order to set up a Hidden Service I highly recommend
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


In this example we'll set up a simple static file serving site with
[thttpd](http://www.acme.com/software/thttpd) as the web server and
[haproxy](http://haproxy.1wt.eu/) as the load balancer. I'm also using
[Tails](https://tails.boum.org/) as my base operating system.


**Packages to install:**

* haproxy (`sudo apt-get install haproxy`)
* thttpd (`sudo apt-get install thttpd`)

---


Configuring `thttpd` is quite easy, the only changes I have made in my example
[thttpd.conf](https://github.com/warms0x/scalable-onions/blob/master/thttpd.conf)
were to disable "chrooting" (which you should use!) and updating the paths to
serve up a directory in my home directory. It is also worth nothing that the
web server is bound to `127.0.0.1` which means it cannot receive connections
from any other host other than the machine that is running the server itself.

Configuring `haproxy` is where the magic happens that helps make the .onion
site scalable. In my example
[haproxy.cfg](https://github.com/warms0x/scalable-onions/blob/master/haproxy.cfg)
I only have one "backend server", but in your set up you would likely have
multiple backend servers which will allow you to serve up more content.

Note the `listen` statement on [line
28](https://github.com/warms0x/scalable-onions/blob/master/haproxy.cfg#L28),
here again we're only listening for connections on `127.0.0.1` and on port
8080. This means that the Tor Hidden Server configuration will be set up to
connect to port 8080 on the local machine. I think this is a good security
measure to undertake as it will ensure that only local connections on the host
running Tor and HAProxy will work.

Another thing to note is that the traffic between HAProxy and the multiple
backends will *not* be encrypted unless you are actually running web servers
with SSL enabled. If you wish to do this you will need to look into running
HAProxy with "TCP Load Balancing" instead of the currently configured "HTTP
Load Balancing."

For running a static site, this combination of HAProxy and multiple web servers
should be enough to scale to additional demand.

## Step 3

If you're running [MediaWiki](https://en.wikipedia.org/wiki/MediaWiki) or some
other "dynamic" web application, then scaling the service will likely involve
more than just adding more web servers.

In the case of MediaWiki, or other PHP-based applications, I suggest looking
into tools like [XCache](http://xcache.lighttpd.net/) or [other PHP
accelerators](https://en.wikipedia.org/wiki/List_of_PHP_accelerators) to serve
each inbound request faster.

Other web applications, like those based off of Django or Rails will have their
own needs or "optimization patterns" that you can follow to get efficiency and
speed per request.


---

## Conclusion

Due to the way that Hidden Services work, you will still need a single
end-point for Tor to connect to, so using a load balancer is your best bet.
There's a lot that you can do to optimize a web application that is completely
unrelated to Tor though, so before you shell out more money for another server,
it's worth learning more about what is exactly causing the slowness in your
hidden service and addressing that.
