---
layout: post
title:  "How Canary Release Helped us Deliver a Rails Upgrade"
description: "A new approach at releasing deployments that would reduce risk and regain confidence"
date:   2015-08-18
author: Marcos Brizeno
index-image-url: screenshots/canary-release-ruby-upgrade/Canary_release_1.png
index-image-alt: 'Bringing Docker Support on Snap CI'
keywords: canary release, continuous deployment, rails, capistrano, ruby, big bang
categories: article
---

### About the Project:

Imagine a project that's eight years old. No matter what language or framework used, the code is very likely to be hard to maintain. The things that we take for granted now didn't even exist eight years ago. We had a challenging experience on these lines, while delivering a Rails 2 to Rails 3.0 upgrade.

It took us a year and half of dedicated work to upgrade the app, while still having to add features to it. The team finished the work to upgrade versions of the language, framework, gems and other external libraries.

The first attempt to deploy the upgraded version was made using a Big Bang release approach. The major benefits of this was that the process was well tested, since the team was doing daily releases to pre-production environments, and it didn't require many changes to the process that was already in place. On the other hand, the biggest risk in this approach was that it was all or nothing. If some critical unexpected issue came up, we would have to rollback the upgrade.

After the first release using the upgraded app, we faced some issues with not well tested integrations points and other external dependencies. In the end, the client asked for a rollback even though we weren't sure about the root causes of the issues.

After the incident, the team decided to work on a new approach to the release that would reduce risk, simplify the rollback strategy and regain the confidence of the client. That's when we decided to try out a Canary Release approach

## Why Canary Release?

When we started exploring approaches other than the Big Bang release, we found Canary Release to be ideal. To quote [Danilo Sato](http://martinfowler.com/bliki/CanaryRelease.html):

"Canary release is a technique to reduce the risk of introducing a new software version in production by slowly rolling out the change to a small subset of users before rolling it out to the entire infrastructure and making it available to everybody"

With that in mind, we started to work on our deployment process and infrastructure to support the two versions of the app (Rails 2 and Rails 3) in the same environment. This posed an interesting challenge of making different versions, and even different gems, communicate well with each other.

One thing that’s worth mentioning is our deployment architecture. We had a lot of servers/boxes with different roles, but the most important in this case were web boxes, web service boxes and worker boxes. The others didn't have app code running, so it wasn't really hard to upgrade them, except for the memcache boxes. You'll understand why we are talking about this one specifically later on.

The web boxes, as the name suggests, are responsible of handling the web traffic and they are where we focus our efforts to split the traffic.

![what are web boxes]({% asset_path screenshots/canary-release-ruby-upgrade/Canary_release_1.png %}){: .screenshot .big}

The web service boxes respond to external calls from third party applications. For these boxes, we decided to have half of them running Rails 2 and the other ones running Rails 3. The policy to split the traffic between them was based on serving the request to the lowest loaded server.

![split the rails boxes]({% asset_path screenshots/canary-release-ruby-upgrade/Canary_release_2.png %}){: .screenshot .big}

Worker boxes are responsible of doing all the asynchronous work, such as sending emails, purge old data, so on. For some workers, it is mandatory that only one instance of the worker is running. We call this type of worker as singleton workers. We had two worker boxes that only ran singleton workers, so we decided to keep one box running Rails 2 and the other running Rails 3. For the other workers that didn't have the requirement of running only one instance, we decided to migrate only one box to Rails 3.

![worker boxes]({% asset_path screenshots/canary-release-ruby-upgrade/Canary_release_3.png %}){: .screenshot .big}

First we had to come up with a strategy to roll out the upgrade to a particular set of boxes and split the web traffic. In order to split the web traffic, we set up two different user pools in our load balancer and split the traffic in a way that if User A accessed the Rails 2 version, he would stay in that pool. If User B accessed the Rails 3 version, she would always access Rails 3 boxes. This helped minimize the risk of clash incompatibility between the versions. We could have done this balance via software through the application, but it was simpler to use our infrastructure to do the job.

To solve the problem of rolling out the upgrade to a particular set of boxes, we had to change our deployment script slightly. We used [Capistrano](http://capistranorb.com/) to execute scripts/commands in our remote servers and it allowed us to specify a set of servers that we wanted to touch.

We had to fix the capistrano tasks where we didn’t use server filtering. Some steps that we did during a full deployment were no longer necessary and we changed the script to optionally skip them.

There was some incompatibilities between the way we serialized objects to the memcache. To avoid this incompatibility issue, we decided to create two more memcache boxes and made each version using different ones.

Our main concern was, by far, the unexpected behaviour of having two different versions of the rails framework running together at the same time and interacting with each other, creating and consuming data interchangeably. We tried to mitigate that by using a pre prod environment as similar as possible to production, but we knew, that we couldn't reproduce the combination of all possible scenarios in advance.

## How it Went

Our plan to do the complete upgrade was divided into three deploys. The first step was to migrate a few boxes, put them in the Rails 3 pool and direct 10% of the total traffic to that pool. After that, we spent a week monitoring the production environment.

During that week, we saw some minor issues happening. Since just a few boxes were being used, we managed to fix the problems without any downtime in the application which helped reduce the pressure on the team considerably. It also allowed us time for a proper investigation.

As a good example, we had web boxes running Rails 2 and 3, where both were producing data for emails jobs. It turns out the worker boxes running Rails 3 were capable of consuming data produced in both versions, but the worker boxes running Rails 2 were not. As our goal was to go towards Rails 3, we decided to turn off the workers on Rails 2 boxes and lived only with the workers running Rails 3 version.

The second step was to migrate half of all boxes and split the traffic 50/50 between Rails 2 and Rails 3. We spent a week monitoring the production environment, but this time no issues came up. Our major concern was regarding the performance.

We knew that the performance was going to be worse compared to what we had before, by comparison in pre production environments and by reading other’s experiences. We just weren’t sure how much worse it would be. It turned out that the performance was not that bad and we were able to manage the load.

In the third step we finished the migration, added all boxes to the Rails 3 pool and directed 100% of the traffic to it. No major issues were found (after all, the upgraded version was live for two weeks) and we knew that we wouldn’t face any load issues.

## The Road Ahead

The project is running technologies on versions that are still being supported and the team has learned a lot. The client also saw the value of investing on technology upgrades. Revisiting the whole application design and architecture decreased the error rate in the application, improved the user experience and made the application more reliable.

The team will continue to investigate the upgrade to the newest versions of Ruby and Rails and with the lessons learned after our efforts, we can plan more carefully on how to proceed.

Our experience using Canary Releases proved to be successful and clients can now consider this as an option when they have a risky releases planned.

This post was originally published on [ThoughtWorks Insights](http://www.thoughtworks.com/insights/blog/how-deliver-rails-upgrade-using-canary-release) Continuous Delivery channel.
