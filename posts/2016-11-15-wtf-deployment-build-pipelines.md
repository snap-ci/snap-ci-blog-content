---
layout: post
title:  "WTF are Deployment Pipelines?"
description: "A beginner's guide to understanding continuous delivery"
date:   2016-11-15
author: Erin Snyder
index-image-url: screenshots/wtf/wtf-deployment-build-pipelines.jpg
index-image-alt: 'wtf is a deployment pipeline'
keywords: snap-ci, build pipeline, deployment pipeline, cd pipeline, deploy software
categories: WTF DEPLOYMENT
---


Let’s do a mindfulness exercise. Apparently they’re all the rage now! Close your eyes and picture your code being built, tested and deployed. What do you see?

Maybe you see a bunch of 1’s and 0’s like in The Matrix? Maybe a bunch of connected lines like a transit map? Or maybe a plate of spaghetti if code integration and deployment tend to be really messy at your workplace?

I picture a pipeline, similar to the green pipes in Super Mario Bros. My latest commit hops into a pipe, runs some tests (and collects some coins and avoids some piranha plants), turns green, and pops out the other end into another part of the world (the world of production).

## Why a pipeline?

The [pipeline concept](https://www.amazon.com/dp/0321601912) makes it easier to split up your work into stages and quickly spot breaking changes. A simple pipeline might have a build stage, followed by a test stage, followed by a deploy stage. The build stage creates a binary and/or downloads artifacts that can be used in later stages. The test stage can run all of your automated tests, and then you can choose to automatically or manually deploy to the environment of your choice.

You could get a bit more complex by breaking up your tests into multiple stages to get faster feedback. Are my smoke tests passing? What about my rspec or jasmine tests? It’s important to be able to tell at a glance where the pipeline has stopped so that you can quickly fix the problem and get moving again.

#### Move faster

Speaking of speed, that’s another good reason for choosing a pipeline over other concepts such as something that looks like a bar graph or a bunch of checkboxes. Try dividing up your test suite and run tests in parallel for even faster feedback. The faster your deployment pipeline, the easier it is for your team to deploy a bug fix or push out a new release.

Pipelines also reduce the risk of your deployment breaking something, because you can test each commit in increasingly more production-like environments. By the time the commit makes it to the end of the pipeline, you can feel confident about deploying to prod. No more waking up at 3am in a cold sweat because someone’s latest commit “worked on their local machine.”

Have we won you over yet? Are you ready to set up your first deployment pipeline? [*cue Mario music*](https://youtu.be/N9Lgapjt7hE)
