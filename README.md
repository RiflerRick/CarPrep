As a start for the development in platforms engineering. We will start with understanding a few concepts:

* Difference between http1.0 and http2.0
* gRPC
* Kubernetes and Docker

## Difference between HTTP1.0 and HTTP2.0

This document is inspired from [here](https://legacy.gitbook.com/book/bagder/http2-explained)

### HTTP Today

#### HTTP today is huge
A few ideas regarding HTTP1.0. First of all it is worth noting that http1.0 is huge and that it has a lot of tiny details and options available for later extensions. This has led to a situation where features that were initially little used saw very few implementations and those that did implement the features then saw very little use of them.

#### inadequate use of tcp

HTTP1.1 has a hard time taking full advantage of the power that TCP offers. HTTP Clients and browsers have to be very creative to find solutions that decrease the page load times.

Other attempts that have been going on in parallel over the years have also confirmed that TCP is not easy to replace. 

#### transfer sizes and number of objects

Over the years the amount of data that needs to be retrieved has gradually risen up to and above 1.9MB. Total transfer size has grown up from 800K in 2011 to 2100K in 2015. Similarly the number of requests used on average has also risen. 

#### Latency kills

HTTP1.1 is very latency sensitive. 

#### head of line blocking

HTTP pipelining is a way to send request while waiting for the response to a pervious request. It is very similar to queuing at a counter. In this kind of a scenario it is not very clear how much time is going to be taken by the first request. This is essentially called head of line blocking.

### Things done to overcome latency pains

#### Spriting

Spriting is the term used to describe combining multiple images to form a single larger image. Then using javascript or CSS you can cut out the pieces of the big image in order to show smaller individual images. A site would use this trick for speed. Getting a single big image is far faster using HTTP1.1 as compared to getting multiple smaller images.

Ofcourse this has its downsite for the pages of the site that want to show only a few of the images.

#### Inlining

embedding data urls in the CSS file is another way.

#### Concatenation

Basically concatenating too much content into one file. Again it is ideally easier to get one file then getting multiple files. This is ofcourse a problem for the developers.

#### Sharding

Domain sharding is a technique used to increase the amount of simultaneously downloaded resources for a particular website by using multiple domains. This allows for websites to be delivered faster to users as they do not have to wait for previous set of resources to be downloaded before beginning the next set.

Web browsers traditionally place limits on the amount of concurrent downloads allowed for each domain(2-16). This limit was put in place by the **Internet Engineering Task Force** and is mentioned in the **HTTP1.1 specs**. 

Over the time the limitation was removed and today clients use over 6 to 8 connections per host. But they still have a limit so sites continue to use the techique of domain sharding so that from multiple domains it is actually faster to download resources simply because it is downloaded from different domains simultaneously. 

### Updating HTTP

Because of the problems above http was updated such that we have a system that is:

* less latency sensitive.
* fix pipelining and the head of line blocking issues.
* eliminate the need to keep increasing the number of connections to each host.
* keep all existing interfaces, all content and URI formats and schemes.
* be made within the IETFs HTTPbis working group.

The **IETF or Internet Engineering Task Force** is an organization that develops and promotes internet standards, mostly on the protocol level. They are widely known for the RFC series of memos documenting everything from TCP, DNS and more. 

Within the IETF various working groups are formed with a limited scope to work toward a goal. One such working group was formed in the summer of 2007 called **HTTPbis** working group tasked with creating an update of the HTTP1.1 spec. Within the group the work for the next version of HTTP really started in 2012.

### HTTP2 negotiation over TLS

Next protocol negotiation is the protocol used to negotiate SPDY with TLS servers. Basically when considering migration from HTTP1.1 to HTTP2 it is worth understanding that the exisiting URI schemes for HTTP1.1 cannot be changed since a lot of machines are actually using them. Since the URI schemes are used in HTTP1.1 today we obviously need a way to upgrade the protocol to HTTP2. 

SPDY of google did it using an extension called Next protocol negotiation(NPN). next protocol negotiation is where the server sends a list of protocols to the client asking it for the protocol the client can understand. 

There was a big debate as to whether TLS should be made mandatory with HTTP2. SPDY requires TLS and therefore there was a great push into making TLS mandatory for HTTP however because of lack of consensus TLS was shipped as optional with HTTP2.

NPN was used by google (SPDY) for negotiation over TLS servers. However since NPN was not a standard, IETF created ALPN or application layer protocol negotiation. 

### The HTTP2 protocol

http2 is a binary protocol. http2 is binary to make framing easier. figuring out the start of end of frames is complicated in http1.1 and in general in any text based protocols. By moving away from text based protocols implementation becomes much simpler. 

There are 10 different frame types in http2 and the 2 most fundamental that maps onto http1.1 are **data and headers**. 

#### Streams and stream multiplexing

a stream is an independent, bi-directional sequence of frames exchanged between the client and the server within the http2 connection. 

a single http2 connection can contain multiple concurrently open streams with either endpoint interleaving frames from multiple streams. 

multiplexing streams means packages from many streams are mixed together over the same connection. 

Each stream also has a **priority**(weight), which is used to tell the peer which streams to consider most important. Using the **PRIORITY** frame, a client can also tell the server which other streams that stream depends on. It allows the client to build a priority tree where several child streams may depend on the completion of the parent streams. The priority weights and dependencies can be changed dynamically at run time, which may enable browsers to display the most important images for instance when a user is scrolling down to check for images. 

#### Header compression as HTTP is stateless

Because HTTP is stateless, it makes it repetitive and so a lot of the data from the sent again and again to the clients and this begs for compression. 

#### RST_STREAM frame

One of the drawbacks of HTTP1.1 is that when an http message has been sent off with a content-length of a certain size, you cant easily just stop it. it is possible to simply terminate the connection but then we need to make another connection using a TCP handshake which would require more resources. 

The **RST_STREAM** frame essentially can stop the current message and start anew. 
