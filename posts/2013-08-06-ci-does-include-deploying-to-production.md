---
layout: post
title:  "CI does include deploying  to production"
description: At Snap, we believe that any Continuous Integration tool, hosted or not, should treat deployments as a first class member of the Continuous Integration process.
date:   2013-08-06
author: Badrinath Janakiraman
keywords: snap ci, continuous delivery, continuous integration, developer tools, github, continuous deployment
categories: deployments
---

[Continuous Integration]( http://www.thoughtworks.com/continuous-integration) has been around in some shape or form for over 10 years now. Back in 2001, with CruiseControl as the only tooling and Selenium still a few years away, it didn't usually involve much more than compiling the application, running JUnit tests and finally building a jar if the tests finished successfully.

By 2004, I encountered my very first hand-crafted CI pipeline, created by hooking up two independent CruiseControl instances building the same repository together. One ran the faster unit tests and the other ran the much slower functional tests. The idea was just as cool as the implementation was fragile. In 2005, at a different client, my team came up with the concept of the "Big Purple Button" which would unsurprisingly be a big, purple button on the cruise dashboard which the business users of the application could click on to self-service a deployment to an internal staging environment. This was definitely not a new idea- and by the time of the [2006 rewrite of the ContinuousIntegration article](http://martinfowler.com/articles/continuousIntegration.html) by [Martin Fowler](http://martinfowler.com/), it was commonplace for ContinuousIntegration to include not just unit tests, but also functional tests of some sort and additionally to even [deploy to production](http://martinfowler.com/articles/continuousIntegration.html#AutomateDeployment "continuous-integration martin-fowler automate-deployments").

Since that time, 7 years ago, automating the deployment of your application all the way through to production has been a standard part of what might be considered Continuous Integration. We take that pretty seriously. We believe that any Continuous Integration tool, hosted or not, should treat deployments as a first class member of the Continuous Integration process. With Snap, this takes the form of a simple deployment pipeline; first described in the book [Continuous Delivery](http://www.amazon.com/gp/product/0321601912?ie=UTF8&amp;tag=martinfowlerc-20&amp;linkCode=as2&amp;camp=1789&amp;creative=9325&amp;creativeASIN=0321601912), and summarized beautifully [here](http://martinfowler.com/bliki/DeploymentPipeline.html).

The key benefits of automating deployments and having them modelled as stages in a deployment pipeline are:
* it creates cross-team visibility of when an environment is ready for an upgrade
* it makes the history of deployments to an environment be easily visible, shared team information
* smoke tests following the deployment can verify the health of the deployment and make evident its readiness for promotion

The following image shows the recent deployment history for the Snap [documentation site](http://docs.snap-ci.com). The first stage generates the website using Jekyll and the subsequent stages which deploy to staging and production sync it to an S3 bucket. And yes, this does mean that deployments in Snap are not limited to just Heroku! That's a different blog post for another day, though. :)

<img src="/assets/images/screenshots/deployment-history@2x.png" class="screenshot"/>
