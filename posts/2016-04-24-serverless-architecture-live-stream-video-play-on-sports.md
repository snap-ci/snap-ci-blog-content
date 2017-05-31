---
layout: interview
title:  "Why Serverless Architecture Makes Sense for Live-Streaming Video"
description: "Although it's still early days, serverless architecture can essentially evaporate architecture, freeing up administrators to deal with the inherent complexity of their domain rather than its framework."
date:   2016-04-24
author: Badrinath Janakiraman
index-image-url: screenshots/serverless-architecture/serverless-architecture-video.jpg
index-image-alt: 'serverless architecture for live-streaming video'
keywords: serverless architecture, aws, lambda, play on sports, live video streaming, continuous delivery, cloudfront, dynamo
categories: SERVERLESS-ARCHITECTURE CUSTOMER-STORIES
---


[PlayOn! Sports](http://www.playonsports.com/), headquartered in Atlanta, is a multi-faceted media company centered around live-streaming high school sports. With a dedicated and expanding user base, they're looking into new ways to increase service while decreasing costs. We sat down with PlayOn's Jay Sandhaus a few weeks ago to talk about how the company's using serverless architecture to address its unique business needs.


#### What got you interested in serverless architectures?

To take a couple of steps back and provide some context, we are a small startup. We have a very small technology budget comparatively speaking, and we want to stay focused on building products for consumers and for our content creators. We don't have a lot of resources for an ops team. So the more efficient our operations are in terms of resources needed, the more I am able to put into development.

We have been talking about serverless architectures for a *long* time. When Amazon released Lambda--which was really the first nominally commercial grade environment in which you could actually execute something like that--we got really excited at its potential.

So one way to reduce cost is to constantly to take away infrastructure whenever we can. So a serverless architecture is the apex of that concept. That’s why we are using a platform like Amazon in the first place. Even though the per-execution cost of a function in Lambda is an order or magnitude more expensive than executing the function on your own server, when you factor in all the time that the servers are doing nothing (which is the overwhelming majority of the time) it becomes really very efficient from a cost perspective. So that was certainly something that got us very interested.

## "One way to reduce cost is to constantly take away infrastructure whenever we can" -@sandhaus

#### Exactly how do you use this design in your application?

So for us, we have a very special use case: we're transcoding a lot of live video. PlayOn! Sports streams live video of high school sports. We have an unusual model in that for most people streaming live video, the typical media company, has a small number of live streams and a large audience for each one of those streams.

We're the opposite. We try to help high schools all across the country stream events like Friday night football live. We might have between 400 to 500 inbound live streams at the same time, but the audience for each of those will be between hundred and few thousand people. If you're a big media company, you can go and apply $250,000 dollars worth of hardware so that it streams properly and is transcoded into a million different flavors, we obviously can't do that. The economics just don't work. So a serverless architecture for transcoding is the first opportunity we’ve had to tackle some really thorny video problems that have a high degree of computational and resource fan-out, without having to invest an enormous amount into infrastructure. This is really crucial to us: it's solving a business problem for us that we could not solve before.


#### Did you consider using Amazon’s transcoding service?

No, not really. From our vantage point it’s expensive. It was priced with larger media companies in mind.


#### So where are you at with it? Is it a POC? Is it deployed?

It is deployed in the sense that we are testing it on the fields and schools. But it is beyond a proof of concept. I guess you would probably describe it as "alpha testing".


#### This is interesting. Seems like a fairly different different use case to what one might have suspected.

Yeah, absolutely. It's not about static content, it's about live video, which is kind of a funny use case for it. That's also why we’re sort of backed into using something like this, because there aren't a lot of affordable options in the marketplace.


#### Is it only Lambda that you are using? What other services are you weaving together?

We're also using Dynamo, SNS and Cloudfront from where we serve the content, and S3 for storage. Lambda is a very good fit for what, in our case, is a *mildly* asynchronous workflow. We can suffer a few seconds of latency, and Lambda is good enough for that.


![serverless architecture live-streaming video](/assets/images/screenshots/serverless-architecture/serverless-architecture-video.jpg){: .screenshot .big}


#### What do you do about testing? Some AWS services such as Dynamo, you can run locally, but a whole load of other Amazon services, such as SNS or Cloudfront, there isn’t exactly a local equivalent that you can run on the average developer workstation. So what is the testing story like? How do you make sure that the application works?

Well, one advantage we have here is that the actual workflow inside of Lambda of what ends up being executed is somewhat discreet and fairly testable. For example, there's a piece of video, which is picked up by five or six different jobs, and then it's kind of stitched together back at the end.

And there's a whole separate process for creating a playlist based on what came in and what came out.

The hardest thing to test there are the timing issues: Is something available when we thought it was going to be available? Right now we are building tests that trigger something locally and see what happens in the cloud. However, we don’t have the sort of cyclical effects that people might have, you know, where there's data coming in, that triggers one job, that then triggers another job, and so on.


#### That’s useful to know. Because we are talking to some people who are trying to weave together four or five different services.

Yeah, I think might be nearly impossible to debug and test something like that. But right now we don’t have that problem.


#### Any parting thoughts?

I guess as a factor of the use case - I’m glad to say that not a lot of time has gone into fine-tuning  Lambda. Most of our time has gone into manipulating the video. It’s actually a fairly hard problem to parallelize video transcoding.

I was very concerned initially about a small percent of the jobs vanishing, or taking an arbitrarily long period of time to finish. At least so far, that hasn’t happened. Things are just working the way we expected them to. So far it's a gold star for Lambda and serverless architecture.

## "So far it's a gold star for Lambda and serverless architecture." -@sandhaus

In terms of problems, this will be an easier question to answer two to three months from now when we leaving beta and have widespread usage. Because then, with people using it all the time, I'll be trying to figure out how to transition between version 1.0 and version 1.1. Validating which version of each function we are executing--that could become challenging.

Some mysterious problem may come up with the architecture, it may not. I feel like with something this new and this sort of incomplete, we are going to eventually stumble into a dark corner where there's no obvious answer. But so far, for our near real-time application, serverless is very good.

#### Conclusion
Although it's still early days, serverless architecture can essentially evaporate architecture, freeing up administrators to deal with the inherent complexity of their domain rather than its framework. Not every business--in fact, I would argue, very few businesses at this point--are going entirely serverless across every sub-section of their product. The most common set-up right now, that we're seeing, is people putting small, critical parts of their application on Amazon, and then leave the core inside their existing infrastructure. The benefit of this is for that small niche of the application, they can get Amazon-level scalability, reliability, stability, etc, in a way that they could not get even if they were to attempt to scale their own little microservice components. So far this does not seem to be a one-size-fits-all solution, but in many cases, it not only fits but makes the application more virile. To learn more about [real-life examples like PlayOn! Sports](https://www.youtube.com/watch?v=U8ODkSCJpJU) of where and how serverless architecture works, we hope you'll continue to read the other blog posts in this series.

*This is Part Two in a series on Serverless Architecture. To read more, [click here](https://blog.snap-ci.com/categories/serverless-architecture/).*

 
Snap CI © 2017, ThoughtWorks
