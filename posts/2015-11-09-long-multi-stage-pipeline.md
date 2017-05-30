---
layout: post
title:  "Who's got long pipelines?"
description: "The need of having multiple stages in your delivery pipeline caused some UI problems, solved by Snap CI's new pipeline interface"
date:   2015-11-09
author: Vishal Naik
index-image-url: screenshots/long-pipeline/snap-ci-long-pipeline.png
index-image-alt: 'long delivery pipeline'
keywords: deployment pipelines, delivery pipelines, multi-stage pipelines, continuous delivery, continuous deployment
categories: feature
---

Ever since Snap CI was created in 2013, it has always had deployment pipelines at the core.
From the beginning, one of the challenges has been to inform users on the concept of the pipeline itself, which is [central to Continuous Delivery](http://martinfowler.com/bliki/DeploymentPipeline.html) and continues to be a [unique aspect of Snap](https://blog.snap-ci.com/blog/2014/07/22/why-snapci-and-travisci-are-not-the-same-thing/) in the hosted CI/CD space.

Custom [multi stage pipelines](https://blog.snap-ci.com/blog/2015/07/23/multi-stage-builds/) in Snap afford a lot of flexibility to model your application delivery workflow and can support best practices like [Trunk Based Development](https://blog.snap-ci.com/blog/2015/10/06/enabling-trunk-based-development/) effectively.

When Snap first came out, users were still warming up to the idea of custom stages and had two or three stages at most.
However, we are now seeing an increase in the number of custom stages set up per build pipeline, and we couldn't be more thrilled! Granular stages provide much better visibility into the deployment process, apart from many [other advantages](https://blog.snap-ci.com/blog/2015/07/23/multi-stage-builds/).

But one of the impacts of users setting up more stages per pipeline was that some users were getting the dreaded horizontal scrollbar when the number of stages exceeded a threshold dependent on screen width.

Last week, we rolled out a more responsive interface for our pipeline history page to get rid of the horizontal scrollbar.

![snap ci delivery pipeline](/assets/images/screenshots/long-pipeline/snap-ci-long-pipeline.png){: .screenshot .big}

This is, of course, a tiny fix. We wanted to give better experiences to affected users even as we wrap up efforts on getting [Docker](https://blog.snap-ci.com/categories/docker/) out in Beta and double our build worker capacity to keep up with recent user growth. Soon after all that is done, we will bring on more usability improvements including improvements on the current layout!

What do you think of our project pipeline UI? Feel free to send any suggestions or feature requests to [support@snap-ci.com](mailto:support@snap-ci.com).
