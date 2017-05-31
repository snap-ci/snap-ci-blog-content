---
layout: post
title:  "An Introduction to Serverless Architecture"
description: "Serverless architecture is another step on the virtualization journey"
date:   2016-04-19
author: Badrinath Janakiraman
index-image-url: screenshots/serverless-architecture/serverless-architecture.png
index-image-alt: 'introduction to serverless architecture'
keywords: serverless architecture, aws, deploy software, continuous delivery, software delivery, lambda, PaaS
categories: SERVERLESS-ARCHITECTURE CUSTOMER-STORIES
---


## How and why real teams are going serverless

One of the benefits of working on Snap CI is that it's a product that attracts some very interesting teams. They are clued into Continuous Delivery, tooling, automation and—being teams that use GitHub and deploy their code to cloud-based services such as Heroku—they also tend to be highly comfortable with the idea of writing code and running a service without owning servers. So I suppose it was inevitable that we would find among them some of the first people to deeply investigate serverless architecture and do really cutting edge work in this space. In a series of blog posts, we will explore our users' journeys to serverless architecture and annotate the problems and solutions they found along the way.

## But wait. What is this "serverless architecture"?

Before we move into the series, it would be wise to take a moment to explore what exactly serverless architecture means. We could choose to see it as yet another step in the virtualization journey. The way I see it (and this is only slightly contrived ;)) is that this journey begins with teams and organizations deploying long-lived applications running on physical servers owned in data centers. Virtualized infrastructure services make it possible to own shorter term reservations for virtual servers in the cloud. One step further, and with PaaS providers you can own shorter lived applications on a virtualized execution platform. The ultimate reduction in this ownership model is when you own nothing except for an ephemeral unit of computation that blips into existence to run a single request to an API endpoint and then dies away. An example of such a component is Amazon’s Lambda.

![history serverless architecture](/assets/images/screenshots/serverless-architecture/history-of-serverless-architecture.png){: .screenshot .big}

Using virtualized infrastructure services and PaaS providers makes us rethink how we design our applications. Similarly, this ephemeral per-request model where you weave your application out of a myriad of cloud-based services requires thinking differently about the design, testing and deployment of applications. The tradeoffs involved are different. This approach to application design and the tradeoffs involved, then, constitute serverless architecture.

As with any new approach to application design, serverless architecture isn't a free lunch. That said, its promise is immense. When applications are woven together from cloud-based services, a whole slew of cross-functional considerations such as scalability, monitoring, caching, security patching, etc, cease to be the problems of the individual teams as they now fall under the purview of the individual service providers. Operating costs are anecdotally lower. However, a whole other slew of other concerns such as testing, tracing execution flow, data consistency and deployment options appear to be more difficult.

How, then, do teams handle these concerns?

![introduction to serverless architecture](/assets/images/screenshots/serverless-architecture/serverless-architecture.png){: .screenshot .big}

## How our users approach serverless architecture

In serverless architecture, there are echoes of some other recent ideas. These include API-first thinking, rich (and in some cases, single page) Javascript applications, designs based on backend-as-a-service, microservices, pushing the limits of static site generators, and more.

Of course, each team will approach it according to their own particular needs and resources. Two companies we work with in particular talked with us about their need for serverless architecture. [Applauze](https://www.applauze.com/), a mobile ticketing app, had explored microservices and mobile-backends-as-a-service. At the same time, [PlayOn Sports](http://www.playonsports.com/), a sports media company, had been experimenting with the limits of static site generation and single page Javascript applications. So it was interesting for us to see both these teams arrive at serverless architecture from different paths, seemingly by convergent evolution.

We hope that in following companion pieces—in which we talk to these teams about their drivers for trying out this approach—the benefits, pitfalls and puzzles of serverless architecture become more apparent. It was illuminating for us to hear our users talk about their specific systems and we hope you find them similarly useful.

## A small note of caution

While a lot of the teams we spoke to use AWS extensively, there is nothing about serverless architecture that implies an exclusive use of AWS services such as the API Gateway or Lambda. The ephemeral nature of compute resources, the lack of long-lived processes and machines, and the style of weaving together applications from various backend services are more generally applicable and not restricted to just one vendor or provider.

**Related links**

* [https://aws.amazon.com/blogs/compute/microservices-without-the-servers/](https://aws.amazon.com/blogs/compute/microservices-without-the-servers/)

* [http://techcrunch.com/2015/11/24/aws-lamda-makes-serverless-applications-a-reality/](http://techcrunch.com/2015/11/24/aws-lamda-makes-serverless-applications-a-reality/)

* [https://leanpub.com/serverless](https://leanpub.com/serverless)

* [https://serverlesscode.com/post/serverless-conf-is-here/](https://serverlesscode.com/post/serverless-conf-is-here/)

* [https://pragprog.com/book/brapps/serverless-single-page-apps](https://pragprog.com/book/brapps/serverless-single-page-apps)

 
Snap CI © 2017, ThoughtWorks
