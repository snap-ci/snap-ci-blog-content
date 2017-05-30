---
layout: post
title:  "3 Key Deployment Pipeline Patterns"
description: "Teams have been automating the build, test and deploy processes of their software for many years, but usually in a very specific \"one off\" manner. This piece walks through 3 key patterns, among many, to setting up a successful deployment pipeline."
date:   2016-01-25
author: Ken Mugrage
index-image-url: screenshots/deployment-pipeline-patterns/dzone_cd_guide.png
index-image-alt: 'dzone cd guide'
keywords: snap ci, gocd, continuous delivery, deployment pipeline, ken mugrage, blue green
categories: deployments
---

Teams have been automating the build, test and deploy processes of their software for many years, but usually in a very specific "one off" manner. This piece walks through 3 key patterns, among many, to setting up a successful deployment pipeline.

## Build Things Once

When you're taking software from initial code change to production in an automated fashion, it's important that you deploy the exact same thing you've tested.

Once you've built a binary, you should upload that back to your CD server or artifact repository for later use.

When you're ready to deploy to a downstream system, you should fetch and install that build artifact. This way you make sure that you're running your functional tests on the exact thing you built and that you're going to deploy.

## Verify on a Production-Like Environment

Ideally you should be staging and testing on the same set up. If your staging and production environments are exactly the same, you can use them interchangeably in a blue/green deployment pattern. It's hard to imagine a better way to be sure your software will work when you turn users loose on it.

If you're deploying to very large environments where it's just not practical to have an exact duplicate, you should still duplicate the technology. In other words, if your web application runs on a cluster of 1,000 machines in production, you should verify that application on a cluster, even if it's only two machines.

One of the reasons people are so excited about advancements in container technology is the help they provide for this.

## Non-Linear Workflows

It's not uncommon for teams to practice Continuous Integration on small parts of code before "throwing it over the wall" to someone else to put together with other parts of the application. In these cases, it's usually OK to set up a linear flow. The entire thing might run end to end in just a few minutes.

Don't feel like you need to emulate the often seen business diagram of pipelines that show unit-test, then functional tests, then security tests etc. The purpose of those diagrams is to show intent, not implementation. If you're able, run as much as possible at the same time for faster feedback.

This article originally appeared in the DZone Continuous Delivery Guide. You can download the guide for free [here](https://dzone.com/guides/continuous-delivery-3).
