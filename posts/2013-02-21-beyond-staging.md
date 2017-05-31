---
layout: post
title:  "Beyond Staging"
description: A working build pipeline means that you are guaranteed that the exact same set of git commits that trigger a stage also trigger the following ones.
date:   2013-02-21
author: Badrinath Janakiraman
keywords: snap ci, continuous delivery, continuous integration, developer tools, github
categories: deployments feature
---

When you start out with your project, it would have been sufficient for you to push your application out to a single environment. You may have called it development or staging and deployed every build that passed your tests onto it. However, soon enough, as your application grows, there is typically a need for multiple environments between the first one and what you may call production. At the very least, you will need to have a staging and a production environment, if not more (UAT, QA?).

When faced with this one recognizes the full value of a build pipeline. A working build pipeline means that you are guaranteed that the exact same set of git commits that trigger a stage also trigger the following ones. Not the first commit in the set, not the last, not a random member of the set - and certainly not the HEAD of the branch at the current point in time, which might have already changed.

In the last couple of weeks, we have added the ability to configure multiple deployments in Snap. Deployments are configured to run as a sequence of stages, one after the other, in your build pipeline. Under ideal circumstances, it is easy to argue that every commit should go directly to production. However, in reality:
Not every environment needs to get updated on every change. For example, someone that is testing your application or demoing it to a customer does not want things changing under them as they are doing it. They might very well want every change, but at their own pace and in their own time.
While using feature toggles to keep all work on a single branch and enabling continuous deployment might work for larger features, to implement this for every story and feature regardless of size or value may not be a the best use of your time.
So in addition to having the ability to configure multiple deployments, you would also need the ability to trigger deployments automatically in sequence or manually on demand.

When combined these two features form a core part of creating a build pipeline in Snap.

{% youtube XofmSr2WyAo 480 320 %}

 
Snap CI © 2017, ThoughtWorks
