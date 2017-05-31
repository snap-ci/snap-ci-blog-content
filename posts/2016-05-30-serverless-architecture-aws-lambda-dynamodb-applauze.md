---
layout: interview
description: Serverless architecture is a perfect solution for online ticketing sales provider, Applauze. See how it works with their large bursts of traffic.
title:  "Building Serverless Architecture with AWS Lambda, Snap CI and DynamoDB"
date:   2016-05-30
author: Badri Janakiraman
index-image-url: applauze2/applauze.png
index-image-alt: 'serverless'
keywords: serverless architecture, aws, deploy software, continuous delivery, software delivery, lambda, PaaS
categories: SERVERLESS-ARCHITECTURE CUSTOMER-STORIES
---


Following our [previous conversation](/blog/2016/05/03/serverless-architecture-burst-traffic-applauze/) with [Applauze](https://www.applauze.com/), where we talked to them about their need to handle spiky, demand-driven traffic, we discuss the details of their application architecture, their previous generation design, how they started down the road to [serverless architectures](/categories/serverless-architecture/), and lessons they learnt along the way. We start by having Chris Turner and Ian Carvell restate and frame their problem, followed by their solution.

As a ticketing system that deals with presales, one of the core problems we have to deal with is the extremely spiky and bursty nature of traffic. When a major artist’s new tour goes on sale, we have a spike in traffic at the exact second that the sale goes live. Also, bands and record companies love to be able to say “we sold out in minutes”. So these are real business needs for us as a ticketing company.

#### So the size of the queue then is a problem?

Exactly. If we were a real venue selling box office tickets, the fact that the queue to get these tickets goes round the block does not impact the ability of the box office to sell tickets. The box office person does not give up and go home at the sight of an overwhelmingly large crowd. They just sell the tickets as fast as they can.

#### Prior to exploring the options available on AWS, what sort of limits were you running into?

Scaling up and down servers was always tricky for extreme bursts. Autoscaling doesn’t quite work fast enough to be able to handle the kind of spikes we see.

Before a pre-sale starts, we serve a static “We are not on sale yet” page from a CDN. The crowd of people hitting refresh on their browsers grows but the CDN and a static page take that load. But the second the covers are lifted, the entire load shifts from the CDN to our servers.

Since we have no way of knowing what the exact load will be, we need to overprovision for the worst case scenario. Overprovisioning for a smaller artist or venue would be relatively easy. However, when you start looking at the really big name artists and large volume, premiere venues, over-provisioning starts to get fairly expensive. In addition, we started to run into limits with the number of database connections we could make. This made our pre-sale queue volume directly hit our application.

![convetional-queue-implementation](/assets/images/applauze2/serverful.png){: .screenshot .big}

Each new user in the queue would post their arrival into the queue into our Rails application. This would then register this user into a ticketing service that then used Redis to persist this user queue. The user's browser would then be given a resource to poll on, which could be used to show the users a couple of useful things such as "How many people are ahead of me in the line?" and "Is it my turn yet?".

In the meantime, the ticketing service had a background "Window Assignment Processor" that would check to see how many open "ticket issuing windows" were currently occupied and slowly move assign the open windows to the records. These windows acted as [Competing Consumers](http://www.enterpriseintegrationpatterns.com/patterns/messaging/CompetingConsumers.html) on the Redis based queue. Once a window was assigned to a record, it would be updated with a purchase flow URL, which the browser could poll on and redirect to as soon as it was present.

The key thing here was the whole weight of the queue descended on our Redis instance and Java server. In addition, the burden of handling all those poll requests for processing too fell on our servers. This burden would have overwhelmed us when the queues got big.

#### Can you describe what your application looks like now?

![serverless-queue-implementation](/assets/images/applauze2/applauze.png){: .screenshot .big}

Sure. The overall flow looks a bit like the diagram above. It is a bit simpler to follow compared to the sequence diagram. Especially if we break it down into parts. :)

The first step was to avoid storing the list of all people in the queue in our application database.

![serverless-inserts](/assets/images/applauze2/applauze-inserts.png){: .screenshot .big .float-right}

We realized that buffering our traffic was something that could be delegated to Amazon since they had already solved this problem for their own services. Dynamo could scale to store data. Lambda could scale to process data in a Dynamo table etc.

So instead of posting each new user into our own Redis instance, we used a Javascript client library to post the user record into a DynamoDB table. We use the user's Cognito identifier as the key through the rest of this flow.

![serverless-processing](/assets/images/applauze2/applauze-process.png){: .screenshot .big .float-left}

Once in this DynamoDB table, we have a lambda that would process all new records in this table and move the records over into a SQS queue. From this queue, the same "Window Assignment Processor" could take over as it did previously - but it would update the record in DynamoDB instead of Redis. Also, the browsers would poll the records in DynamoDB rather than hitting our server.

This then meant that our Ticketing Service was free to scale to meet the demands of the only thing that should be its concern - which is the issuing of tickets from the limited supply that we have. The scaling requirements for this service now are directly mapped to the business concerns which we talked about earlier - i.e. being able to issue the whole pool of tickets as quickly as possible, independent of the size of the queue that hits the pre-sale.

The actual scaling requirements are met by Amazon. DynamoDB can do all the inserts and querying that it needs to do at pretty much any scale that we are likely to encounter. Similarly, Lambda and SQS can scale on demand, and in addition, are both pay-as-you-go services so that you only pay for the number of Lambda invocations or the number of queuing requests you make.

So with this mechanism in place, we have managed to solve both our scaling issues and cost concerns by offloading them to services that gave us the right characteristics we needed.

#### That's neat! So it sounds like you have managed to  offload the scaling to services that have chosen to solve that problem and focus on ticketing, which is your core business.

Pretty much!

#### Can you tell me a little bit about how you build and deploy all these moving parts?

![serverless-infra](/assets/images/applauze2/applauze-deployment-infra.png){: .screenshot .big}

There were a couple of key things we wanted to get done with the automation.

We wanted all environments to be identical and be able to bring up a dev environment just for one developer if we wanted. At the same time, we wanted the environments to be isolated as much as possible and not share key components so that if we trashed one during development, other environments could keep chugging along. Lastly, we wanted the automation to be all done via CloudFormation but using a restricted IAM user that would only have the rights to do the deployment.

However, we had to deal with some issues during this process.

###### CloudFormation
We wanted to use CloudFormation - but it is not complete. Some things such as support for Lambda are partial. You can make the lambda but not deploy code to it. Other things such as Cognito are not automatable through CloudFormation at all.

###### Essential shared components
Due to the nature of Lambda, we ended up having a single lambda backed by a single S3 bucket. The good thing, however, is that a single lambda can be aliased to have different names and we were able to combine that with the fact that Lambda versions the code it runs to effectively deploy different versions of the code into the different environments.

###### The deployment process
We use Snap to build, test and deploy all our code.

First, we ensure that the stack is created. We then use Snap to deploy the code for the lambda to the S3 bucket that is shared. This results in a new version of the lambda getting created. We also use a small database to keep track of the correspondence between the pipeline numbers and git commits in Snap to the Lambda version number. When we deploy to any environment, the stage in Snap looks up the correct Lambda version for that build, switches the alias to point to that version and completes the deployment.

One of the interesting things though is that since it is the same lambda resource that runs in both staging and production, that could cause problems. The lambda is responsible for processing the user in the queue and pushing that down to SQS queue. But the queue that it needs to push to is different in the different environments. So we needed some way to inject configuration information into our lambdas.

#### I have not heard of teams supplying configuration information to their lambdas. How did you accomplish that?

Well - that is where the little DynamoDB database called the config database that is connected to the lambda comes from. That database holds environment-specific information. Sort of seed data but more infrastructural. The initial CloudFormation script would create this database. The build in Snap would then query the CloudFormation stack output parameters to find the correct DynamoDB table to go to for the rest of the dynamic configuration information. This is pretty much what the folks at AWS recommend to do to pass configuration information to lambdas anyway.

#### How do you make sure that the whole thing hangs together and works as expected?

We have different kinds of tests to make sure this all works.

- We have some tests that use Selenium to exercise the flow as an integration test starting from the browser.
- We also have some tests that pump messages through the system without the browser to simulate the flow. In these tests, we can bypass the any filters and validations for the intermediate steps that prevent data from entering half-way through the processing so that our testing data can be used to run these tests.
- Lastly, we also have some tests running in production. The ticket processing application pushes data into the top of the funnel and waits for that data to make it's way back into it. If that doesn't happen in a fixed amount of time, we notify the development team that there is a problem. This idea has been around a while and is referred in it's various flavours as [Semantic Monitoring](https://www.thoughtworks.com/radar/techniques/semantic-monitoring) or the [Test Message pattern](http://www.enterpriseintegrationpatterns.com/patterns/messaging/TestMessage.html).

We potentially need to also add some scenario-specific tests but we have not quite gotten to that point yet.

#### Did you run into any other subtle issues that you think could be use to our readers?

In addition to figuring our things like aliases and configuring lambdas that we mentioned earlier, we also ran into a couple of other issues.

###### Lambda clogging

We noticed that occasionally the lambdas would appear to stop working. Upon investigation we found that it was due to the nature of the DynamoDB-lambda interaction.

If the Lambda execution failed for a DynamoDB entry, DynamoDB (or more accurately, the transport that moved the message between the systems, which we suspect is Kinesis) would redeliver the message. That could cause one bad message to clog the lambda. The nasty thing is that even timeouts can cause this issue. So we have had to catch all errors, log them, and mark the records that come from DynamoDB as having been successfully processed.

This is quite common in web-hook style processes where returning anything 2xx from the processor is equivalent to signalling the origination system to retry - but it took us a while to understand that it could happen in this context too.

###### Changing the shared lambda

Another thing that we have as an open question is how to deal with making infrastructural changes to the shared components - such as the lambda because that is shared by the dev and production environments. We know we will need to come up with a migration path for this - but we don't have one yet.

###### It's complicated!

In case it is not obvious, there are a lot of moving parts to a system like this that make it much harder to understand and grok compared to a our original application. The common thing that people think when faced with this is that they will hand off individual parts of the whole to individual pairs. However, we have found that that doesn't work well for us. We are a small team and the way we ensure that this stays simple and easy to understand for everyone is if we all work on the whole thing. So it became important for us to not have the developer with most context nail down every part of every component even though it might have been initially efficient to do so. Extensive pairing and rotation was how we made sure that everyone on the team understood all parts of the application.

#### Wow, this has been a lot. Thanks so much for walking through all of this with us. It's been really helpful to understand exactly how all the bits and pieces come together.

Our pleasure.  

#### Special thanks to Mike Aleksiuk, Chris Turner, Chris Stevenson and Ian Carvell from Applauze for speaking with Snap CI.

 
Snap CI © 2017, ThoughtWorks
