---
layout: interview
title:  "Tuesdays at 10am: Serverless Architecture and Bursty Traffic"
description: "In our third post in a series on serverless architectures, we talk to long-time Snap CI customers Applauze about why they chose a serverless architecture to provide reliable, cost-efficient and stable architecture for their spiky, demand-driven traffic."
date:   2016-05-03
author: Suzie Prince
index-image-url: screenshots/serverless-architecture/serverless-architecture-applauze.jpg
index-image-alt: 'serverless architecture and burst traffic'
keywords: serverless architecture, aws, lambda, cloudfront, applauze, burst traffic
categories: SERVERLESS-ARCHITECTURE CUSTOMER-STORIES
---


In our third post in a [series on serverless architectures](/categories/serverless-architecture/), we talk to long-time Snap CI customers [Applauze](https://www.applauze.com/) about why they chose a serverless architecture to provide reliable, cost-efficient and stable architecture for their spiky, demand-driven traffic.

#### Let's start with a definition. What does serverless architecture mean to you?

The way we look at it is that our architecture does not include servers: It is server -less. Kind of in the same way our architecture does not include electricity. Obviously, we use electricity but I don’t have to think about it. Like electricty, server-less architecture has become a commodity to me. It's a service just like Heroku. With Heroku, I don't think about the physical server itself, but I am running a server. Serverless architecture is the same but at a different level.


#### Why did you consider a serverless architecture?

There are three reasons why we considered a serverless architecture - performance, cost and stability.

###### Performance
We sell pre-sales tickets for musicians and bands and as a result, we have high, bursty traffic whenever tickets go on sale. These massive changes in demand mean that we can go from very few requests a second to thousands of requests a second very quickly. We need our architecture to be able to scale and support this traffic.

We had hit a performance limit with what we were doing. Caching helped in many cases, but on our biggest days it was insufficient to handle the full demand. We have a high degree of dynamic information (what tickets are available, views and clicks) and there are limits of what normal scaling we could do with our existing Rails on Heroku core systems’ database and Redis connection limits. By replacing the existing architecture and using AWS CloudFront with a simple 10-second cache, we got massive benefits.

###### Cost
The second reason we had an interest in serverless architectures was to get a more predictable and efficient cost structure. Due to the pay-as-you-go model of Lambda, S3 and CloudFront, we don't have to worry about having services pre-scaled up to a certain point beforehand. We know that they will scale when we need them to, and we only pay for what we need.

###### Reliability
Finally, we needed to ensure that our system behaved well in case parts of the system failed. For example, we needed the tour information we show to artists and management to keep working regardless of how many tickets were being sold to fans via the checkout platform.


#### How did you get started?

We started by reading [Serverless by Obie Fernandez](https://leanpub.com/serverless) and [Serverless Single Page Apps by Ben Rady](https://pragprog.com/book/brapps/serverless-single-page-apps), then we took a look at the services Amazon had, and finally we spiked out a solution. We took a pragmatic approach aimed at solving a specific problem first.

From our performance characteristics, we identified that the biggest performance problem was listing a tour in an event list. All of our artists have a list of events: this list gets the most traffic because people are looking to see when and where an artist will be playing. Our artists insert this information on their website by using a JavaScript widget. Every time an affiliate site loads and shows the widget, our server gets hit by having to give them the content. In addition, the system that was selling orders was the same system that was serving out this event list, so we needed a way to throttle the event list without impacting the other parts of the system. We investigated caching the event list but then we lost important information about view-tracking that we needed to provide analytics to the artists’ management. Obviously caching is an important solution to investigate to improve the event list, but we also needed a way to handle the dynamic click-tracking information we needed.

We decided that this was our biggest problem. We took a week and did a quick spike to see if it was possible to have API Gateway pass click-tracking and view data directly into AWS Kinesis. And it worked really well! We learned that we needed to just cache in CloudFront for 10 seconds, and it made a huge difference. Now the worst case scenario is that we have all of the CloudFront nodes talking to our server at the same time, which is still a subset of the amount of traffic it was before.


![serverless architcture and burst traffic](/assets/images/screenshots/serverless-architecture/serverless-architecture-applauze.jpg)


#### How has event list performance improved after making these changes?

We were maxing out before at about a hundred requests a second but with Cloudfront, we can hit it at about 3,000 requests a second and there's no impact. The main limitation became our ability to test it. When we couldn't make requests any faster from our machine, we stopped testing.


#### How do you test the event list?

We wrote some Ruby tests that are part of our main build. The tests put a message up into our serverless infrastructure. The message goes through the API Gateway, through Kinesis, and then we make sure that once it’s gone through all that we can pull out the message in our Ruby backend.  


#### What services are you using specifically and can you describe the architecture?

We use S3 for some static assets and Cloudfront. We are also using API Gateway and Kinesis. We don’t use Lambda for that at all. Our initial spike used Lambda to consume the data from Kinesis. When we implemented it, we realized it was just as easy in our core back-end server, to have a Ruby process that pulls off Kinesis.


#### What does the process of making changes and modifications to your code look like when your code gets split across multiple different Amazon services?

You need stable APIs and you have to be very cognizant of when you need to change an API. We try to be additive instead of subtractive. We can't have the situation where we try to deploy two environments at exactly the same time. We have to, at the very least, lockstep it in one direction. Usually a change in the core system goes first, and the checkout system needs to follow and support both versions of the API. Occasionally we've had to do significant changes and we've been mostly doing API versioning.


#### What problems do you see with serverless architectures?

One of the biggest problems conceptually with serverless is there are a lot of moving parts. We needed to impose our own structure to have cohesion because it's not provided. It's easy to say, "anything running on this Heroku dyno is one unit". That's simple. Whereas when you have a situation where you have multiple Lambdas, that are interacting with Dynamo, which is interacting with SQS... it’s more complex. To tackle that problem, we've been reading Sam Newman's [microservices book](http://shop.oreilly.com/product/0636920033158.do), which has really been helping us get some structure around that.

We looked into serverless frameworks specifically to see if it would help with this. The biggest reason why we didn't use it is that most of the frameworks that we saw were focused primarily on Lambda behind API Gateway. It's something we're sort of using, but serverless really doesn't deal with Kinesis or the other services, and we needed to be able to manage the entirety of it.

#### What have you learned about the value of serverless architecture?

We realized our microservices could be implemented without the server or at least, the actual part that does the serving, could be much smaller.

Our goal for a lot of this was to outsource the load and the demand, and we've achieved that. These services have allowed us to focus on our domain and on writing the code for our application, and not be limited by worries about the infrastructure and the scaling. Stability is now taken care of by Amazon because that is what they do really well. They know how to do that, and we know our domain.


----
Thanks to [Chris Turner](https://www.linkedin.com/in/bestfriendchris) and [Ian Carvell](https://www.linkedin.com/in/ian-carvell-5a75b81) from Applauze for sharing their serverless journey with us. In the next part of this interview, will we discuss how the Applauze team tackled their next serverless architecture challenge, and how they test and deploy their serverless architecture using tools like Snap CI.
