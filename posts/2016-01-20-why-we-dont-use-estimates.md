---
layout: post
title:  "Why we don't use estimates in our day-to-day"
description: "It’s difficult to find someone who would argue that we should estimate an entire software project without having written a single line of code for it."
date:   2016-01-20
author: Andrei Tognolo
index-image-url: screenshots/no-estimates/no-estimates.jpg
index-image-alt: 'no estimates'
keywords: estimates, project estimates, development time, software development, devops,
categories: estimates software development
---


Eleven months. That was the upfront estimation of the first project that I worked on, nine years ago. Nowadays, it’s difficult to find someone who would argue that we should estimate an entire software project without having written a single line of code for it. On the other hand, it’s pretty common to find people who provide estimates for day-to-day work, even though they don't believe that the estimate provided is accurate.

![no estimates](/assets/images/screenshots/no-estimates/no-estimates.jpg){: .screenshot .big}

## If I could, I would never provide estimates ##

As a professional software developer, if I could, I would never provide estimates. I could tell you many reasons why I don’t want to use estimates anymore. However, in this post, I’m going to highlight only two:

### Estimates tend to hide complexity ###

Software development is usually considered a complex activity, operating within a [Cynefin framework](https://en.wikipedia.org/wiki/Cynefin_Framework). Handling the complexity of software development is part of our job. We should strive to reduce the friction of the software development process, as showed by [Mary Poppendieck](https://twitter.com/mpoppendieck) in [this blog post](http://www.leanessays.com/2015/08/friction.html). Every day, as developers, we fight to keep things as simple as we can. Docker is a great example of how we can reduce complexity by eliminating the need to handle different environments.

However, it’s really important to keep in mind that reducing the complexity is different than hiding the complexity. Although it may not be true in all cases, in my experience, when we provide a number as an estimate, we tend to use this number as a fact and forget about all the complexity behind it. Furthermore, even though we know in advance that this number doesn’t represent reality, it’s pretty difficult to avoid considering it as a fact. That’s one of the reasons why if I could, I’d never provide an estimate.

### Some things I can estimate, others I can't ###
In a [great blog post](http://zuill.us/WoodyZuill/2013/01/22/a-thing-i-can-estimate/) called "A Thing I Can Estimate", [Woody Zuill](https://twitter.com/WoodyZuill) states that "I can estimate how long it will take me to drive to work". He explains in detail why he can estimate that. In my blog post, I’m going to grab only two reasons he gives that enable him to accurately estimate his daily commute:

- It’s essentially the same thing every day
- There are almost no unknowns

In the end of Zuill's post, he asks a simple, but intriguing, question: How similar is this to computer programming? From my experience, I would state that in computer programming these two things are usually false. There ARE unknowns, and it is NOT essentially the same thing every day. Furthermore, taking care of not falling into the [fallacy of the inverse](https://en.wikipedia.org/wiki/Denying_the_antecedent), when you cannot answer yes for these two question it is pretty hard to provide a reasonable estimate.

Let's take my day-to-day work as an example. As a developer for Snap CI, my days are based on developing different kinds of activities, such as:

- Support work, helping people troubleshoot issues on their builds
- DevOps works (security, provisioning, LXC, Docker, cloud infrastructure, Zero Downtime)
- Developing features (parallel, debug, collaborators)
- A lot of other things, like: SEO, blog, reports, website, performance, marketing, data analysis

Now, looking back to Zuill's questions, can we say that it is essentially the same thing every day? No. Each day is a different day, a different challenge. Can we say that there are "almost no unknowns"? No. We’re working with a lot of new technologies, like LXC and Docker. It’s impossible to know everything, especially because these tools are still being developed and are themselves changing.

Based on that, it seems that I can’t provide a good estimate in this scenario. To take Zuill's example a step further, providing reasonable estimates in computer programming would be like trying to plan your daily commute if you had to be at work at different times every day, took different routes, and the roads were constantly under construction and in some cases removed completely. Could you estimate that drive to work?

And here follows a simple lesson that took me a long time to learn: If I don’t know how to estimate something, the best answer I can provide is “I don’t know”. That’s why planning poker doesn’t exist for the Snap CI development team. We don’t believe that we can have predictability from that.

## Is it possible to manage a product development without estimates? ##

If you believe that we cannot have a reasonable predictability from estimates, then it's time for the next natural question: what are the alternatives? In other words, is it possible to manage a product development without estimates?

In order to answer this question, it's really important to understand why people usually ask for estimates. Of course, they don't ask that just because they are anxious about the outcome. They ask that because they have decisions to make. As Martin Fowler said, [estimation is valuable when it helps you make a significant decision](http://martinfowler.com/bliki/PurposeOfEstimation.html). Thus, in order to answer if we can manage development without estimates, we need to check if we can make the decisions required without them.

Instead of start thinking about all the situations and check if each one requires an estimate, I'd like to invite you to think about a decision that doesn't require an estimate. Well, a good example would be a critical security breach in production environment. We have a decision to make: should I fix it right now or should I continue working on what I was doing? It sounds like a silly question, doesn't it? Of course you should fix the bug in production first, and we don't need an estimate to make this decision because of two things:

- We don't have a choice. We have to do it
- It is the most urgent thing to do

This example shows that the need for an estimate diminishes when we are doing the most urgent thing. Now, trying to expand this example, what if we decide to treat our features in the same way, prioritizing them and working on the most urgent thing? That is what we do on Snap.

However, even in this way, people who have decisions to make can face difficulty in planning. Although the software industry is still trying to figure out alternatives for the use of estimates, I would say that there is a consensus that we should provide high visibility of the work-in-progress. I am sure that you will not be surprised if I tell you that a great technique to bring visibility is Continuous Delivery.

That’s our secret sauce. We don’t use estimates to guide our day-to-day work because we are always trying to prioritize the most important things and constantly delivering working software.

## To estimate or not to estimate, that is the question ##

To estimate or not to estimate? That’s the question and there's no easy answer. In this post, we've at least shown that it’s *possible* to manage a product without using estimates. Deciding if and when we should estimates, or not, is a tough task and we are constantly rethinking our process, which we will detail in another blog post. That said, we would love to hear from you. How do you handle estimates on your team?
