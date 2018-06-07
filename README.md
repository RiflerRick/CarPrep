As a start for the development in platforms engineering. We will start with understanding a few concepts:

* Difference between http1.0 and http2.0
* gRPC
* Kubernetes and Docker

## Difference between HTTP1.0 and HTTP2.0

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

